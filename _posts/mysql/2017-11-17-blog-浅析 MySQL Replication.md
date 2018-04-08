---
layout: blog
istop: true
title: "浅析 MySQL Replication"
background-image: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=143837728,2741875622&fm=27&gp=0.jpg
date:  2017-11-17
category: mysql
tags:
- nginx
- mysql
- MysqlReplication
- 架构
---
[浅析mysql Replication](http://mp.weixin.qq.com/s/YCTdHCGnQLjqOziE8dPbQA "浅析mysql Replication")
本文几乎涵盖了MySQL Replication（主从复制）的大部分知识点，包括Replication原理、binlog format、复制中如何保证数据一致性、组提交、复制优化、半同步复制、多源复制。

目前很多公司中的生产环境中都使用了MySQL Replication ，也叫 MySQL 复制，搭建配置方便等很多特性让 MySQL Replication 的应用很广泛，我们曾经使用过一主拖20多个从库来分担业务压力。关于 MySQL Replication 的文章网络上也有很多，但大多数都是讲如何搭建MySQL Replication，并没有说清楚如何才能搭建出高可靠的MySQL Replication。这篇文章也对半同步复制，无损复制，多源复制做了讲解。
