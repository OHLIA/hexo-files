---
title: 教程-Windows系统镜像文件大于4G的解决方法
date: 2018-03-16 21:53:24
tags: [Windows,Learn,教程]
categories: 教程
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=538560&auto=1&height=66"></iframe>

# 教程-Windows系统镜像文件大于4G的解决方法

一般制作启动盘都是采用FAT32分区格式，若系统实际映像文件/source/install.wim大于4G，则无法放入。此时需要一些解决方法。

![ebbnu.jpg](https://s1.ax2x.com/2018/03/16/ebbnu.jpg)

## （推荐）使用Dism命令对WIM映像文件进行分卷

Windows安装程序原生支持此法，故推荐。
用管理员权限运行CMD或者Power shell，输入如下命令：

```
Dism /Split-Image /ImageFile:X:\source\install.wim /SWMFile:Y:\source\install.swm /FileSize:xxxx
```

- X:\source\install.wim表示来源路径和文件
- Y:\source\install.swm表示卷后的路径和文件，后缀名swm
- FileSize:xxxx，XXXX请替换成小于4096的数值，单位为MB，表示单个分卷文件大小，分卷完成后，即可把生成的文件拷贝至启动盘的\source路径下。

![ebsW9.jpg](https://s1.ax2x.com/2018/03/16/ebsW9.jpg)

## 其他方法

1. 使用光驱安装
2. 在WinPE下安装，即制作PE维护盘，从PE里面加载镜像安装系统
3. 启动盘使用NTFS分区格式
   - 目前仅部分主板支持
   - 可用Rufus制作启动盘提高兼容性，原理是增加一个小分区，放入NTFS的驱动，达到启动目的，但仍有部分主板不支持
   - ![ebOUN.jpg](https://s1.ax2x.com/2018/03/16/ebOUN.jpg)
4. 自行对启动盘双分区：
   - 第一个分区为NTFS或者ExFat分区格式，并且解压放入ISO所有文件
   - 第二个分区为Fat32分区格式，放入启动所需文件：
     - BOOT文件夹，里面放入boot.sdi文件
     - EFI文件夹，ISO对应的所有文件
     - Source文件夹，放入boot.wim文件
5. U盘量产成光驱模式