title: Clash自定义规则配置的一次记录
author: lily blessing
date: 2021-06-19 19:46:20
tags:
---
# Clash自定义规则配置
## 前言
体验了一段时间，Clash的分流功能很好用，而且用订阅转换可以很方便的把v2和ssr的订阅链接转换为clash使用。但是也存在一个痛点，个别网站的规则并没有收录于在线规则列表中，需要自己本地手动添加；而添加之后，每次更新订阅之前手动写入的规则就会被覆盖掉。本来打算在转换服务器中添加自定义规则，折腾一番发现clash客户端上自带这种功能。

## 流程
### 1. 写入自定义规则  
在`Profiles`页面找到需要加入特定规则的配置项，第三个图标即为编辑规则
![editrules](https://oss.lilyblessing.xyz/2:/img/editRules.jpg)
打开后会显示前一百条规则，点击左上角的`add`添加自定义规则
![addrules](https://oss.lilyblessing.xyz/2:/img/addRules.jpg)
DOMAIN-SUFFIX为一级域名匹配，如bilibili.com；  
DOMAIN为全域名匹配，如pan.baidu.com；  
DOMAIN-KEYWORD为域名关键词匹配，如baidu可以匹配pan.baidu.com也可以匹配www.baidu.com；
![CreatNewRule](https://oss.lilyblessing.xyz/2:/img/CreatNewRule.jpg)
编辑完毕后，点击右上角的`Add`加入规则集，然后点击右上角的`Save`写入磁盘。

### 2. 获取自定义规则代码
在`Profiles`页面找到刚刚加入特定规则的配置项，第一个图标`< >`即为打开规则文件。  
打开后按`Ctrl+F`搜索`rules`，即可定位指规则集部分。  
此时规则从第一行至`  - DOMAIN-SUFFIX,acl4.ssr,🎯 全球直连`的前一行为你的自定义规则条目，将其复制。

### 3. 预处理自定义规则
打开`Clash - Settings - Profiles - Parsers - Edit`打开预处理规则
之前修改以下代码复制进去  
- 这是单个配置文件的情况
``` bash
parsers: # array
  - url: https://****.***/******  #将这些替换成你的订阅链接
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX,****.com,🚀 节点选择  #将这些替换成你的规则
        - DOMAIN-KEYWORD,****,🚀 节点选择
```
- 如果有多个配置文件，并且订阅链接的域是一样的话，可以使用正则表达式
``` bash
parsers: # array
  - reg: https://abcd.xyz.+$  #将abcd.xyz替换成你的订阅链接的相同部分
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX,****.com,🚀 节点选择  #将这些替换成你的规则
        - DOMAIN-KEYWORD,****,🚀 节点选择
```

点击右下角保存，然后去更新一下订阅的配置文件。  
可以点击`editRules`检查一下规则是否确实被写入了。

### 4. 进阶配置
这部分主要讲一些略微进阶的配置，具体都可以在github的clash项目[**wiki**](https://docs.cfw.lbyczf.com/contents/configfile.html)上查到。  

#### 配置dns方面：  
打开Clash的主界面 切换至**`Settings`**选项卡，然后找到**`Profile Mixin`**选项，`Type`选择`YAML`，然后点击YAML行的**`edit`**  
输入以下代码并保存:  
```
mixin: # object
  dns:
    enable: true
    enhanced-mode: redir-host
    nameserver:
      - 114.114.114.114
      - 223.5.5.5
      - 1.2.4.8
      - tls://dns.rubyfish.cn:853
      - https://dns.alidns.com/dns-query
    fallback:
      - tls://1.0.0.1:853
      - tls://8.8.4.4:853
  tun:
    enable: true
    stack: gvisor
    dns-hijack:
      - 198.18.0.2:53
    macOS-auto-route: true
    macOS-auto-detect-interface: true

```

这样就配置好了

---

#### 添加负载均衡组：
打开Clash的主界面 切换至`**Settings**`选项卡，然后找到`**Profiles**`下的`**Parsers**`选项，然后点击`**edit**`  
自行更改以下代码保存：
```
parsers: # 
  - reg: https://****.***/******  #将这些替换成你的订阅链接
    yaml:
      append-proxy-groups:  #添加自定义负载均衡组
        - name: 🔮 HK负载均衡
          type: load-balance
          url: http://www.gstatic.com/generate_204
          interval: 180
          proxies:
            - DIRECT
        - name: 🔮 TW负载均衡
          type: load-balance
          url: http://www.gstatic.com/generate_204
          interval: 180
          proxies:
            - DIRECT
      prepend-rules:  #添加自定义规则
        - DOMAIN-SUFFIX,****.com,🚀 节点选择 
        - DOMAIN-KEYWORD,****,🚀 节点选择	
      commands:
        - proxy-groups.🔮 HK负载均衡.proxies=[]proxyNames|HK|香港 # 向🔮 HK负载均衡策略组添加所有定义的节点名，并按“HK|香港”正则表达式过滤
        - proxy-groups.🔮 TW负载均衡.proxies=[]proxyNames|台湾 # 向🔮 HK负载均衡策略组添加所有定义的节点名，并按“台湾”正则表达式过滤
        - proxy-groups.🚀 节点选择.proxies.8+🔮 HK负载均衡 # 向🚀 节点选择策略组添加组名🔮 HK负载均衡 proxies.8是从指 节点选择 组中从0开始数的第八位 这里意思是在第八位插入这个🔮 HK负载均衡组
        - proxy-groups.🚀 节点选择.proxies.9+🔮 TW负载均衡 # 向🚀 节点选择策略组添加组名🔮 TW负载均衡
```
修改保存后，更新订阅就能看到新加入的负载均衡组了。

这里介绍一下clash的几种策略组：  
策略组有：`延迟最低`、`故障转移`、`手动选择`、`负载均衡` 四种模式。
- 延迟最低：`url-test`，也就是自动选择默认组，顾名思义，每隔一段时间进行延迟测试，选择延时最低的节点。他有几个参数我稍微提一下
	1. `url:http://www.gstatic.com/generate_204` 这里是指延迟测试的网站，一般保持默认即可
    2. `interval: 300` 这里是指自动测试、切换节点的间隔时间 如果觉得每次切换太慢，可以适当调整
    3. `tolerance: 50` 容忍度，指测试出来一组节点中，延迟相差不太大的时候怎么处理；比如容忍度50，当前选择节点150ms，节点测速中只有更进一步低于50，也就是小于100ms时才会切换节点，避免频繁切换造成连接打断
- 故障转移：每次都选组内的第一个节点，如果无法使用在切到第二个，依此类推。
- 手动选择：字面意思，自己手选点一个
- 负载均衡：每次连接都随机到组里挑一个节点，如果那个节点故障了，就再随机挑一个；所以也有自动选择组中的一些参数。

---

以上，暂时先讲这么多；后续还有啥没说的话，就想起来了再补。

![End](https://oss.lilyblessing.xyz/2:/img/pid=90325048.webp)
