---
layout: blog
istop: true
category: mysql
title: "explain 用法详解"
background-image: data:image/jpeg;base64,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
date:  2017-05-20 20:02
tags:
- nginx
- mysql
- 索引优化
- 分区分表
---

explain < table_name >

例如：

explain select * from t3 where id=3952602;

二、explain输出解释

+----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+
| id | select_type | table | type  | possible_keys     | key     | key_len | ref   | rows | Extra |
+----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+

1 id

SQL执行的顺利的标识，SQL从大到小的执行。

例如:

mysql> explain select * from (select * from ( select * from t3 where id=3952602) a) b;
+----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
| id | select_type | table      | type   | possible_keys     | key     | key_len | ref  | rows | Extra |
+----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
|  1 | PRIMARY     | <derived2> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
|  2 | DERIVED     | <derived3> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
|  3 | DERIVED     | t3         | const  | PRIMARY,idx_t3_id | PRIMARY | 4       |      |    1 |       |
+----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+

很显然这条SQL是从里向外的执行，就是从id=3 向上执行。
2 select_type

就是select类型，可以有以下几种

（1）SIMPLE

简单SELECT（不使用UNION或子查询等）。

例如：

mysql> explain select * from t3 where id=3952602;
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+
 | id | select_type | table | type  | possible_keys     | key     | key_len | ref   | rows | Extra |
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+
 |  1 | SIMPLE      | t3    | const | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 |       |
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+

（2）PRIMARY

就是最外层的select。

例如:

mysql> explain select * from (select * from t3 where id=3952602) a ;
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 | id | select_type | table      | type   | possible_keys     | key     | key_len | ref  | rows | Extra |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 |  1 | PRIMARY     | <derived2> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
 |  2 | DERIVED     | t3         | const  | PRIMARY,idx_t3_id | PRIMARY | 4       |      |    1 |       |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+

（3）UNION

UNION中的第二个或后面的SELECT语句。

例如

mysql> explain select * from t3 where id=3952602 union all select * from t3 ;
 +----+--------------+------------+-------+-------------------+---------+---------+-------+------+-------+
 | id | select_type  | table      | type  | possible_keys     | key     | key_len | ref   | rows | Extra |
 +----+--------------+------------+-------+-------------------+---------+---------+-------+------+-------+
 |  1 | PRIMARY      | t3         | const | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 |       |
 |  2 | UNION        | t3         | ALL   | NULL              | NULL    | NULL    | NULL  | 1000 |       |
 |NULL | UNION RESULT | <union1,2> | ALL   | NULL              | NULL    | NULL    | NULL  | NULL |       |
 +----+--------------+------------+-------+-------------------+---------+---------+-------+------+-------+

（4）DEPENDENT UNION

UNION中的第二个或后面的SELECT语句，取决于外面的查询。

mysql> explain select * from t3 where id in (select id from t3 where id=3952602 union all select id from t3)  ;
 +----+--------------------+------------+--------+-------------------+---------+---------+-------+------+--------------------------+
 | id | select_type        | table      | type   | possible_keys     | key     | key_len | ref   | rows | Extra                    |
 +----+--------------------+------------+--------+-------------------+---------+---------+-------+------+--------------------------+
 |  1 | PRIMARY            | t3         | ALL    | NULL              | NULL    | NULL    | NULL  | 1000 | Using where              |
 |  2 | DEPENDENT SUBQUERY | t3         | const  | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 | Using index              |
 |  3 | DEPENDENT UNION    | t3         | eq_ref | PRIMARY,idx_t3_id | PRIMARY | 4       | func  |    1 | Using where; Using index |
 |NULL | UNION RESULT       | <union2,3> | ALL    | NULL              | NULL    | NULL    | NULL  | NULL |                          |
 +----+--------------------+------------+--------+-------------------+---------+---------+-------+------+--------------------------+

（4）UNION RESULT

UNION的结果。

mysql> explain select * from t3 where id=3952602 union all select * from t3 ;
 +----+--------------+------------+-------+-------------------+---------+---------+-------+------+-------+
 | id | select_type  | table      | type  | possible_keys     | key     | key_len | ref   | rows | Extra |
 +----+--------------+------------+-------+-------------------+---------+---------+-------+------+-------+
 |  1 | PRIMARY      | t3         | const | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 |       |
 |  2 | UNION        | t3         | ALL   | NULL              | NULL    | NULL    | NULL  | 1000 |       |
 |NULL | UNION RESULT | <union1,2> | ALL   | NULL              | NULL    | NULL    | NULL  | NULL |       |
 +----+--------------+------------+-------+-------------------+---------+---------+-------+------+-------+

