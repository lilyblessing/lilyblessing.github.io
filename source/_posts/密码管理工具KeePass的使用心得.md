title: 密码管理工具KeePass的使用心得
author: lily blessing
date: 2021-07-07 23:27:58
tags:
---
# 前言
关于密码的管理，以前一直用的chrome浏览器自带的密码管理功能；但总有一些网站在注册时，chrome做不到智能的识别，这时就需要人自己现场编一个密码并且记住；这样做带来的后果就是密码的重复度会很高，而且容易遗忘、管理困难。同时，现场编密码对于一个有起名困难症的人来说是一种煎熬。~~(咕,杀了我吧)~~  
在饱受自己想密码的苦痛折磨后，我开始寻找一款合适的密码管理工具，然后我发现了KeePass。这是一款跨平台开源免费的密码管理工具。

![start](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/4f440d805478d9748967f220257a4bbd-c95267.webp)

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
![SelectLang](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/e1119c63324ff01b1399feb054dfd4e5-197844.webp)
点击左下方的【获取更多语言...】选择简体中文语言文件下载，点击【打开文件夹】将刚刚下载的语言文件放入，然后重启软件切换语言。

### 新建数据库
新建数据库，可以将数据库文件存放在Onedrive网盘的同步目录下，方便后期多终端调用。  
![newDatabases](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/1e24c0f320e90ff070289bdab25442c2-78a7a6.webp)
设置数据库密码，可以同时使用三种密码验证方案；密码、密钥验证与win账号验证是AND关系，缺一不可。如果同时勾选三种验证，那么只要有一个验证不通过就打不开数据库，更安全但是不太方便，一般我们使用密码验证就可以了。
![S_3PasswordMethon](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/f46eb9a8a7e87cab6bff6880738123c7-94ab12.webp)
然后使用默认配置，点【OK】即可
![S_DefConf](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/b3f508c1a755e6162261a1a94d3c8ad3-d77d29.webp)
注意，数据库密码一定要牢记，如果不放心可以在这一步选择打印（没有打印机就找个小本本写上你的数据库密码，其实有了密码管理工具只需要记一个主密码应该还是不难的），一般这里选择跳过即可。
![ebf5958fdd508af4708d9f02a82ce748-f1e2c4](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/ebf5958fdd508af4708d9f02a82ce748-f1e2c4.webp)
至此已经可以使用本软件生成随机密码。  
Ctrl+i可以直接新增一条记录，Ctrl+S保存。当然仅仅是装好软件还是不太够的；平常最主要的使用场景是浏览器使用和移动端使用。

### 浏览器扩展
#### 浏览器扩展的安装与配置
安装浏览器扩展后，下载辅助插件，将其放入Keepass的插件文件夹，按Alt+T+P可以打开【插件管理器】，点击【打开文件夹】可以打开插件文件夹，将下载的RPC放入，重启软件。  
当软件重启完成浏览器会自动开始接入数据库，
![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200328-97ac61b8fe1b66b5692ba209217c165e-1b0001.webp)
输入KeePass本体弹出的验证窗口中的红色代码即可确认链接。
![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200328-0be937adb3967a99205a12b824185fba-abf3ea.webp)
链接成功后浏览器扩展会提示选择使用【新的数据库】还是使用【现有数据库】，这里选择现有数据库
![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200328-df03b33fb74589330cd14d074f634a9b-7babe8.webp)
至此浏览器扩展就安装完成了。

#### 浏览器扩展的使用方法
当我们再某个网站注册或者输入信息登陆以后，可以点击浏览器扩展Kee的图标，弹出创建记录的对话框
![6649f3e65764a7c7bae99d0a9224ff33-af5a82](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/6649f3e65764a7c7bae99d0a9224ff33-af5a82.webp)
这里点一下【+】创建新记录，会自动将刚刚输入的表单数据填上；这里删掉验证码的那行表单，然后点【NEXT】。
![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200711-72a28f34943414d4b62b2859818d18ac-8acf30.webp)
然后选择一个群组路径保存这条信息记录，这里我选择放在我创建的【网站账号】群组中，点击【保存】
![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200711-dad5588771f576cbd472812c325d816a-864af3.webp)
这样下次打开网站需要登陆的时候，点一下登陆的框框旁的Kee图标，就可以看到上次记录的信息了。
![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200711-20dfbc56244545000da31a6a3cf9af32-179254.webp)

### 同步
此时，由于新增记录，数据库中已经存在改动，Onedrive会自动上传你的改动；但如果在其他终端(比如手机)也做了改动，OneDrive就会判断文件出现冲突无法完成合并。  
此时，可以通过选择【文件】-->【同步】-->【与文件同步...】，选择Onedrive中的同步文件，【同步】合并数据改动。
![Sync](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200711-f35c67fb3ae1ce76c7e4477c73a4b84a-da961c.webp)

---

Windows端还有其他功能，比如软件界面的自动填充也可以配置
![Autofill](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200711-16731714b5a20ea3469230e7f3957727-94a1e4.webp)
这里就不过多赘述，请读者自行摸索。

## Android端

Android端的选择是Keepass2Android，打开之前Onedrive中同步的数据库文件即可。在手机的系统设置中搜索自动填充服务，设定为Keepass2Android即可接管手机的密码填充。记得加入电池优化白名单。  
至此结束。

---

后话：在使用过一段时间的原版Keepass后，偶然接触到了windows第三方Keepass版本[KeepassXC](https://keepassxc.org/)。相比于初代版本的keepass，KeepassXC界面更为美观，在各个平台的数据库合并同步整理也相比原版更加简单方便。

![](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/210153-82d111a15ae37eb2768475b27986ae01-e5f870.png)

Android端则推荐第三方的[KeepassDX](https://www.keepassdx.com/)，同样也是界面更美观、同步更方便、高效。如果你对原版keepass功能方面有些不满，或者审美疲劳了，想尝试一下新的应用程序；以上的两款应用是一个不错的选择。

![](https://www.keepassdx.com/assets/img/device.png)

![end](https://raw.githubusercontent.com/lilyblessing/img/main/20211018/200711-5d918b7257dc9d64eb8735f56986415e-53601d.webp)