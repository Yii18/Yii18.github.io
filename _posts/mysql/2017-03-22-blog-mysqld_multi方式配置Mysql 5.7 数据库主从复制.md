---
layout: blog
istop: true
title: "mysqld_multi方式配置Mysql 5.7 数据库主从复制"
background-image: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=61696430,1796609944&fm=27&gp=0.jpg
date:  2017-03-22
category: mysql
tags:
- nginx
- mysql
- 索引优化
- 主从复制
---



mysqld_multi方式配置Mysql 5.7 数据库主从复制

实战参考文档：http://www.tianshouzhi.com/api/tutorials/mysql

说明，本文图片并茂，讲解比较细，练习完全可以安装上面的步骤。
但是为了下一步练习atlas读写分离中间件，建议把文章中的mysql换为版本为5.6的

其它参考文档： MySQL主从复制简单介绍、重建、中断处理

将原标题 mysqld_multi方式配置Mysql数据库主从复制 修改成 mysqld_multi方式配置Mysql 5.7 数据库主从复制


当在从库执行mysql>start slave;后报错：
ERROR 1200 (HY000): The server is not configured as slave; fix in config file or with CHANGE MASTER TO

解决方法：

mysql> CHANGE MASTER TO \
-> MASTER_HOST='localhost',\
-> MASTER_USER='slave',\
-> MASTER_PORT=3306,\
-> MASTER_PASSWORD='slave',\
-> MASTER_LOG_FILE='mysql-bin.000001',\
-> MASTER_LOG_POS=2418;

备注：
MASTER_LOG_POS的值要在主库上执行show master status 来获取。


MySQL 查看 binlog日志内容

[root@izj6c2vchus974clf0hfu8z ~]# mysqlbinlog --base64-output=decode-rows -v /usr/local/mysql/data3306/mysql-bin.000001

 
mysql数据库备份、恢复

主库备份：
[root@izj6c2vchus974clf0hfu8z ~]# mysqldump -uroot -p -S /tmp/mysql.sock3306 --default-character-set=utf8 -q --single-transaction --master-data -A > /tmp/all_database.sql

从库恢复：
3307库
[root@izj6c2vchus974clf0hfu8z ~]# mysql -uroot -S /tmp/mysql.sock3307 -p --default-character-set=utf8 < /tmp/all_database.sql
3308库
[root@izj6c2vchus974clf0hfu8z ~]# mysql -uroot -S /tmp/mysql.sock3308 -p --default-character-set=utf8 < /tmp/all_database.sql


如何查看用户和组是否创建成功

查看用户：cat /etc/passwd |grep mysql
查看组：cat /etc/group |grep mysql


mysql5.6 改为主从 install数据报错解决：

改原来的5.6为主从
[root@localhost ~]# /usr/local/mysql/scripts/mysql_install_db --no-defaults --initialize-insecure --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data3308 --user=mysql --explicit_defaults_for_timestamp

完全按照文档安装
[root@localhost ~]# /usr/local/mysql2/bin/mysqld --no-defaults --initialize-insecure --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data3308 --user=mysql --explicit_defaults_for_timestamp


mysqld_multi 指定my.cnf配置文件启动服务

[root@iZ2zeeefbpoojk9incccu6Z mysql]# mysqld_multi --defaults-extra-file=./my.cnf start 3306,3307,3308

[root@iZ2zeeefbpoojk9incccu6Z mysql]# mysqld_multi --defaults-extra-file=./my.cnf stop 3306,3307,3308

[root@iZ2zeeefbpoojk9incccu6Z mysql]# mysqld_multi --defaults-extra-file=./my.cnf report


mysql 指定端口、mysql.sock参数登陆服务：

[root@iZ2zeeefbpoojk9incccu6Z ~]# mysql -uroot -S /usr/local/webserver/mysql/mysql.sock3307 -P3307 -p

mysqld_multi report/start/stop 之后状态值不正确的处理方法

执行如下命令（以下命令都是mysqld_multi脚本所使用的命令，但是脚本里取密码有bug）

mysqld_multi report操作
执行命令
[root@iZ2zeeefbpoojk9incccu6Z ~]# /usr/local/webserver/mysql/bin/mysqln -u root -p --port=3307 --socket=/usr/local/webserver/mysql/mysql.sock3307 ping
输入密码
Enter password:
输出值
mysqld is alive

mysqld_multi stop 操作
执行命令
[root@iZ2zeeefbpoojk9incccu6Z ~]# /usr/local/webserver/mysql/bin/mysqln -u root -p --port=3307 --socket=/usr/local/webserver/mysql/mysql.sock3307 shutdown

mysqld_multi start 操作
[root@iZ2zeeefbpoojk9incccu6Z ~]# /usr/local/webserver/mysql/bin/mysqld_safe --basedir=/usr/local/webserver/mysql --datadir=/usr/local/webserver/mysql/data3307 --port=3307 --user=mysql --socket=/usr/local/webserver/mysql/mysql.sock3307 --server_id=2 >> /usr/local/webserver/mysql/mysqld_multi.log 2>&1 &


mysql数据库修改密码
mysql> update mysql.user set Password = password('mysql@123') where user='root';
mysql> flush privileges;