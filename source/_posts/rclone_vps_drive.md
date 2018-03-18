---
title: '[教程]用Rclone在Linux VPS上挂载网盘使用'
date: 2018-03-04 09:19:53
tags: [VPS,Linux]
---

# [教程]用Rclone在Linux VPS上挂载网盘使用
@[TOC]
## 安装

官网：https://rclone.org

下载地址：https://downloads.rclone.org

``` bash 
#命令行安装
curl https://rclone.org/install.sh | sudo bash
```

## 配置

``` bash
#初始化配置
 rclone config
 
#返回结果如下，这里输入n
Current remotes:

Name                 Type
====                 ====

n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
n/d/r/c/s/q>

#选择需要的网盘：
 1 / Amazon Drive
   \ "amazon cloud drive"
 2 / Amazon S3 (also Dreamhost, Ceph, Minio)
   \ "s3"
 3 / Backblaze B2
   \ "b2"
 4 / Box
   \ "box"
 5 / Dropbox
   \ "dropbox"
 6 / Encrypt/Decrypt a remote
   \ "crypt"
 7 / FTP Connection
   \ "ftp"
 8 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
 9 / Google Drive
   \ "drive"
10 / Hubic
   \ "hubic"
11 / Local Disk
   \ "local"
12 / Microsoft Azure Blob Storage
   \ "azureblob"
13 / Microsoft OneDrive
   \ "onedrive"
14 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
15 / QingClound Object Storage
   \ "qingstor"
16 / SSH/SFTP Connection
   \ "sftp"
17 / Yandex Disk
   \ "yandex"
18 / http Connection
   \ "http"
```

以onedrive for business为例，选择13，

``` bash
#以下两个默认回车
Microsoft App Client Id - leave blank normally.
client_id> 
Microsoft App Client Secret - leave blank normally.
client_secret>

#这里选择b
Remote config
Choose OneDrive account type?
 * Say b for a OneDrive business account
 * Say p for a personal OneDrive account
b) Business
p) Personal
b/p> 

#因VPS无可视窗口，故选n
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No

#需要Windows下授权，将结果粘贴进来
For this to work, you will need rclone available on a machine that has a web browser available.
Execute the following on your machine:
        rclone authorize "onedrive"
Then paste the result below:

#保存并退出，显示如下即可
#DriveName为自己改的网盘名称，注意简短，后面操作比较方便
Name                 Type
====                 ====
DriveName            onedrive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q>

```

挂载：
``` bash
#挂载为磁盘，加&保持后台运行
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 &

# 错误提示，原因不明，可以忽略不管
NOTICE: One drive root 'DriveName': poll-interval is not supported by this remote

#卸载磁盘
fusermount -qzu LocalFolder
```

## 客户端授权

1. Windows下运行

```bash
#进入目录，CMD或者Power shell运行：
rclone authorize "onedrive"

#选择选择Business版本，并按Y
```

2. 在打开的浏览器中登陆账户
3. 把命令行的access token输出结果复制到VPS中



## 常用命令

