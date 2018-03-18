---
title: 教程-安装Windows系统
date: 2018-03-15 22:01:13
tags: [教程, WIN, 系统]
categories: 教程
---

# 教程-安装Windows系统

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=419596181&auto=1&height=66"></iframe>

## 系统镜像

从 `MSDN, I tell you: https://msdn.itellyou.cn/ `下载需要的原版ISO文件,
下载后比对hash值，确保无篡改。
另外，不要从莫名其妙的网站下载各种奇怪的GHO镜像。

## 制作启动盘

> 现今主流为UEFI mode启动，比较旧的主板可能只支持Legacy mode启动，
> 从EFI Fireware 2.5 开始只支持UEFI，彻底抛弃Legacy。

一般启动盘都是格式化成FAT32，不能存储单个大于4G的文件，需要其它方法解决，以后再写。

- Legacy mode
  - 软碟通往U盘或者移动硬盘写入镜像来制做可启动设备，非免费软件。
  - [Rufus](https://rufus.akeo.ie/)，开源免费的多功能启动盘制作工具，推荐。
  - [Bootice](http://www.ipauly.com/2015/11/15/bootice/)，如果不想格式化移动存储设备，可以使用此软件写入 主引导记录，分区引导记录，然后把镜像文件全部解压到U盘或者移动硬盘。
- UEFI mode：直接把镜像文件全部解压到U盘或者移动硬盘

## 安装

- 开机按快捷键（看厂商定义）进入BIOS（实际现在都是UEFI fireware）
- 点选`自定义`，全新安装
- 硬盘分区，如果是全新硬盘，点选新建，程序会自动建立4个分区
- 选择分区安装系统
- 安装完成后，设置用户名和密码

## 安装驱动

- Windows10 update会联网下载更新一些驱动，比如：CPU核显驱动
- 如果是品牌机，请务必到官网下载驱动
- 如果是组装机，需要驱动助手类软件的
  不推荐国产的各种驱动人生/精灵等，避免被捆绑安装
  推荐使用[Driver Booster 5](https://www.iobit.com/en/driver-booster.php)，免费版足够使用。



## 激活

VOL，即零售版可以使用KMS激活。

- 可用KMSpico软件本地激活

- 也可网络KMS服务器激活，如 https://03k.org/kms.html ，
  直接两条命令无脑激活即可

  ```bash
  #用管理员权限运行CMD或者Power shell
  slmgr /skms kms.03k.org
  slmgr /ato
  ```

  ​



