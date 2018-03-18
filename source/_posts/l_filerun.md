---
title: 笔记-在线文件管理filerun
date: 2018-03-11 12:21:17
tags: [Linux,VPS,Learn,PHP]
---

[TOC]
@[TOC]

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1859313&auto=1&height=66"></iframe>

# 安装

## 环境

0. 宝塔面板（推荐新手使用）

   ```bash
   #Centos系统
   yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh

   #Ubuntu系统
   wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && sudo bash install.sh

   #Debian系统
   wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && bash install.sh
   ```


1. LNMP全家桶

2. PHP安装拓展：iconCube, imagemagick, exif，同时删除禁用参数中的 exec，然后重启PHP

3. 安装ffmpeg

   ```
   ##Ubuntu 16.04+
   apt-get update
   apt-get install ffmpeg

   ##debian 8
   vim /etc/apt/sources.list
   #添加四个软件源
   deb http://www.deb-multimedia.org jessie main non-free
   deb ftp://ftp.deb-multimedia.org jessie main non-free
   deb http://www.deb-multimedia.org stable main non-free
   deb ftp://ftp.deb-multimedia.org stable main non-free
   #更新并安装
   apt-get -y update
   apt-get -y install ffmpeg

   ##CentOS
   #到http://www.ffmpeg.org/releases/下载最新版本
   #编译安装
   wget http://www.ffmpeg.org/releases/ffmpeg-*.*.tar.gz
   tar -zxvf ffmpeg-*.*.tar.gz
   cd ffmpeg-*.*
   ./configure
   make
   make install
   ```

   ​

## 搭建网站

1. 从官网：http://www.filerun.com/download 下载对应PHP版本的压缩包，解压到网站根目录。

   ```bash
   #以下为举例,以具体链接和文件为准
   wget FileRun_**.zip
   unzip FileRun_**.zip

   ```

2. 创建并修改filerun存储目录权限

   ```bash
   chmod 777 -R /www/wwwroot/你绑定的站点域名/system/data
   mkdir /www/wwwroot/你绑定的站点域名/system/data/default_home_folder
   ```

3. 访问 网站/index.html , 按照FileRun向导初始化

   - 检查运行环境，有一两项不是 OK 也可以继续。
   - 设置数据库信息
   - 用分配好的超级用户登录，然后修改密码



## 功能实现

###  客户端

Android：Filerun

需要网站配置ssl，然后勾选Control Panel-Security-API-Enable API




### 图片预览

PHP安装拓展：ImageMagick, FFmpeg，exif

安装pngquant，获得更多图像支持

```bash
git clone git://github.com/pornel/pngquant.git
cd pngquant
make
sudo make install
```

查看安装目录

```bash
#两种皆可
whereis imagemagick
find / -name pngquant
```

勾选Control Panel-Files-Image preview对应选项



### webdav功能

Filerun自带webdav功能，链接：https://网站/dav.php/@Home。

如果不加 `@Home`，会还需要再进一个目录，故添加。



### 界面汉化

在Control Panel->Interface options里可设置。

