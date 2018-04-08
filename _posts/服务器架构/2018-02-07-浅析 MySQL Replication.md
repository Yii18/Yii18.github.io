---
layout: blog
banana: true
category: mysql
title:  浅析 MySQL Replication 
date:   2018-02-07 19:06:42
background-image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522826644054&di=73e0e2303f6588a9e646f164c9e84008&imgtype=0&src=http%3A%2F%2Fbpic.ooopic.com%2F15%2F31%2F43%2F15314383-b584ea06260074133cb666f7762daed2-2.jpg
tags:
- Redis
- php
- mysql
- 高可用&&高并发	
---

### [浅析 MySQL Replication](http://mp.weixin.qq.com/s/YCTdHCGnQLjqOziE8dPbQA "浅析 MySQL Replication")
本文几乎涵盖了MySQL Replication（主从复制）的大部分知识点，包括Replication原理、binlog format、复制中如何保证数据一致性、组提交、复制优化、半同步复制、多源复制。

目前很多公司中的生产环境中都使用了MySQL Replication ，也叫 MySQL 复制，搭建配置方便等很多特性让 MySQL Replication 的应用很广泛，我们曾经使用过一主拖20多个从库来分担业务压力。关于 MySQL Replication 的文章网络上也有很多，但大多数都是讲如何搭建MySQL Replication，并没有说清楚如何才能搭建出高可靠的MySQL Replication。这篇文章也对半同步复制，无损复制，多源复制做了讲解。