---
layout: blog
istop: true
title: "Mysql部署常用命令"
background-image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=746080144,3992674839&fm=27&gp=0.jpg
date:  2016-08-02
category: mysql
tags:
- nginx
- mysql
- 负载均衡
---


安装mysql后,需要对数据库进行基本的配置操作,下面是平时常用的一些操作命令

## Mysql配置
### 1. 设置外网用户可访问
打开：  
`/etc/mysql/my.cnf`
修改该项为:  
`bind-address            = 0.0.0.0`

用root登录mysql,修改host为任意主机,并刷新权限到内存中。
执行命令:
```
mysql> update `user` set `host` = '%' where `user` = 'root';  
mysql> flush privileges; 
```

## Mysql操作
### 数据库操作
#### 1. 登录mysql
`mysql -u root -p`

### 2. 刷新更改的配置,使其即可生效
`flush privileges;`

### 用户操作
#### 1. 新建用户
> 注意：此处的Host列，是指该用户在本地登录还是在远程登录.
>如果本地，写:"localhost";如果远程，写:"%",想远程登录的话.

`INSERT INTO mysql.user(Host,User,Password) values("%","webdba",password("**********"));`

或

`CREATE USER 'webdba'@'%' IDENTIFIED BY '**********'; `

#### 2. 给用户授权
> 格式：grant 权限 on 数据库.* to 用户名@登录主机 identified by "密码";

##### 所有权限
`grant all privileges on bbs.* to webdba@"%" identified by "**********";`

##### 部分权限
`grant select,delete,update,insert,create on bbs.* to test@"%" identified by "**********";`


#### 3. 回收root管理员密码

`SET PASSWORD FOR 'root'@'%' = PASSWORD('**********');`

或

`update mysql.user set password=password('新密码') where User="root" and Host="%";`


### 表操作
#### 1. 修改表名
`alter table test rename test1; `

#### 2. 给表名增加注释
`alter table td_user_refund_type comment='退款原因类型表';`

#### 3. 添加表列
`alter table test add  column name varchar(10);`
 
#### 4. 修改表列名
`alter table td_user_refund_type change  column titile title varchar(30);`

#### 5. 修改列属性
`alter table td_user_refund_type modify create_time int(11) default null comment '创建时间';`

#### 6. 删除表列
`alter table test drop  column name; `

#### 7. 给列增加注释
`alter table table_name modify column_name  datetime default null comment '这是字段的注释';`

#### 8. 给表字段增加唯一性约束 
`alter table td_user_weixin add unique(wx_openid);`





 


