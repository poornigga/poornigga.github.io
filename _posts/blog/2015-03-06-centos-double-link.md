---
layout: post
title: centos 双网卡配置
description: 公司需要一个新的服务器，2个网口，一个是内网访问，另外一个网口接外网，目的是让服务器本身上网下载资源，setup工具等;
category: blog
---

## 双网卡配置双路网络备忘

### setup static ip /etc/sysconfig/network-scripts/ifcfg-eno1/eno2
port 1 : static ip 191.61.4.86 netmask 255.255.255.192 gateway 191.61.4.126
port 2 : static ip 192.168.11.199 netmask 255.255.254.0 gateway 192.168.10.1

```
    
    # create 2 table
    echo "200 phi" >> /etc/iproute2/rt_tables
    echo "201 pub" >> /etc/iproute2/rt_tables

    # add internal network rules phi
    ip route flush table phi
    ip route add default via 161.91.4.126 dev eno1 src 161.91.4.86 table phi
    ip rule add from 161.91.4.86 table phi

    # add pub network rules pub
    ip route flush table pub
    ip route add default via 192.168.10.1 dev eno2 src 192.168.11.199 table pub
    ip rule add from 192.168.11.199 table pub

```

append /etc/rc.local

restart network.

[poornigga]:    http://poornigga.github.io "poornigga"
