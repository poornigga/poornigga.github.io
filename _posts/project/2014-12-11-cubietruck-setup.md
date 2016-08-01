---
layout: post
title: Cubietruck构建
category: project
description: cubietruck 设备初始化
---
# [{{ page.title }}][1]
2014-12-11 By {{ site.author_info }}


0. download lubuntu-server-nand.img board by suit.
1. username **linaro**, password **linaro**.
2. update root password. (ssh root /etc/ssh/sshd_config)
3. update your repo, `sudo apt-get update`


## ap mode related file
* /etc/network/interfaces
* /etc/modules

[domain]:    http://poornigga.github.io  "博客"
[1]:    {{ page.url}}  ({{ page.title }})
