title: Youtube直播录制与下载的配置记录
author: lily blessing
date: 2021-07-05 12:54:27
tags:
---
# 前言
由于平常看的一些youtuber的直播或者歌回容易触发youtube版权大棒，导致无法留档；  
由此萌生了在个人VPS上部署实时录制youtube直播和youtube视频下载的工具。

![占位1](https://oss.lilyblessing.xyz/2:/img/pid90402323.webp)
## 所需的工具和项目
 - 录制工具: [liverecord](https://github.com/lovezzzxxx/liverecord)  
 - 下载工具: [youtube-dl](https://github.com/ytdl-org/youtube-dl)  
 - 下载工具: [you-get](https://you-get.org/) 

# 直播录制

## 部署安装

这里用的是Liverecord这个脚本,安装以centOS为例:
``` bash
# 首先安装自动录制脚本
mkdir record 
wget -O "record/record_new.sh" "https://github.com/lovezzzxxx/liverecord/raw/master/record_new.sh" 
chmod +x record/record_new.sh

# 然后编译安装ffmpeg
# 编译耗时较长,建议使用screen或者Tmux等工具放入后台运行
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
cd ffmpeg
./configure
make
make install
cp ffmpeg /usr/bin/ffmpeg

# 安装Streamlink
pip3 install streamlink

# 安装rclone 用于配置上传
curl https://rclone.org/install.sh | sudo bash
# 配置rclone  具体配置看其他教程
rclone config
```
![占位2](https://oss.lilyblessing.xyz/2:/img/pid=90915922.webp)
## 配置使用
``` bash
# 使用方法
cd record
./record_new.sh [-参数 值] 频道类型 频道号码

# 使用默认值进行录制
./record_new.sh youtube "UCJABeTIKrhExlG6tBTov_ZA"
```
|参数|默认值|作用|
|:---|:---|:---|
|--nico-id|无|nico用户名|
|--nico-psw|无|nico密码|
|--you-cookies|无|youtube监测cookies文件,需要配合--you-config参数使用|
|--you-config|无|youtube录制配置文件,仅支持youtube频道类型|
|--bili-config|无|bilibili录制配置文件,仅bilibili频道类型|
|--bili-cookies|无|bilibili录制cookies文件,仅支持bilibiliy频道类型|
|--bili-proxy|无|bilibili录制代理|
|--bili-proxy-url|无|bilibili录制代理获取链接|
|-f/--format|best|清晰度|
|-p/--part-time|0|分段时间，0为不分段|
|-l/--loop-interval|60|检测间隔|
|-ml/--min-loop-interval|180|最短录制间隔|
|-ms/--min-status|1|开始录制前需要持续检测到开播次数|
|-o/-d/--output/--dir|record_video/other|输出目录|
|-u/--upload|无|上传网盘,格式为网盘类型重试次数:盘符:路径，网盘类型支持rclone paidupcs onedrive，例如rclone3:vps:record|
|-dt/--delete-type|1|删除本地录像需要成功上传的数量，默认为1，del为强制删除，keep为强制保留，all为需要全部上传成功|
|-e/--except|无|排除转播，格式同录制频道，如-e youtube "UCWCc8tO-uUl_7SJXIKJACMw|
``` bash
# 使用自定义参数进行录制
./record_new.sh --you-cookies "you-cookies.txt" --you-config "you-config.txt" -f best -l 300 -o "record_video/blanc" -u rclone3:vps:data youtube "UCJABeTIKrhExlG6tBTov_ZA"

# 使用youtubeCookie录制最高清晰度，在直播间间隔300秒检测一次是否开播；
# 将文件输出至当前目录的record_video/blanc文件夹下；
# 录制完毕后上传至rclone配置的vps主机内的data文件夹下，如果失败，自动重试3次。
```
由于会一直占用终端, 建议配合screen或Tmux使用

---

# 录播下载
Youtube的下载工具主流就那几种  
windows平台可以使用浏览器脚本配合idm下载, linux则可以使用youtube-dl或者you-get。

## 部署安装
以下以CentOS为例
```bash
# 安装youtube dl
yum install youtube-dl
# 或者
pip install --upgrade youtube-dl
# 或者
curl https://yt-dl.org/latest/youtube-dl -o /usr/local/bin/youtube-dl

# 安装you-get
pip3 install you-get
```
![占位3](https://oss.lilyblessing.xyz/2:/img/S_88062650_p0.webp)
## 配置使用
```bash
# Youtube-dl 最简单的使用方式,默认直接放youtube地址
youtube-dl https://www.youtube.com/watch?v=iKCQ5fyEpU8

# 或者也可以设置清晰度之类的参数
youtube-dl https://www.youtube.com/watch?v=iKCQ5fyEpU8 -F
# -F 查看所有清晰度,格式 -f 根据编号选择清晰度格式

youtube-dl https://www.youtube.com/watch?v=iKCQ5fyEpU8 -f best
# 可以直接设置best,直接下载油管推荐的当前清晰度最好的音视频轨合一文件

# 但是best无法下载2K4K以上的高清视频,可以将音视频轨分开下载
youtube-dl https://www.youtube.com/watch?v=iKCQ5fyEpU8 -f bestvideo[height=1440]+bestaudio
# 这个例子为同时下载限定视频轨清晰度高度最高为1440(也就是2K清晰度)加上音质最好的音频轨文件
# 然后调用ffmpeg合并
```
每次下载都要手动添加这一串配置实在是太麻烦了，所以我们可以添加一个配置文件。Linux的配置文件应该放在`/etc/youtube-dl.conf`下,如何编写可以参考[官网给出的配置](https://github.com/ytdl-org/youtube-dl#configuration)。
以下是我的示例,仅供参考
``` bash
# Lines starting with # are comments
 
# Use this proxy
--proxy 127.0.0.1:1089
 
# Save all videos under Movies directory in your home directory
-o /home/data/Download/%(title)s.%(ext)s
 
# Use this prefix for unqualified URLs.
# For example "gvsearch2:" downloads two
# videos from google videos for youtube-
# dl "large apple". Use the value "auto"
# to let youtube-dl guess ("auto_warning"
# to emit a warning when guessing).
# "error" just throws an error. The
# default value "fixup_error" repairs
# broken URLs, but emits an error if this
# is not possible instead of searching.
--default-search "ytsearch"
 
# Keep the video file on disk after the
# post-processing; the video is erased by
# default
--keep-video

# 设置下载视频的默认画质
-f bestvideo[height=1440]+bestaudio
```
设置好配置文件后，以后就直接下载就行了, 到这里就全部完成了