---
layout: blog
istop: true
title: "mysql 语句索引优化"
background-image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=746080144,3992674839&fm=27&gp=0.jpg
date:  2016-08-02
category: mysql
tags:
- nginx
- mysql
- 索引优化
---


### 准备：使用之前pdo多线程脚本导入1000万条记录的订单表

### 查看表索引
show index from table_name

### 创建普通单列索引
mysql>ALTER TABLE table_name ADD INDEX index_name ( column );

### 练习语句
SELECT order_id FROM orders LIMIT 6199000, 1000;
-- limit测试
SELECT order_id FROM orders order by order_id asc LIMIT 6199000, 1000;
-- count，where测试
select count(1) from orders where add_time='2017-01-20 13:12:45' order by order_id asc;
-- 字段、where测试
select order_id,order_num,order_price,add_time from orders where add_time='2017-01-20 13:12:45' order by order_id asc;
-- 字段、where多字段测试
select order_id,order_num,order_price,add_time from orders where add_time='2017-01-20 13:12:45' and order_price<100 order by order_id asc;
-- 字段、where多字段or测试
select order_id,order_num,order_price,add_time from orders where add_time='2017-01-20 13:12:45' or order_price<100 order by order_id asc;
-- 字段、where多字段 like
select order_id,order_num,order_price,add_time from orders where add_time='2017-01-20 13:12:45' and order_price like '%100%' order by order_id asc;

### 组合索引练习
组合索引规则：最左优先
创建多列索引
mysql>ALTER TABLE table_name ADD INDEX index_name ( column1, column2, column3 );

### 大数据量导入优化
alter table film_test2 disable keys;
导入操作
alter table film_test2 enable keys;
#添加复合索引
alter table goods_order add index idx_goods(goods_id,goods_father,user_id);
### 索引练习实例
explain select goods_id from goods_order where goods_id<10000 
explain select goods_id from goods_order where goods_father=3 
explain select goods_id from goods_order where user_id in (2,3,4,5,6)

explain select * from goods_order where goods_id<10000 and goods_father=3 and user_id in (2,3,4,5,6)

explain select from goods_order where goods_father=3 and user_id in (2,3,4,5,6)
explain select from goods_order where user_id in (2,3,4,5,6)
explain select * from goods_order where user_id in (2,3,4,5,6) and goods_father=3 and goods_id<10000