---
layout: blog
banana: true
category: Redis 高级特性：如何用nginx直接访问Redis？Lua （openresty）
title:  redis
date:   2017-08-09 01:06:42
background-image: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1118154010,206880052&fm=27&gp=0.jpg
tags:
- road
- nginx
- lua
- 负载均衡
- php


#  哪些大公司在使用这种架构？

## 目前在京东如实时价格、秒杀、动态服务、单品页、列表页等都在使用Nginx+Lua架构，其他公司如淘宝、去哪儿网等。

###  参考文档

#### [OpenResty官方文档](http://openresty.org/cn/getting-started.html "OpenResty官方文档")
#### [浅谈 OpenResty](http://www.linkedkeeper.com/detail/blog.action?bid=1034 "浅谈 OpenResty")
#### [基于cosocket API的ngx_lua的Lua redis客户端驱动程序](https://github.com/openresty/lua-resty-redis "基于cosocket API的ngx_lua的Lua redis客户端驱动程序")
