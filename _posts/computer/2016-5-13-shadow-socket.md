---
layout: post
title: "科学上网 2.0"
author: changeforan
category: computer
tags: [linux]
---

##1. 安装ss

**vps OS : Ubuntu 14.04 LTS**

###1.1 安装 pip

`apt-get install python-pip`

###1.2 安装 shadowsocks

`pip install shadowsocks`

##2. ss配置

###2.1 创建/etc/shadowsocks.json，内容如下

```
{
    "server":"0.0.0.0",
    "port_password": {
	    "8381": "password",
        "8382": "for",
        "8383": "each",
        "8384": "user"
    },
    "local_address": "127.0.0.1",
    "local_port":1080,
    "timeout":300,
    "method":"rc4-md5",
    "fast_open": false
}
```

###2.2  开启ssserver

`ssserver -c /etc/shadowsocks.json -d start`

##3. ss在线管理

###3.1 端口流量统计
1. 向OUTPUT链添加某用户端口（以443为例）出网规则
`iptables -A OUTPUT -p tcp --sport 443`
2. 显示流量统计
`iptables -L -vn`
3. 保存 iptables 规则
`iptables-save>/etc/iptables.rules`
4. 恢复 iptables 规则
`iptables-restore</etc/iptables.rules`
5. 清空规则链及重置计数器
`iptables -F`
`iptables -Z OUTPUT`