（5）SUBQUERY

子查询中的第一个SELECT。

mysql> explain select * from t3 where id = (select id from t3 where id=3952602 )  ;
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------------+
 | id | select_type | table | type  | possible_keys     | key     | key_len | ref   | rows | Extra       |
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------------+
 |  1 | PRIMARY     | t3    | const | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 |             |
 |  2 | SUBQUERY    | t3    | const | PRIMARY,idx_t3_id | PRIMARY | 4       |       |    1 | Using index |
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------------+

（6）  DEPENDENT SUBQUERY

子查询中的第一个SELECT，取决于外面的查询。

mysql> explain select id from t3 where id in (select id from t3 where id=3952602 )  ;
 +----+--------------------+-------+-------+-------------------+---------+---------+-------+------+--------------------------+
 | id | select_type        | table | type  | possible_keys     | key     | key_len | ref   | rows | Extra                    |
 +----+--------------------+-------+-------+-------------------+---------+---------+-------+------+--------------------------+
 |  1 | PRIMARY            | t3    | index | NULL              | PRIMARY | 4       | NULL  | 1000 | Using where; Using index |
 |  2 | DEPENDENT SUBQUERY | t3    | const | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 | Using index              |
 +----+--------------------+-------+-------+-------------------+---------+---------+-------+------+--------------------------+

（7）DERIVED

派生表的SELECT（FROM子句的子查询）。

mysql> explain select * from (select * from t3 where id=3952602) a ;
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 | id | select_type | table      | type   | possible_keys     | key     | key_len | ref  | rows | Extra |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 |  1 | PRIMARY     | <derived2> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
 |  2 | DERIVED     | t3         | const  | PRIMARY,idx_t3_id | PRIMARY | 4       |      |    1 |       |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+

3 table

显示这一行的数据是关于哪张表的。
有时不是真实的表名字，看到的是derivedx（x是个数字，是第几步执行的结果）。

mysql> explain select * from (select * from ( select * from t3 where id=3952602) a) b;
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 | id | select_type | table      | type   | possible_keys     | key     | key_len | ref  | rows | Extra |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 |  1 | PRIMARY     | <derived2> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
 |  2 | DERIVED     | <derived3> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
 |  3 | DERIVED     | t3         | const  | PRIMARY,idx_t3_id | PRIMARY | 4       |      |    1 |       |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+

4 type

这列很重要，显示了连接使用了哪种类别，有无使用索引。
从最好到最差的连接类型为const、eq_reg、ref、range、index和ALL。

（1）system

这是const联接类型的一个特例，表仅有一行满足条件。

如下（t3表上的id是 primary key）

mysql> explain select * from (select * from t3 where id=3952602) a ;
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 | id | select_type | table      | type   | possible_keys     | key     | key_len | ref  | rows | Extra |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+
 |  1 | PRIMARY     | <derived2> | system | NULL              | NULL    | NULL    | NULL |    1 |       |
 |  2 | DERIVED     | t3         | const  | PRIMARY,idx_t3_id | PRIMARY | 4       |      |    1 |       |
 +----+-------------+------------+--------+-------------------+---------+---------+------+------+-------+

（2）const

表最多有一个匹配行，它将在查询开始时被读取。

因为仅有一行，在这行的列值可被优化器剩余部分认为是常数。

const表很快，因为它们只读取一次！

const用于用常数值比较PRIMARY KEY或UNIQUE索引的所有部分时。

在下面的查询中，tbl_name可以用于const表：

SELECT * from tbl_name WHERE primary_key=1；
SELECT * from tbl_name WHERE primary_key_part1=1和 primary_key_part2=2；

例如：

mysql> explain select * from t3 where id=3952602;
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+
 | id | select_type | table | type  | possible_keys     | key     | key_len | ref   | rows | Extra |
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+
 |  1 | SIMPLE      | t3    | const | PRIMARY,idx_t3_id | PRIMARY | 4       | const |    1 |       |
 +----+-------------+-------+-------+-------------------+---------+---------+-------+------+-------+

（3） eq_ref

对于每个来自于前面的表的行组合，从该表中读取一行。

这可能是最好的联接类型，除了const类型。

它用在一个索引的所有部分被联接使用并且索引是UNIQUE或PRIMARY KEY。

eq_ref可以用于使用= 操作符比较的带索引的列。

