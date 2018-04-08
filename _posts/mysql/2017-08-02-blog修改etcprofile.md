---
layout: blog
istop: true
title: "修改/etc/profile导致所有命令不能使用"
background-image: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2962384197,120518553&fm=27&gp=0.jpg
date:  2017-08-02
category: mysql
tags:
- nginx
- mysql
- 索引优化
---

#### 输入：/usr/bin/vim /etc/profile

注意vim后有空格
然后将原来修改的内容删除

#### 重新启动 
/sbin/shutdown -r now 或者/sbin/reboot