[官方文档](https://rclone.org/commands/)

```bash
## 文件上传
rclone copy /home/backup gdrive:backup  # 本地路径 配置名字:谷歌文件夹名字
### 文件下载
rclone copy gdrive:backup /home/backup 
### 列表
rclone ls gdrive:backup
rclone lsl gdrive:backup # 比上面多一个显示上传时间
rclone lsd gdrive:backup # 只显示文件夹
### 新建文件夹
rclone mkdir gdrive:backup
### 挂载
rclone mount gdrive:mm /root/mm &
### 卸载
fusermount -u  /root/mm
 
#清理，删除存储源的垃圾箱或者是文件历史版本，部分存储类型支持
rclone cleanup remote:path
#删除文件，可以使用--(min|max)-size --(min|max)-age等参数来限定范围
rclone delete remote:path
#删除空目录，类似的还有删除只包含空目录的目录的rmdirs命令
rclone rmdir
#删除，这个就是啥都删了
rclone purge remote:path

#同步
rclone sync source:path dest:path
 
#### 其他 ####
#### https://softlns.github.io/2016/11/28/rclone-guide/
rclone config - 以控制会话的形式添加rclone的配置，配置保存在.rclone.conf文件中。
rclone copy - 将文件从源复制到目的地址，跳过已复制完成的。
rclone sync - 将源数据同步到目的地址，只更新目的地址的数据。   –dry-run标志来检查要复制、删除的数据
rclone move - 将源数据移动到目的地址。
rclone delete - 删除指定路径下的文件内容。
rclone purge - 清空指定路径下所有文件数据。
rclone mkdir - 创建一个新目录。
rclone rmdir - 删除空目录。
rclone check - 检查源和目的地址数据是否匹配。
rclone ls - 列出指定路径下所有的文件以及文件大小和路径。
rclone lsd - 列出指定路径下所有的目录/容器/桶。
rclone lsl - 列出指定路径下所有文件以及修改时间、文件大小和路径。
rclone md5sum - 为指定路径下的所有文件产生一个md5sum文件。
rclone sha1sum - 为指定路径下的所有文件产生一个sha1sum文件。
rclone size - 获取指定路径下，文件内容的总大小。.
rclone version - 查看当前版本。
rclone cleanup - 清空remote。
rclone dedupe - 交互式查找重复文件，进行删除/重命名操作。
#### 其他 ####
```

## 自启动

适用于Debian,使用方法:
1.复制内容,注意文本格式(UNIX).  
2.将其重命名为rcloned(只要不是rclone就可以).  
3.填好REMOTE和LOCAL,复制到/etc/init.d文件夹内.  
4.运行初始化命令:bash /etc/init.d/rcloned init  
5.使用 bash /etc/init.d/rcloned start 挂载.  
6.使用 bash /etc/init.d/rcloned stop 卸载.  

```bash
#!/bin/bash
### BEGIN INIT INFO
# Provides:          rclone
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start rclone at boot time
# Description:       Enable rclone by daemon.
### END INIT INFO
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
 
REMOTE=''
LOCAL=''
CONFIG='/root/.config/rclone/rclone.conf'
DEMO='rclone'
 
[ -n "$REMOTE" ] || exit 1;
[ -x "$(which fusermount)" ] || exit 1;
[ -x "$(which $DEMO)" ] || exit 1;
 
case "$1" in
start)
  ps -ef |grep -v grep |grep -q "$REMOTE"
  [ $? -eq '0' ] && {
    DEMOPID="$(ps -C $DEMO -o pid= |head -n1 |grep -o '[0-9]\{1,\}')"
    [ -n "$DEMOPID" ] && echo "$DEMO already in running.[$DEMOPID]";
    exit 1;
  }
  fusermount -zuq $LOCAL >/dev/null 2>&1
  mkdir -p $LOCAL
  rclone mount $REMOTE $LOCAL --config $CONFIG --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 >/dev/null 2>&1 &
  sleep 3;
  DEMOPID="$(ps -C $DEMO -o pid=|head -n1 |grep -o '[0-9]\{1,\}')"
  [ -n "$DEMOPID" ] && {
    echo -ne "$DEMO start running.[$DEMOPID]\n$REMOTE --> $LOCAL\n\n"
    echo 'ok' >/root/ok
    exit 0;
  } || {
    echo "$DEMO start fail! "
    exit 1;
  }
  ;;
stop)
  DEMOPID="$(ps -C $DEMO -o pid= |head -n1 |grep -o '[0-9]\{1,\}')"
  [ -z "$DEMOPID" ] && echo "$DEMO not running."
  [ -n "$DEMOPID" ] && kill -9 $DEMOPID >/dev/null 2>&1
  [ -n "$DEMOPID" ] && echo "$DEMO is stopped.[$DEMOPID]"
  fusermount -zuq $LOCAL >/dev/null 2>&1
  ;;
init)
  fusermount -zuq $LOCAL
  rm -rf $LOCAL;
  mkdir -p $LOCAL;
  chmod a+x $0;
  update-rc.d -f $(basename $0) remove;
  update-rc.d -f $(basename $0) defaults;
  rclone config;
  ;;
esac
 
exit 0
```



## 备注

1. 没有安装fuse

```bash
#出现提示如下：
Fatal error: failed to mount FUSE fs: fusermount: exec: "fusermount": executable file not found in $PATH

#Debian/ubuntu安装
apt-get install fuse

# CentOS安装
yum install fuse
```

2. OVZ VPS

```bash
# 出现如下提示：

# OVZ由于虚拟化技术原因，可能需要发工单要求支持fuse
```

3. 配置文件存储位置

```bash
~/.config/rclone/rclone.conf
```

## 参考链接

[[ 教程 ] 在 Linux VPS 上利用 rclone 挂载 OneDrive 网盘](https://moeclub.org/2017/11/28/500/)

[[ 教程 ] 在Linux VPS上使用 rclone 挂载 Google Drive 网盘](https://moeclub.org/2017/11/28/500/?v=300)

[Rclone使用指南](https://softlns.github.io/2016/11/28/rclone-guide/)
