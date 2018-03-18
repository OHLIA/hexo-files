---
title: vv
date: 2018-03-05 20:19:25
tags: [Linux,VPS,祥瑞御兔]
password: xiangruiyutu
---

# 【笔记】V2ray

[TOC]
@[TOC]

官网：https://www.v2ray.com/
配置指南：https://toutyrater.github.io/

## 安装

### 准备

- 服务器对时，误差不超过2分钟

### 服务端

```bash
#官方文档安装脚本，详见：https://www.v2ray.com/chapter_00/install.html
bash <(curl -L -s https://install.direct/go.sh)
#V2Ray配置指南 提供的安装脚本，详见：https://toutyrater.github.io/prep/install.html
wget https://toutyrater.github.io/install-release.sh
sudo bash install-release.sh 
```

##  使用

- 常用命令

```bash
sudo systemctl start v2ray #运行
sudo systemctl stop v2ray #停止
sudo systemctl restart v2ray #重启
systemctl status v2ray.service #查看是否运行
```

- 客户端：
  - 多平台核心程序release：v2ray-core: https://github.com/v2ray/v2ray-core/releases
  - Windows GUI： v2rayN : https://github.com/2dust/v2rayN/releases #搭配core使用
  - Andriod： v2rayNG  #Google Play下载
  - IOS: ShadowRocket #换美区下载

##  WS+V2ray+TLS+CDN

### 服务端

#### V2ray配置

文件位置：/etc/v2ray/config.json

下面代码直接复制会有格式问题，可以下载并解压[v2ray配置文件](https://github.com/OHLIA/filesplan/blob/master/v2ray.config.json.tar.gz)到：/etc/v2ray/ ,然后自行改 port 和 UUID，path 三个地方。

修改好后重启以下v2ray。

```bash
{
    "log": {
            "access": "/var/log/v2ray/access.log",
            "error": "/var/log/v2ray/error.log",
            "loglevel": "warning"
    },
    "inbound": {
            "port": 11111,
            "listen":"127.0.0.1",
            "protocol": "vmess",
            "allocate": {
                   "strategy": "always"
                        },
            "settings": {
                    "udp": true
                    "clients": [{
                            "id": "061dc924-dbe0-4175-953e-2b7e6255c86c",
                            "level": 1,
                            "alterId": 64,
                            "security": "auto"
                    }]
            },
            "streamSettings":{
                    "network":"ws",
                    "security": "auto",
                    "wsSettings":{
                            "connectionReuse": true,
                            "path": "/v2ray/"
                            }
                    }
    },
    "outbound": {
            "protocol": "freedom",
            "settings": {}
    },
    "outboundDetour": [
        {
            "protocol": "blackhole",
            "settings": {},
            "tag": "blocked"
        }
    ],
    "routing": {
        "strategy": "rules",
        "settings": {
            "rules": [
                {
                    "type": "field",
                    "ip": [
                        "0.0.0.0/8",
                        "10.0.0.0/8",
                        "100.64.0.0/10",
                        "127.0.0.0/8",
                        "169.254.0.0/16",
                        "172.16.0.0/12",
                        "192.0.0.0/24",
                        "192.0.2.0/24",
                        "192.168.0.0/16",
                        "198.18.0.0/15",
                        "198.51.100.0/24",
                        "203.0.113.0/24",
                        "::1/128",
                        "fc00::/7",
                        "fe80::/10"
                    ],
                    "outboundTag": "blocked"
                }
            ]
        }
    }
}
```

- port，V2ray使用的端口
- "listen":"127.0.0.1", 只监听localhost，避免暴露端口，如果不能正常使用，可以去掉
- "id": "b831381d-6324-4d53-ad4f-8cda48b30811"，UUID需要更换
  - UUID生成网站：https://www.uuidgenerator.net/
- "path": "/v2ray/"，填入的/v2ray和/v2ray/是有区别的，需要统一，除旧版ngnix必须/ray/外，其余最好不要出现“v2”“ray”等字眼！
- `"allocate": {"strategy": "always"}`，分配设置-分配策略，`"always"`表示总是分配所有已指定的端口

  ​

#### http服务配置

修改好后记得重启nginx或者caddy使其生效。

- nginx

  在对应网站配置文件添加

  - 例如宝塔面板，网站-设置-配置文件，添加

  ```
  location /v2ray/ {
  proxy_redirect off;
  proxy_pass http://127.0.0.1:11111; #此处的11111为你的v2ray端口。请自己修改
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  proxy_set_header Host $http_host;
  }
  ```

- caddy

  ```
  mydomain.me
  {
    log ./caddy.log
    proxy /v2ray/ localhost:111111 {
      websocket
      header_upstream -Origin
    }
  }
  ```

### 客户端

- Windows下使用V2rayN

![Windows V2rayN](https://s1.ax2x.com/2018/03/05/ruLUn.png)

- IOS下使用shadowrocket
<div align="center">
<img src="https://s1.ax2x.com/2018/03/05/ruenE.png" width="300px" alt="IOS_Shadowrocket" style="display: inline-block" /><img src="https://s1.ax2x.com/2018/03/05/ruKWQ.png" width="300px" alt="IOS_Shadowrocket2" style="display: inline-block" />    
</div>








### CDN

选用免费的CF CDN：https://www.cloudflare.com

---

## 参考链接

https://www.aihoom.com/1274.html

https://toutyrater.github.io/advanced/wss_and_web.html

https://www.ivyseeds.cf/websocket/