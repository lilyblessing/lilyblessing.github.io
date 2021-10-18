title: 配置Mosh的一次记录
author: lily blessing
tags: []
categories: []
date: 2021-06-17 17:48:00
---
## 使用mosh来优化ssh连接

笔者日常使用的是广电网，极其不稳定的网络连接导致一个网络波动就能掉线。
由于SSH采用不可持续的连接，当网络出现波动时，SSH断开会导致当前正在运行的服务中断，对工作产生非常大的影响。
无意间知道了Mosh这个东西，安装使用下，发现网络波动这种事情不会导致服务器连接断开了，特意查了下，原来Mosh使用的是UDP方式传输：虽然也支持使用SSH配置进行认证登录，但是数据传输本身是使用UDP方式的，Mosh支持在会话中断时，不会立即退出，而是启用一个计时器，当网络恢复后会自动连接，同时会延续之前的会话，不会重新开启一个。实在是帮大忙了。

### 安装mosh

#### 1. 在服务器端安装

安装以笔者使用的CentOS7系统为例
``` bash
$ yum install -y epel-release
$ yum install -y mosh 
```

mosh使用的默认端口号UDP的60000-61000，很明显这个范围太大了，不合适也不安全。
我们只放行60000-60010区间的udp端口即可。

- 如果是iptables
``` bash
$ iptables -I INPUT 1 -p udp --dport 60000:60010 -j ACCEPT
```
- 如果是firewall
``` bash 
$ firewall-cmd --permanent --zone=public --add-port=60000-60010/udp
```

#### 2. 在客户端使用

- 如果默认端口号22没有改变，则只需要将原ssh改为mosh，mosh会自动接管ssh的配置(因此原ssh所用的端口号不能关闭)

``` bash
$ mosh root@192.168.**.**
```

- 如果ssh默认端口号改变了，就需要指定端口号，这里ssh端口号为123

``` bash
$ mosh --ssh="ssh -p 123" root@192.168.**.**
```

- 只开启了一个或者几个mosh端口，需要指定端口号mosh端口号

``` bash
$ mosh -p 60010 --ssh="ssh -p 123" root@192.168.**.**
```

- 不用密码使用秘钥登录 需要将服务器与本机的密钥的目录写好

``` bash 
mosh -p 60010 --ssh="/usr/bin/ssh -i /home/ivo/.ssh/id_rsa" root@192.168.**.**
```

#### 这里推荐一个终端 [FluentTerminal](https://github.com/felixse/FluentTerminal)

这个终端采用fluent设计，界面很美观，而且原生支持mosh连接，感兴趣的朋友可以自行安装体验
![FluentTerminal](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/524be333a47eae8969b395f59c9151a5-060c8f.png)

![FluentSettings](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/aed501b0d68152164588d42b32ca4919-6c4cfc.png)