比较值可以为常量或一个使用在该表前面所读取的表的列的表达式。

在下面的例子中，MySQL可以使用eq_ref联接来处理ref_tables：

SELECT * FROM ref_table,other_table
  WHERE ref_table.key_column=other_table.column;

SELECT * FROM ref_table,other_table
  WHERE ref_table.key_column_part1=other_table.column
  AND ref_table.key_column_part2=1;

例如

mysql> create unique index  idx_t3_id on t3(id) ;
 Query OK, 1000 rows affected (0.03 sec)
 Records: 1000  Duplicates: 0  Warnings: 0

mysql> explain select * from t3,t4 where t3.id=t4.accountid;
 +----+-------------+-------+--------+-------------------+-----------+---------+----------------------+------+-------+
 | id | select_type | table | type   | possible_keys     | key       | key_len | ref                  | rows | Extra |
 +----+-------------+-------+--------+-------------------+-----------+---------+----------------------+------+-------+
 |  1 | SIMPLE      | t4    | ALL    | NULL              | NULL      | NULL    | NULL                 | 1000 |       |
 |  1 | SIMPLE      | t3    | eq_ref | PRIMARY,idx_t3_id | idx_t3_id | 4       | dbatest.t4.accountid |    1 |       |
 +----+-------------+-------+--------+-------------------+-----------+---------+----------------------+------+-------+

（4）ref

对于每个来自于前面的表的行组合，所有有匹配索引值的行将从这张表中读取。

如果联接只使用键的最左边的前缀，或如果键不是UNIQUE或PRIMARY KEY（换句话说，如果联接不能基于关键字选择单个行的话），则使用ref。

如果使用的键仅仅匹配少量行，该联接类型是不错的。

ref可以用于使用=或<=>操作符的带索引的列。

在下面的例子中，MySQL可以使用ref联接来处理ref_tables：

SELECT * FROM ref_table WHERE key_column=expr;

SELECT * FROM ref_table,other_table
  WHERE ref_table.key_column=other_table.column;

SELECT * FROM ref_table,other_table
  WHERE ref_table.key_column_part1=other_table.column
  AND ref_table.key_column_part2=1;

例如:

mysql> drop index idx_t3_id on t3;
 Query OK, 1000 rows affected (0.03 sec)
 Records: 1000  Duplicates: 0  Warnings: 0

mysql> create index idx_t3_id on t3(id) ;
 Query OK, 1000 rows affected (0.04 sec)
 Records: 1000  Duplicates: 0  Warnings: 0

mysql> explain select * from t3,t4 where t3.id=t4.accountid;
 +----+-------------+-------+------+-------------------+-----------+---------+----------------------+------+-------+
 | id | select_type | table | type | possible_keys     | key       | key_len | ref                  | rows | Extra |
 +----+-------------+-------+------+-------------------+-----------+---------+----------------------+------+-------+
 |  1 | SIMPLE      | t4    | ALL  | NULL              | NULL      | NULL    | NULL                 | 1000 |       |
 |  1 | SIMPLE      | t3    | ref  | PRIMARY,idx_t3_id | idx_t3_id | 4       | dbatest.t4.accountid |    1 |       |
 +----+-------------+-------+------+-------------------+-----------+---------+----------------------+------+-------+
 2 rows in set (0.00 sec)

（5）  ref_or_null

该联接类型如同ref，但是添加了MySQL可以专门搜索包含NULL值的行。

在解决子查询中经常使用该联接类型的优化。

在下面的例子中，MySQL可以使用ref_or_null联接来处理ref_tables：

SELECT * FROM ref_table
  WHERE key_column=expr OR key_column IS NULL;

（6） index_merge

该联接类型表示使用了索引合并优化方法。

在这种情况下，key列包含了使用的索引的清单，key_len包含了使用的索引的最长的关键元素。

例如：

mysql> explain select * from t4 where id=3952602 or accountid=31754306 ;
 +----+-------------+-------+-------------+----------------------------+----------------------------+---------+------+------+------------------------------------------------------+
 | id | select_type | table | type        | possible_keys              | key                        | key_len | ref  | rows | Extra                                                |
 +----+-------------+-------+-------------+----------------------------+----------------------------+---------+------+------+------------------------------------------------------+
 |  1 | SIMPLE      | t4    | index_merge | idx_t4_id,idx_t4_accountid | idx_t4_id,idx_t4_accountid | 4,4     | NULL |    2 | Using union(idx_t4_id,idx_t4_accountid); Using where |
 +----+-------------+-------+-------------+----------------------------+----------------------------+---------+------+------+------------------------------------------------------+
 1 row in set (0.00 sec)

