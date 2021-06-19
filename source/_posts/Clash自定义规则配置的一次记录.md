title: Clash自定义规则配置的一次记录
author: lily blessing
date: 2021-06-19 19:46:20
tags:
---
# Clash自定义规则配置
## 前言
体验了一段时间，Clash的分流功能很好用，而且用订阅转换可以很方便的把v2和ssr的订阅链接转换为clash使用。但是也存在一个痛点，个别网站的规则并没有收录于在线规则列表中，需要自己本地手动添加；而添加之后，每次更新订阅之前手动写入的规则就会被覆盖掉。本来打算在转换服务器中添加自定义规则，折腾一番发现clash客户端上自带这种功能。

## 流程
1. 写入自定义规则  
在`Profiles`页面找到需要加入特定规则的配置项，第三个图标即为编辑规则
![editrules](https://oss.lilyblessing.xyz/2:/img/editRules.jpg)
打开后会显示前一百条规则，点击左上角的`add`添加自定义规则
![addrules](https://oss.lilyblessing.xyz/2:/img/addRules.jpg)
DOMAIN-SUFFIX为一级域名匹配，如bilibili.com；  
DOMAIN为全域名匹配，如pan.baidu.com；  
DOMAIN-KEYWORD为域名关键词匹配，如baidu可以匹配pan.baidu.com也可以匹配www.baidu.com；
![CreatNewRule](https://oss.lilyblessing.xyz/2:/img/CreatNewRule.jpg)
编辑完毕后，点击右上角的`Add`加入规则集，然后点击右上角的`Save`写入磁盘。

2. 获取自定义规则代码
在`Profiles`页面找到刚刚加入特定规则的配置项，第一个图标`< >`即为打开规则文件。  
打开后按`Ctrl+F`搜索`rules`，即可定位指规则集部分。  
此时规则从第一行至`  - DOMAIN-SUFFIX,acl4.ssr,🎯 全球直连`的前一行为你的自定义规则条目，将其复制。

3. 预处理自定义规则
打开`Clash - Settings - Profiles - Parsers - Edit`打开预处理规则
之前修改以下代码复制进去
``` bash
parsers: # array
  - url: https://****.***/******  #将这些替换成你的订阅链接
  - url: https://****.***/******
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX,****.com,🚀 节点选择  #将这些替换成你的规则
        - DOMAIN-KEYWORD,****,🚀 节点选择
```
点击右下角保存，然后去更新一下订阅的配置文件。  
可以点击`editRules`检查一下规则是否确实被写入了。

