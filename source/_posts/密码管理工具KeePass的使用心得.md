title: 密码管理工具KeePass的使用心得
author: lily blessing
date: 2021-07-07 23:27:58
tags:
---
# 前言
关于密码的管理，以前一直用的chrome浏览器自带的密码管理功能；但总有一些网站在注册时，chrome做不到智能的识别，这时就需要人自己现场编一个密码并且记住；这样做带来的后果就是密码的重复度会很高，而且容易遗忘、管理困难。同时，现场编密码对于一个有起名困难症的人来说是一种煎熬。~~(咕,杀了我吧)~~  
在饱受自己想密码的苦痛折磨后，我开始寻找一款合适的密码管理工具，然后我发现了KeePass。这是一款跨平台开源免费的密码管理工具。

![start](http://oss.lilyblessing.xyz/2:/img/pid=90877153.webp)

## 所需的工具和项目
- 软件本体 [KeePass](https://keepass.info/)
- Chrome浏览器扩展 [Kee - Password Manager](https://chrome.google.com/webstore/detail/kee-password-manager/mmhlniccooihdimnnjhamobppdhaolme)
- Chrome扩展的辅助插件 [KeePassRPC](https://github.com/kee-org/keepassrpc/releases)
- 安卓端同步管理工具 [Keepass2Android](https://play.google.com/store/apps/details?id=keepass2android.keepass2android&hl=zh&gl=US)
- 同步数据库用网盘 随意 我的选择是 [OneDrive](https://onedrive.live.com/) 免费个人同步空间5G足够使用

# 使用流程

## Windows端

下载软件后，打开软件，可以先点【Cancel】取消。不去新建数据库，我们把界面先汉化了再说。
### 汉化
首先我们先处理界面语言，按Alt+V+L可以打开语言选择界面对话框 。
![SelectLang](http://oss.lilyblessing.xyz/2:/img/SelectLang.webp)
点击左下方的【获取更多语言...】选择简体中文语言文件下载，点击【打开文件夹】将刚刚下载的语言文件放入，然后重启软件切换语言。

### 新建数据库
新建数据库，可以将数据库文件存放在Onedrive网盘的同步目录下，方便后期多终端调用。  
![newDatabases](http://oss.lilyblessing.xyz/2:/img/newDatabases.webp)
设置数据库密码，可以同时使用三种密码验证方案；密码、密钥验证与win账号验证是AND关系，缺一不可。如果同时勾选三种验证，那么只要有一个验证不通过就打不开数据库，更安全但是不太方便，一般我们使用密码验证就可以了。
![S_3PasswordMethon](http://oss.lilyblessing.xyz/2:/img/S_3PasswordMethon.webp)
然后使用默认配置，点【OK】即可
![S_DefConf](http://oss.lilyblessing.xyz/2:/img/S_DefConf.webp)
注意，数据库密码一定要牢记，如果不放心可以在这一步选择打印（没有打印机就找个小本本写上你的数据库密码，其实有了密码管理工具只需要记一个主密码应该还是不难的），一般这里选择跳过即可。
![S_Skip](http://oss.lilyblessing.xyz/2:/img/S_Skip.webp)
至此已经可以使用本软件生成随机密码。  
Ctrl+i可以直接新增一条记录，Ctrl+S保存。当然仅仅是装好软件还是不太够的；平常最主要的使用场景是浏览器使用和移动端使用。

### 浏览器扩展
#### 浏览器扩展的安装与配置
安装浏览器扩展后，下载辅助插件，将其放入Keepass的插件文件夹，按Alt+T+P可以打开【插件管理器】，点击【打开文件夹】可以打开插件文件夹，将下载的RPC放入，重启软件。  
当软件重启完成浏览器会自动开始接入数据库，
![RPC01](https://www.softzone.es/app/uploads-softzone.es/2020/01/Kee-vincular-con-KeePass.jpg)
输入KeePass本体弹出的验证窗口中的红色代码即可确认链接。
![RPC02](https://www.softzone.es/app/uploads-softzone.es/2020/01/Clave-conexi%C3%B3n-Kee-KeePass.jpg)
链接成功后浏览器扩展会提示选择使用【新的数据库】还是使用【现有数据库】，这里选择现有数据库
![RPC03](https://www.softzone.es/app/uploads-softzone.es/2020/01/KeePass-Usar-base-de-datos.jpg)
至此浏览器扩展就安装完成了。

#### 浏览器扩展的使用方法
当我们再某个网站注册或者输入信息登陆以后，可以点击浏览器扩展Kee的图标，弹出创建记录的对话框
![S_SavePwd](http://oss.lilyblessing.xyz/2:/img/S_SavePwd.webp)
这里点一下【+】创建新记录，会自动将刚刚输入的表单数据填上；这里删掉验证码的那行表单，然后点【NEXT】。
![S_SavePwd02](http://oss.lilyblessing.xyz/2:/img/S_SavePwd02.webp)
然后选择一个群组路径保存这条信息记录，这里我选择放在我创建的【网站账号】群组中，点击【保存】
![S_SavePwdPathSelect](http://oss.lilyblessing.xyz/2:/img/S_SavePwdPathSelect.webp)
这样下次打开网站需要登陆的时候，点一下登陆的框框旁的Kee图标，就可以看到上次记录的信息了。
![S_loginView](http://oss.lilyblessing.xyz/2:/img/S_loginView.webp)

### 同步
此时，由于新增记录，数据库中已经存在改动，Onedrive会自动上传你的改动；但如果在其他终端(比如手机)也做了改动，OneDrive就会判断文件出现冲突无法完成合并。  
此时，可以通过选择【文件】-->【同步】-->【与文件同步...】，选择Onedrive中的同步文件，【同步】合并数据改动。
![Sync](http://oss.lilyblessing.xyz/2:/img/Sync.webp)

---

Windows端还有其他功能，比如软件界面的自动填充也可以配置
![S_softwareAutoLogin](http://oss.lilyblessing.xyz/2:/img/S_softwareAutoLogin.webp)
这里就不过多赘述，请读者自行摸索。

## Android端

Android端的选择是Keepass2Android，打开之前Onedrive中同步的数据库文件即可。在手机的系统设置中搜索自动填充服务，设定为Keepass2Android即可接管手机的密码填充。记得加入电池优化白名单。  
至此结束。

![end](http://oss.lilyblessing.xyz/2:/img/pid=90743556.webp)