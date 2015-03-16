---
layout: post
title: centos 双网卡配置
description: 公司需要一个新的服务器，2个网口，一个是内网访问，另外一个网口接外网，目的是让服务器本身上网下载资源，setup工具等;
category: blog
---

## 双网卡配置双路网络备忘

###  首先配置2个网口的静态IP地址
     setup static ip /etc/sysconfig/network-scripts/ifcfg-eno1/eno2

 默认网关只存在一个，我们选择外网优先，然后对于内网网段的特殊问题后面加网段到固定表;
 故，start ip 191.61.4.86 不需要设置gateway.

```

    # 默认网关启动后是192.168.10.1，此时能访问外网，不能访问内网的部分服务器
    
    port 1 : static ip 191.61.4.86 netmask 255.255.255.192
    port 2 : static ip 192.168.11.199 netmask 255.255.254.0 gateway 192.168.10.1

```

### 配置路由表
    可以根据static-routes的规则添加在/etc/sysconfig/static-routes 里面，
    这么做的好处是不管重启系统还是重启网络服务，设置都会生效;
    我加在了rc.local 文件里面，如果手动重启了网络服务，那么需要手动运行下这部分代码;
    不过对于我们的系统，放机房手动重启网络的机会会很少,所以直接append了脚本啦...

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

    # add static inner net to table phi
    # nslookup result (seds/Address: /ip rule add to /g'...)
    ip rule add to 130.140.80.134 table phi

```

append /etc/rc.local

restart network.

参考: 
 * [ref-1][1], 
 * [2][], 
 * [linux策略路由][3], 
 * [ip命令][4]

[poornigga]:    http://poornigga.github.io "poornigga"
[1]: http://blog.sina.com.cn/s/blog_66719ff30100yft7.html "ref-1"
[2]: http://blog.sinzy.net/jinjian/entry/21485 "ref-2"
[3]: http://my.oschina.net/guol/blog/156607 "linux策略路由"
[4]: http://blog.csdn.net/wolongzhumeng/article/details/8848389 "ip命令"

