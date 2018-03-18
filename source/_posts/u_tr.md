---
title: 笔记-在Ubuntu安装Transmission
date: 2018-03-08 22:20:26
tags: [Linux,Ubuntu,VPS,Learn,Transmission]
categories: 笔记
---

# 笔记-在Ubuntu安装Transmission

[TOC]

@[TOC]

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1859345&auto=1&height=66"></iframe>

## 安装

```bash
apt-get install vim git unzip 
apt-get install transmission transmission-daemon transmission-cli
```

## 基本命令
```bash
#建议用root权限启动
sudo service transmission-daemon start
sudo service transmission-daemon stop
sudo service transmission-daemon restart
#以下命令会是配置文件恢复初始值
/etc/init.d/transmission-daemon start
/etc/init.d/transmission-daemon stop
/etc/init.d/transmission-daemon status
transmission-daemon {start|stop|reload|force-reload|restart}
#查看端口占用情况
netstat -an
netstat -antup
```

## 更新WEBUI
WEBUI:https://github.com/ronggang/transmission-web-control/releases
先把自带的index.html更名为 index.original.html
K3路由器解压到 /etc/share/transssion/web
Ubuntu系统解压到 /usr/share/transmission/web/

## 配置文件
```bash
#先结束进程，否则改动配置文件会复原
sudo service transmission-daemon stop
#修改配置文件/etc/transmission-daemon/setting.json
#用户名和密码
#取消白名单
“rpc-authentication-required”: true, #需要验证
“rpc-bind-address”: “0.0.0.0″, #表示任意地址域名可访问TR面板
“rpc-enabled”: true, #开启rpc
“rpc-password”: “{c8c083168db9fff40b5136b6d0f3f4a864110a78\/oH51JaE”, #修改密码！生效后会变成乱码
“rpc-port”: 9091,
“rpc-username”: “root”,
“rpc-whitelist”: “127.0.0.1″, #白名单
“rpc-whitelist-enabled”: false, #如果不使用，此处false!
```

补充：
注意两个问题：
1、修改时，尽量用telnet进去用vi修改；或者用FTP上去直接选中按Alt-V键启动编辑程序。经常遇到拷贝到Windows下用记事本修改后，由于编码格式不对，配置文件失效的情况。
2、Linux下是区分大小写的，不要把Windows下的习惯带过来。
以下是该配置文件的全文，常用项用红色字注解，完全不要抄到该配置文件中。
```js
{
“alt-speed-up”: 50, #计划时段上传限速值，ADSL不宜超过40，否则会影响该时段的其它网络应用性能；如果希望该时段全部网络都给tr使用，也最好设置50。
“alt-speed-down”: 250, #计划时段下载限速值，建议不超过260
“alt-speed-enabled”: true,
“alt-speed-time-begin”: 1380, #计划开始时间，为零点到开始时间的分钟数，比如23：30就是23*60+30=1410
“alt-speed-time-day”: 127, #计划周期，每周一执行则为2；周二=4；周三=8；周四=16；周五=32；周六=62；周日=1；工作日=62；周末=65；每天=127
“alt-speed-time-end”: 420, #计划结束时间，为零点到开始时间的分钟数，比如7：00就是7*60=420
“alt-speed-time-enabled”: true, #启用计划工作，为false时，以上计划配置则不生效

“bind-address-ipv4″: “0.0.0.0″,
“bind-address-ipv6″: “::”,
“blocklist-enabled”: false,
“dht-enabled”: false, #关闭DHT功能，不少PT站的要求，但BT下载设置为true会使得下载更好
“download-dir”: “\/tmp\/hdd\/media\/Torrent”, #默认下载的内容存放的目录
“encryption”: 0,
“incomplete-dir”: “\/tmp\/hdd\/media\/Torrent”,
“incomplete-dir-enabled”: false,
“lazy-bitfield-enabled”: true,
“message-level”: 2,
“open-file-limit”: 32,
“peer-limit-global”: 80, #全局连接数，据观测取值80能提高tr的稳定性，可能太多的连接数会导致1073的CPU过载而死机
“peer-limit-per-torrent”: 60,
“peer-port”: 51413,
“peer-port-random-high”: 65535,
“peer-port-random-low”: 49152,
“peer-port-random-on-start”: false,
“peer-socket-tos”: 0,
“pex-enabled”: true,
“port-forwarding-enabled”: true,
“preallocation”: 1, #预分配文件磁盘空间，建议取1开启该功能，防止下载大半了才发现磁盘不够。但注意如果连续添加几个大个头的种子时，一定要等待前一个种子添加成功后再添加下一个种子，否则由于在分配空间时，tr无法响应你的添加操作而导致死机。
“proxy”: “”,
“proxy-auth-enabled”: false,
“proxy-auth-password”: “”,
“proxy-auth-username”: “”,
“proxy-enabled”: false,
“proxy-port”: 80,
“proxy-type”: 0,
“ratio-limit”: 2.0000,
“ratio-limit-enabled”: false,
“rename-partial-files”: true,
“rpc-authentication-required”: true,
“rpc-bind-address”: “0.0.0.0″,
“rpc-enabled”: true,
“rpc-password”: “{c8c083168db9fff40b5136b6d0f3f4a864110a78\/oH51JaE”, #修改密码！
“rpc-port”: 9091,
“rpc-username”: “root”,
“rpc-whitelist”: “127.0.0.1″, #白名单
“rpc-whitelist-enabled”: false, #如果使用，此处false!
“speed-limit-down”: 300, #平时的下载限速，建议不大于260，我的RP好，所以300也可用
“speed-limit-down-enabled”: true, #启用平时下载限速
“speed-limit-up”: 30, #平时上传限速，ADSL建议不超过40，据观测30以下的值才能保证全速下载，40以上即使你下载限速高也无法高速，这是网络TCP协议特性所致。
“speed-limit-up-enabled”: true, #启用平时上传限速
“umask”: 18,
“upload-slots-per-torrent”: 14
}
```

