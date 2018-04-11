---
layout: blog
banana: true
category: 负载均衡
title:  Nginx upstream 为php-fpm 实现负载均衡 使用实战
date:   2018-01-15 22:06:42
background-image: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3416533205,4005987426&fm=27&gp=0.jpg
tags:
- Redis
- 架构之路
- Nosql
- 负载均衡
- road	
---

1、nginx配置vhost虚拟主机
2、nginx通过upstream配置负载均衡
1）配置http负载均衡
2）配置php-fpm负载均衡
3、nginx配置日志
4、awk日志统计分析
