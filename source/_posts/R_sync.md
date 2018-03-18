---
title: 教程-同步工具Resilio sync使用
date: 2018-03-14 21:40:35
tags: [教程,同步,sync]
---

# 教程-同步工具Resilio sync使用

## 前言

官网：https://www.resilio.com/
中文官网：https://www.resilio-sync.cn/

- 目前此工具的Tracker服务器在大陆无法访问，需要自由上网才能充分发挥作用，不过拿来局域网同步文件也是极好的。

## 安装

- Windows平台，直接默认选项安装
- Linux平台，下载对应二进制文件

## 设置

- 初次安装，弹出订阅提示，可忽略。
- 初次运行，设置昵称，即网络显示的名称，勾选“入门”。
- 修改默认文件夹位置：首选项 -> 通用
- 设置限速：首选项 -> 高级 -> 带宽
- 设置代理服务器：首选项 -> 高级 -> 连接 -> 使用代理服务器，支持socks4，socks5，Https协议。勾选`使用代理服务器用于主机名查询` ，否则也会代理下载流量。不勾选`验证` 。
- 许可证：可以直接用 .btsky 后缀名的文件；也可以用文本编辑器打开此文件，复制文本内容，粘贴到`输入密钥或链接` 中。

## 使用

### 下载（同步）文件

把同步密钥或者链接复制到`输入密钥或链接`，设置选择性同步，点下一步选择存储位置。

### 发布文件夹

点击左上角`+`，选择同步类型，一般选择标准文件夹即可， 
选择要同步的文件夹，
在弹出的窗口，按需设置`安全性`选项，一般全部不勾选即可，
把对应的 只读密钥 发布到网上。
如果是读写密钥，则任何人皆可增删文件。

## 其他

### 无法获取节点

1. 使用代理，最简单粗暴

2. 修改hosts文件，具体IP需要网上搜索

3. 安装Zerotier并加入Network

   ```
   ##神Key的Network
   #zerotier里面加入
   446b538c9d8ca1d3
   #修改hosts,添加
   54.192.232.168 config.resilio.com 
   ```

   ​

### 选择性同步

手机客户端免费支持，桌面端需要许可证，否则只有30天试用。

### 加密同步

同步类型选择`加密文件夹`，增加了`加密密钥`，适合在服务器上存储又不想被知道内容的情况，无法破解，只能用常规的读写或只读密钥同步。

### 许可证

