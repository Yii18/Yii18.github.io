---
layout: blog
istop: true
title: "mysql读写分离中间件"
background-image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2128502162,1917000571&fm=27&gp=0.jpg
date:  2017-07-30
category: mysql
tags:
- nginx
- mysql
- 索引优化
---


[参考文档](https://github.com/Qihoo360/Atlas/blob/master/README_ZH.md "参考文档")


读写分离

可以通过两种方式实现 
一是通过程序
  ● 框架的底层一个db文件	
二是通过中间键
  ● atlas 会通过配置文件 对 mysql 进行读写分离

注意在配置atlas 的时候请注意保持 和 主库的密码 一样 否则会报错

安装atlas中 Current database: *** NONE ***





1、下载Atlas2.2.1
wget https://github.com/Qihoo360/Atlas/releases/download/2.2.1/Atlas-2.2.1.el6.x86_64.rpm
2、安装
rpm –ivh Atlas-2.2.1.el6.x86_64.rpm
3、修改mysql-proxy配置文件
[root@iZ2zeeefbpoojk9incccu6Z ~]# vim /usr/local/mysql-proxy/conf/test.cnf
修改的参数
proxy-backend-addresses = 127.0.0.1:3306
proxy-read-only-backend-addresses = 127.0.0.1:3307@1,127.0.0.1:3308@1
pwds = root:TZ6zxvGQr9A==
（说明：加密密码生成方法 [root@iZ2zeeefbpoojk9incccu6Z ~]# /usr/local/mysql-proxy/bin/encrypt 密码）
instance = test
4、启动atlas
[root@iZ2zeeefbpoojk9incccu6Z ~]# /usr/local/mysql-proxy/bin/mysql-proxyd test start
5、测试
[root@iZ2zeeefbpoojk9incccu6Z tools]# mysql -h127.0.0.1 -P1234 -uroot -p
6、帮助
[root@iZ2zeeefbpoojk9incccu6Z ~]# mysql -h127.0.0.1 -P2345 -uuser -ppwd
  ● Aadmin
  ● 3 天前
  ● 已编辑
admin
atlas读写分离测试
思路：在my.cnf中分别为3306、3307、3308配置general_log，如果从库的general_log中只有读的语句，说明配置成功。
my.cnf配置内容：
#config general log
general_log = ON
general_log_file = /usr/local/webserver/mysql/data/mysql_general3306.log


https://www.cnblogs.com/yyhh/archive/2015/12/29/5084844.html
http://blog.csdn.net/fgf00/article/details/50599814