（7） unique_subquery

该类型替换了下面形式的IN子查询的ref：

value IN (SELECT primary_key FROM single_table WHERE some_expr)

unique_subquery是一个索引查找函数，可以完全替换子查询，效率更高。

（8）index_subquery

该联接类型类似于unique_subquery，可以替换IN子查询。

但只适合下列形式的子查询中的非唯一索引：

value IN (SELECT key_column FROM single_table WHERE some_expr)

（9）range

只检索给定范围的行，使用一个索引来选择行。

key列显示使用了哪个索引。

key_len包含所使用索引的最长关键元素。

在该类型中ref列为NULL。

当使用=、<>、>、>=、<、<=、IS NULL、<=>、BETWEEN或者IN操作符，用常量比较关键字列时，可以使用range。

mysql> explain select * from t3 where id=3952602 or id=3952603 ;
 +----+-------------+-------+-------+-------------------+-----------+---------+------+------+-------------+
 | id | select_type | table | type  | possible_keys     | key       | key_len | ref  | rows | Extra       |
 +----+-------------+-------+-------+-------------------+-----------+---------+------+------+-------------+
 |  1 | SIMPLE      | t3    | range | PRIMARY,idx_t3_id | idx_t3_id | 4       | NULL |    2 | Using where |
 +----+-------------+-------+-------+-------------------+-----------+---------+------+------+-------------+
 1 row in set (0.02 sec)

（10）index

该联接类型与ALL相同，除了只有索引树被扫描。

这通常比ALL快，因为索引文件通常比数据文件小。

当查询只使用作为单索引一部分的列时，MySQL可以使用该联接类型。

（11） ALL

对于每个来自于先前的表的行组合，进行完整的表扫描。

如果表是第一个没标记const的表，这通常不好，并且通常在它情况下很差。

通常可以增加更多的索引而不要使用ALL，使得行能基于前面的表中的常数值或列值被检索出。
5 possible_keys

possible_keys列指出MySQL能使用哪个索引在该表中找到行。

注意，该列完全独立于EXPLAIN输出所示的表的次序。

这意味着在possible_keys中的某些键，实际上不能按生成的表次序使用。

如果该列是NULL，则没有相关的索引。

在这种情况下，可以通过检查WHERE子句，看是否它引用某些列，或适合索引的列来提高你的查询性能。

如果是这样，创造一个适当的索引并且再次用EXPLAIN检查查询
6 key

key列显示MySQL实际决定使用的键（索引）。

如果没有选择索引，键是NULL。

要想强制MySQL使用、或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。
7 key_len

key_len列显示MySQL决定使用的键长度。

如果键是NULL，则长度为NULL。

使用的索引的长度。在不损失精确性的情况下，长度越短越好
8 ref

ref列显示使用哪个列，或常数与key一起从表中选择行。
9 rows

rows列显示MySQL认为它执行查询时必须检查的行数。
10 Extra

该列包含MySQL解决查询的详细信息，下面详细介绍。

（1）Distinct

一旦MYSQL找到了与行相联合匹配的行，就不再搜索了

（2）Not exists

MYSQL优化了LEFT JOIN，一旦它找到了匹配LEFT JOIN标准的行，就不再搜索了

（3）Range checked for each

Record（index map:#）

没有找到理想的索引，因此对于从前面表中来的每一个行组合，MYSQL检查使用哪个索引，并用它来从表中返回行。

这是使用索引的最慢的连接之一。

（4）Using filesort

看到这个的时候，查询就需要优化了。

MYSQL需要进行额外的步骤，来发现如何对返回的行排序。

它根据连接类型、以及存储排序键值、和匹配条件的全部行的行指针来排序全部行。

（5）Using index

列数据是从仅仅使用了索引中的信息，而没有读取实际的行动的表返回的。

这发生在，对表的全部的请求列都是同一个索引的部分的时候。

（6）Using temporary

看到这个的时候，查询需要优化了。

这里，MYSQL需要创建一个临时表来存储结果。

这通常发生在对不同的列集进行ORDER BY上，而不是GROUP BY上。

（7）Using where
使用了WHERE从句来限制哪些行将与下一张表匹配或者是返回给用户。

如果不想返回表中的全部行，并且连接类型ALL或index，这就会发生，或者是查询有问题。