```
网上找到的许可证，直接全部复制到输入密钥或链接即可。
btos1_eyJzIjogIm5xNjRmNGgrWDhxWHFiaWRodEtyTWdNcEVaMHdYaUJLbExwSWFwSDUxMzkyaHdqMFFpQWJOTTFGd2JGWGpJQ0FhV2w4V3ZidVpPenpITXZmNTRLWU1XVGVsVFZjS3JGQUpYMmVUMGttT2lCMm8zQ1BjVFd2VWlCLzRsTTlQN1FsalVSVzREMVFNUktWcEhwaG5IVzIvZmdqN05MaThmMnhNa1NDNERwRVVpQ0pXZHpPWXFIbW1OMFdOUEt6QWg4RkJoanNvUVNBa1hKN09WS0c5WElkVTczWnNlVWNxWW1WVUo0WDc0S05wYmlYZk0vemlsU2pIK0U5Y3ZMZ092YlplVjlCZXlveDNwT3pRYUJOeFNQT3FYWEpTRHE1bVRhRXMvWGppVGtJV21uVHJudVJnb01DZ0RBOHdBS3RIYjRvVkduWm1jeU8xNlhoQ3dvL2VkeHFxZz09IiwgImQiOiAiZXlKamF5STZJQ0ppZEc5ell6RmZaWGxLYW1ONVNUWkpRMHBJVlZoYVYxRldUakZsVldoWVZrWldNMUpXU2tWVVJURlZZVEZXZFZacVVsUmpNMUp5Wkcxd1FsUkVRVFZWUXpsMVZraGtUVk5wT0hsUFEzUjNUV3hrVjJOck1IaFVhelI1VXpOQ1QyUlhiR3RTYlVwb1ZtNUtURlJZVVRWVFJ6RXpWVVU1TVZwdVdtRlVWbVJ1VVd4Sk1sWXlXbkZsYm14b1ZWaHdlbEV3TVVKamJXaE5XVlYwZFZwVVJYZFRSbEpOVW5wU2JGbHBkRXBVVlRGSlYwUnNWMkV5ZEZSalZFRXdUbTVuTUdKck1WZE5SR2N3VmxWYVRsSnJNVzlqUlVwMFlVUlNlRTFJYkZaWmExRXhVbTFzTUUxSVpIZE5iR3hVWWxob1IyTkVSbE5sVlhkeVVsWm9OV0ZGUlROUmJWSnVWVEZDYUdKSVZrWkxNV3hZVmpGb2FtSjVPVWxSVlRsT1ltMTNNbE5zUlRCYWFsSlFWbXBCZUZRd1VrSlpha1Z5V20wMWRVNXVXblZSTTJnell6RldTazlGTVU1aFNHeFFUREE1TUZOc2JFWldTRkpXVlVaSk1sUkRkRkpXUjJoUlYwUlNjbGxyTlhGTlJHeFpXbmwwYjJGVk1VeFZWMVpVU3pCd2RFNTZVblpUYW14SVpEQnJlVkZ0TVVsalJGWXdWRlZTZEZreWVIcE9WbWhLV1Zkb1MxZEdSa1pqTUhSMVVsaFdlbE13VGpSaVJsSjVUVmR3ZFdOSGFFNWFlbFV3WW0xNGJFOVVhRzlUU0VWeVVWaE5NV1ZZVWxSTlIyTTVVRk5KYzBsRFNtcGFRMGsyU1VOS2JHVlZjSGRaYlRGaFpHdHNjV0l5Wkd4bFZYQTJXa2N4VDFKSFNYbFZiWGhLWVcwNWJsTlhOVTlPVjBwMFZHeEdhbUpVYUhCVVJVNUNZVlpyZVdSSVFtRlJNR3N5VTFWT1MyVkdWblJXVkVKVFltczFjbGt3V2s1TmJVVjNaVVYwYW1Wc1NsaFpla0pyVFVaRmVGRnRlRmRUUlRWNVUxZHNNMW93YkhSV2FsSnFVVEJyTWxOVlVrcGxSVFZGVmxSV1RsWkdhekJVVlZKQ1l6QnNSRk50Y0dwTk1VWndWREpzUW1GWFNqVk5WelZQVmtaYVVGbDZRbTloYkZwelZsZHNUVkV3Um5CYVJXaHpaREZ3VkZOVVdrcFJNSEF6VjJ4b1MyVnRTWGxPVjJocFVUQnNlbE5WVGt0a2JVNUpWVzV3U21GdE9XNWFXR3hMWWxkSmVXVkhkR0ZYUlhCVlYyeGtUMlZXY0ZsVlYyeFFZVlZHY0ZSdGMzaFVNVlpIVjJ4YVZGSkhVa05VV0hCVFYyeEdWbFZVUmxoU1ZGWkhWV3RXVTFWV1VrWk9WVnBYVFZWd1UxUlhjRk5VVmxsNFdqSnNUVkV3Um5CWmVrcFhZVWRTU1ZSWGJGQmhWVVkwV214bmQyTXdiRVJUYlhoc1UwVkdjRlF5YkVKbFZURlZWVlJHVUZaRlZYbFVNRkpDWkRKYVVsQlVNR2xtVVQwOUlpd2dJbWx1Wm04aU9pQjdJbTl5WkdWeVNXUWlPaUFpWVc1aGRXeG1ZVzUxYTJadWIydHViVzF6TURZNWNXbzNOQ0lzSUNKaGNpSTZJREFzSUNKbGVIQWlPaUF5TVRRMU9URTJPREF3TENBaVkzSmxJam9nTVRRNE1UUXlOek00Tnl3Z0luTjJZMGxrSWpvZ0luTjVibU5RY204eElpd2dJbU56ZENJNklDSnZMV2MxTlU1elNHTldWU0lzSUNKemRtTkRiMlJsSWpvZ0luTjVibU5RY204aUxDQWlZMmxrSWpvZ0ltZHFaV295YmlJc0lDSjBlWEJsSWpvZ0luQmxjbk52Ym1Gc0lpd2dJbTl3ZEhNaU9pQjdJbVp2YkdSbGNsTmxZM0psZENJNklDSkJWMFJOVHpSRlUxTkJUbGhCV2xKVE1rMHlORlpCTmtkQlIxSlhWVFJRTlZZaUxDQWljMlZoZEhNaU9pQXhmWDBzSUNKbGVIQWlPaUF5TVRRMU9URTJPREF3ZlE9PSJ9
```

- 提示授权失效：
  1. 找到安装目录，删除License 目录和.SyncUserxxxxxx 目录
  2. 重新授权
  3. 禁止两个目录写入权限
  4. 禁止授权验证，在hosts文件添加一行：`127.0.0.1 license.resilio.com`

### 同步密钥分享

- 神key：BCWHZRSLANR64CGPTXRE54ENNSIUE5SMO

  - 网络资源集合密钥，不想费力找最新电影电视剧可以用用，内容是一个html导航文件，目录如下：

    ```
    主目录（点击可直达分类列表）
    关于神 Key 和 Sync
    数字资源拷贝服务
    最近更新
    大片抢先看 movie_express
    高分电影 top_movies
    2018年：1月 2月
    2017年：1月 2月 3月 4月 5月 6月 7月 8月 9月 10月 11月 12月
    2016年：1月 2月 3月 4月 5月 6月 7月 8月 9月 10月 11月 12月
    2015年：1月 2月 3月 4月 5月 6月 7月 8月 9月 10月 11月 12月
    电视剧 tv_series
    国产剧 港台剧 日剧 韩剧 美剧/英剧
    纪录片 documentary
    综艺 variety_shows
    软件 applications
    电子书 books
    教育 education
    无损音乐 music
    经典影视合集 top_collections
    网友分享的其他Sync资源
    优质资源保种计划（beta）
    教程 tutorials
    工具 tools
    常用播放软件推荐（for Win）
    视频标题里的CAM、TS、TC、HD等是什么意思
    友情链接 Other Sites
    广告位招租 Your Ad Here
    ```

  - Wanimal1983：

    ```
    只读密钥：ERTRX7DOYJREIJVRZ3AMNX6O65ZXL5FLYXYCA2KI4ZV2RBAG6UGLVISV3QA
    加密密钥：FRTRX7DOYJREIJVRZ3AMNX6O65ZXL5FLY
    ```
    - wanimal，故宫门事件当事人，争议人体摄影师。
