title: OneDrive备份目录问题
author: lily blessing
date: 2021-10-24 13:32:54
tags:
---
# 前言
由于工作需求，需要在几台电脑上同步工作区而开始找同步盘。然后直接盯上了Windows系统自带的Onedrive，虽然免费版只有5G的同步空间，但对于工作同步目录已经绰绰有余。

不知道微软出于何种考虑只能同步限定的几个目录，无法向同类竞品那般自定义同步目录，搜索了一下大家的解决方案都是用mklink命令处理。我实际使用了一些效果还可以接受，就记录一下步骤。

![akinecoco987-1446779424771231745-20211009_180749-img2](https://cdn.jsdelivr.net/gh/lilyblessing/img@main/20211024/akinecoco987-1446779424771231745-20211009_180749-img2.5av4o3htqys0.webp)

## 创建符号链接流程

### 符号链接

``` bash
mklink [/d][/h][/j] link target
/d 创建目录符号链接。默认为文件符号链接。
/h 创建硬链接而非符号链接。
/j 创建目录链接。
link 指定新的符号链接名称
target 制定新链接引用的路径
```

1. 确定自己Onedrive盘所在路径。

   1. 打开Onedrive文件夹。![打开文件夹](https://cdn.jsdelivr.net/gh/lilyblessing/img@main/20211024/打开文件夹.1yv4bhzfomio.webp)
   2. 随意选择一个文件右键查看属性—位置。![打开路径](https://cdn.jsdelivr.net/gh/lilyblessing/img@main/20211024/打开路径.2pvg9tpmd2q0.webp)

2. 确定自己需要同步的工作路径，比如 `E:\workspace`

3. 创建软链接

   1. 新建的Onedrive符号链接地址`C:\Users\lilyB\OneDrive\workspace`
   2. 工作目录地址`E:\workspace`

   ``` bash
   mklink /j "C:\Users\lilyB\OneDrive\workspace" "E:\workspace"
   ```

   注意创建符号链接时，OneDrive目录不能存在跟新建的符号同名的文件或者目录。

这样符号链接就创建完成了，之后该文件夹就会上传至OneDrive。

但这样还会有一个问题：当第一次文件夹同步完成以后，以后做的文件修改不会马上同步，会在OneDrive中显示同步挂起状态。

## 解决同步挂起问题

这是由于Onedrive目录下所检测的只是一个软连接符号，而非真正目录。所以同步会有延迟，并非是完全不同步。对于需要马上同步的较小工作目录而言，这是致命伤；但是有大佬给出了解决方案。

**反其道而行之**

逆向创建符号链接就可以了：把工作目录整个搬进OneDrive中，然后在原本的工作目录创建符号链接就可了。

``` bash
mklink /d "E:\workspace" "C:\Users\lilyB\OneDrive\workspace"
```

这样同步问题也能解决了。

![akinecoco987-1446779424771231745-20211009_180749-img1](https://cdn.jsdelivr.net/gh/lilyblessing/img@main/20211024/akinecoco987-1446779424771231745-20211009_180749-img1.qb0k7929sa.webp)