补充一个：
［转贴］Transmission 配置文件全解：：
```js
“alt-speed-down”: 50, 时段限速下载最大值，KB/s,transmission remote（tr） 可设置。

“alt-speed-enabled”: false, 是否启动时段限速，启动改为true。tr可设。

“alt-speed-time-begin”: 540,时段限速开始时间，单位为分钟，540表示早上九点。tr可设。

“alt-speed-time-day”: 127, 时段限速日期（星期几），127表示每天，此处比较复杂，是用7位二进制数表示，然后转换成十进制数填入。例如0000001表示周日，1000000表 示周六，0000010表示周一，0000100表示周二。如果你只要在周末限速，该数应该为1000001，转换为十进制就是65；如果你只要在工作日 限速，该数应该为0111110，转换为十进制就是62，不知道我有没有说明白。tr不可设。

“alt-speed-time-enabled”: false, 启用时段限速日期，默认不开启，启动改为true。如果改为true，那么alt-speed-enabled就应该改为false，也即两项只能启动一 项，如果同时为true，则alt-speed-enabled有效。tr不可设。

“alt-speed-time-end”: 1020, 时段限速日期内限速的结束时间，分钟，1020表示下午5点。tr不可设。

“alt-speed-up”: 50, 时段限速上传最大值，KB/s。tr可设置。

“bind-address-ipv4″: “0.0.0.0″,IPv4地址绑定，一般不要改动。tr不可设。

“bind-address-ipv6″: “::”, IPv6地址绑定，一般不要改动。tr不可设。

“blocklist-enabled”: false, 启动白名单，默认不启动，需要启动改为true。tr可设置。

“dht-enabled”: true, 启用DHT网络，默认启动，不需要改为false。tr可设置。

“download-dir”: “\/data\/download\/UsbDisk1\/volume1\/transmission\/”, 下载保存的默认目录。注意该目录最好已经存在。tr不可设。

“encryption”: 1, 加密。指定节点的加密模式，默认1。0表示关闭（tr表示为允许），1表示优先，2表示强制开启。tr可设置。

“lazy-bitfield-enabled”: true, 位字段延迟？，默认为true，设置为true时可以避免某些ISP通过查询完整位段来屏蔽BT，从而破解部分ISP对BT的封杀，当然不一定完全有效。tr不可设置。

“message-level”: 2, 消息等级，应该和tr中显示统计和显示错误报告有关，默认为2，不要改动改动。有兴趣的话可以改为1和3试试。tr不可设置。

“open-file-limit”: 32, 打开文件的最大数量，如果我没有理解错，应该是指文件数而不是指种子数量，改小后可以减轻机器负荷，但是如果种子不活跃，也会影响下载速度，默认值为32。tr不可设置。

“peer-limit-global”: 240, 全局连接数限制，即用户上限，据说改为80可以提高稳定性。tr可设置。

“peer-limit-per-torrent”: 60,每个种子连接数限制，即种子属性中的最大用户数，tr可设置。

“peer-port”: 51413, 传入端口

“peer-port-random-high”: 65535,传入端口随机值范围上限，tr不可设置。

“peer-port-random-low”: 49152, 传入端口随机值范围下限，tr不可设置。

“peer-port-random-on-start”: false, 启用随机端口，默认关闭。如果改为true，则每次启动系统时，transmission会在传入端口随机值范围下限传入端口随机值范围上限随机选择一个 端口。没有必要还是false吧。tr不可设置。

“peer-socket-tos”: 0, 这个在官方没有任何解释，还是保持不动吧，呵呵。tr不可设置。

“pex-enabled”: true, 启用用户交换，默认为true，关于PEX，有兴趣的朋友可参考http://en.wikipedia.org/wiki/Peer_exchange，对于只用PT的朋友，可以设为false。 tr可设置。

“port-forwarding-enabled”: true, 启用端口转发（uPnP），如果路由支持并且也开启了uPnP，则路由会自动做端口映射，但是需要注意的是如果内网有几台机器同时使用 transmission，就必须更改peer-port值为不一样。tr可设置。

“preallocation”: 1, 文件磁盘空间预分配，默认值1为快速，0为关闭，2为完全，该值为2时，耗时较多，但是可以有效防止磁盘碎片。为了防止下载大半了才发现磁盘不够，还是默 认值1为好。但注意如果连续添加几个大个头的种子时，一定要等待前一个种子添加成功后再添加下一个种子，否则由于在分配空间时，tr无法响应你的添加操作 而导致死机。tr不可设置。

“proxy”: “”, 代理服务器URL，默认无。tr不可设置。

“proxy-auth-enabled”: false, 启用代理认证，默认不启用。tr不可设置。

“proxy-auth-password”: “”, 代理认证密码。tr不可设置。

“proxy-auth-username”: “”,代理认证用户名。tr不可设置。

“proxy-enabled”: false, 启用代理，默认不启用。tr不可设置。

“proxy-port”: 80, 代理端口。tr不可设置。

“proxy-type”: 0, 代理类型，0 = HTTP, 1 = SOCKS4, 2 = SOCKS5。tr不可设置。

“ratio-limit”: 2.0000,分享率限制。 tr可设置。

“ratio-limit-enabled”: false, 启用分享率限制，默认不启用。 tr可设置。

“rpc-authentication-required”: true,远程控制需要验证，默认为需要。tr不可设置。

“rpc-bind-address”: “0.0.0.0″, #远程控制地址绑定，默认值表示任何地址都可以访问。tr不可设置。

“rpc-enabled”: true, #启用远程控制，默认启用。tr不可设置。

“rpc-password”: “user”, #远程控制用户名。tr不可设置。

“rpc-port”: 9091, #远程控制端口，此项似乎uPnP不能自动映射，需要手动做端口映射，否则如果没有开DMZ，远程控制还是不成功。tr不可设置。

“rpc-username”: “user”,#远程控制密码，如果你已经使用过远程控制，该项会变成一串看不懂的字符，不要害怕，直接改成想要的就是了。tr不可设置。

“rpc-whitelist”: “*.*.*.*”, #远程控制白名单，默认值为所有地址，支持通配符*，如192.168.0.*，。tr不可设置。

“rpc-whitelist-enabled”: true,#启用远程控制白名单，如果启用，则仅仅上面列出的地址可以远程连接，tr不可设置。

“speed-limit-down”: 100, #下载速度限制，KB/s。tr可设置。

“speed-limit-down-enabled”: false, #启用下载限速，默认不启动。 tr可设置。

“speed-limit-up”: 100,#上传速度限制，KB/s。对于ADSL，设为35已经很好了。tr可设置。

“speed-limit-up-enabled”: false,#启用上传速度限制，默认不启动，对于ADSL，还是根据需要开启吧。 tr可设置。

“umask”: 0,
#文件权限的掩码，linux创建文档的权限（读、写、执行），默认值为18，（八进制为022）， 学过Linux的应该明白。如果我没有理解错，如果改为0，可以对创建的文件取得最高权限。tr不可设置。

“upload-slots-per-torrent”: 14 #每个种子的上传通道数，默认值14，utorrent默认值是6，我想改为6也可以了吧。tr不可设置。
```
---

## 参考链接:

https://github.com/smallwhiter/vps.github.io/wiki/Ubuntu%E4%B8%8B%E5%AE%89%E8%A3%85Transmission

https://www.noteofstudy.com/sitenote/237.html

https://www.xdty.org/482
