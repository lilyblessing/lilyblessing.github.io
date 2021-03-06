title: 配置tmux的一次记录
author: lily blessing
date: 2021-07-05 02:01:48
tags:
---
# Tmux的安装与配置记录
##  Tmux是什么？
Tmux 是一个终端复用器（terminal multiplexer），属于常用的开发工具，用于终端会话进程的配置与分离，也就是起到一个类似于虚拟终端的效果；类似功能的还有screen，这两个工具我都有在使用，我是由screen转到tmux的。  
![PID90581839](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/98f0d741113770a71e86d71134617b13-63f5c3.webp)
### 什么是会话？  
命令行的典型使用方式是，打开一个终端窗口（terminal window，以下简称"窗口"），在里面输入命令。用户与计算机的这种临时的交互，称为一次"会话"（session） 。  
会话的一个重要特点是，窗口与其中启动的进程是连在一起的。打开窗口，会话开始；关闭窗口，会话结束，会话内部的进程也会随之终止，不管有没有运行完。  
一个典型的例子就是，SSH 登录远程计算机，打开一个远程窗口执行命令。这时，网络突然波动导致断线，再次登录的时候，是找不回上一次执行的命令的。因为上一次 SSH 会话已经终止了，里面的进程也随之消失了。  
为了解决这个问题，会话与窗口可以"解绑"：窗口关闭时，会话并不终止，而是在后台继续运行，等到以后需要的时候，再让会话"绑定"其他窗口。
### Tmux 的作用
Tmux 就是会话与窗口的"解绑"工具，将它们彻底分离。

1. 它允许在单个窗口中，同时访问多个会话。这对于同时运行多个命令行程序很有用。
2. 它可以让新窗口"接入"已经存在的会话。
3. 它允许每个会话有多个连接窗口，因此可以多人实时共享会话。
4. 它还支持窗口任意的垂直和水平拆分。

Screen也是类似的作用，不过在细节方面Tmux更胜一筹；screen如果在一个窗口中再开窗口就会陷入嵌套窗口的状况，而Tmux则无这种特性。
## Tmux的配置
### 安装
以CentOS为例
``` bash
sudo yum install tmux
```
### 常用命令
``` bash
# 新建会话 
tmux
# 默认直接开启一个新的会话

tmux new -s test
# 新建一个名为test的会话

# 脱离会话 一般是使用快捷键 ctrl+b  d (记得ctl+b完全松手后再按 d)
tmux detach
# 也可以直接输入 tmux detach进行分离

# 查看所有会话
tmux ls

# 进入名为test的会话
tmux a -t test
# 这里的a其实是tmux attach-session，只是平常我们直接简写为a

# 如果已经在窗口中 想要切换也可以直接 ctrl+b  s 进行选择 非常方便快捷

# 重命名会话
tmux rename-session -t 0 <new-name>
# 或者ctrl+b $

#退出删除会话
exit
# 或者ctrl + d

# 列出快捷键
tmux list-keys
# 也可以在窗口中使用ctrl+b  ? 
```