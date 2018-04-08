---
layout: blog
banana: true
category: 负载均衡
title:  Nginx upstream 为php-fpm 实现负载均衡 使用实战
date:   2018-01-15 22:06:42
background-image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522826644054&di=73e0e2303f6588a9e646f164c9e84008&imgtype=0&src=http%3A%2F%2Fbpic.ooopic.com%2F15%2F31%2F43%2F15314383-b584ea06260074133cb666f7762daed2-2.jpg
tags:
- Redis
- 架构之路
- Nosql
- 负载均衡	
---

1、nginx配置vhost虚拟主机
2、nginx通过upstream配置负载均衡
1）配置http负载均衡
2）配置php-fpm负载均衡
3、nginx配置日志
4、awk日志统计分析
