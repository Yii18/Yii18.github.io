---
layout: blog
banana: true
category: Nosql
title:  根据status信息进行mysql服务器优化
date:   2018-02-03 16:06:42
background-image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=4049646291,2411973880&fm=27&gp=0.jpg
tags:
- Redis
- 架构之路
- Nosql
- road
---
根据服务器的状态进行优化

["参考地址"](http://www.jb51.net/article/28290.htm ,"参考地址")
## 慢查询
1.打开慢查询日志可能会对系统性能有一点点影响   
2.如果你的MySQL是主－从结构，可以考虑打开其中一台从服务器的慢查询日志，这样既可以监控慢查询，对系统性能影响又小。
## 链接数
>经常会遇见 MySQL: ERROR 1040: Too many connections”的情况  
一种是访问特别高，mysql扛不住
这时候要考虑增加从服务器来分散读压力
另一种是mysql配置文件中max_connections 值过小
Key_buffer_size
key_buffer_size是对MyISAM表性能影响最大的一个参数，下面一台以MyISAM为主要存储引擎服务器的配置：
## 临时表
1.每次创建临时表
>Created_tmp_tables增加，如果是在磁盘上创建临时表，Created_tmp_disk_tables也增加,Created_tmp_files表示MySQL服务创建的临时文件文件数，比较理想的配置是：
只有256MB以下的临时表才能全部放内存，超过的就会用到硬盘临时表。

2.Open Table情况
>Open_tables表示打开表的数量，Opened_tables表示打开过的表数量，如果Opened_tables数量过大，说明配置中 table_cache(5.1.3之后这个值叫做table_open_cache)值可能太小，我们查询一下服务器table_cache值：
## 进程使用情况
>如果我们在MySQL服务器配置文件中设置了thread_cache_size，当客户端断开之后，服务器处理此客户的线程将会缓存起来以响应下一个客户而不是销毁（前提是缓存数未达上限）。Threads_created表示创建过的线程数，如果发现Threads_created值过大的话，表明MySQL服务器一直在创建线程，这也是比较耗资源，可以适当增加配置文件中thread_cache_size值，查询服务器 thread_cache_size配置：
## 查询缓存(query cache)
1.MySQL查询缓存变量解释：

2.Qcache_free_blocks：缓存中相邻内存块的个数。数目大说明可能有碎片。FLUSH QUERY CACHE会对缓存中的碎片进行整理，从而得到一个空闲块。

3.Qcache_free_memory：缓存中的空闲内存。

4.Qcache_hits：每次查询在缓存中命中时就增大

5.Qcache_inserts：每次插入一个查询时就增大。命中次数除以插入次数就是不中比率。

6.Qcache_lowmem_prunes：缓存出现内存不足并且必须要进行清理以便为更多查询提供空间的次数。这个数字最好长时间来看；如果这个数字在不断增长，就表示可能碎片非常严重，或者内存很少。（上面的 free_blocks和free_memory可以告诉您属于哪种情况）

7.Qcache_not_cached：不适合进行缓存的查询的数量，通常是由于这些查询不是 SELECT 语句或者用了now()之类的函数。

8.Qcache_queries_in_cache：当前缓存的查询（和响应）的数量。

9.Qcache_total_blocks：缓存中块的数量。

10.各字段的解释：

11.query_cache_limit：超过此大小的查询将不缓存

12.query_cache_min_res_unit：缓存块的最小大小

13.query_cache_size：查询缓存大小

14.query_cache_type：缓存类型，决定缓存什么样的查询，示例中表示不缓存 select sql_no_cache 查询

15.query_cache_wlock_invalidate：当有其他客户端正在对MyISAM表进行写操作时，如果查询在query cache中，是否返回cache结果还是等写操作完成再读表获取结果。

16.query_cache_min_res_unit的配置是一柄”双刃剑”，默认是4KB，设置值大对大数据查询有好处，但如果你的查询都是小数据查询，就容易造成内存碎片和浪费。

17.查询缓存碎片率 = Qcache_free_blocks / Qcache_total_blocks * 100%

19.如果查询缓存碎片率超过20%，可以用FLUSH QUERY CACHE整理缓存碎片，或者试试减小query_cache_min_res_unit，如果你的查询都是小数据量的话。

20.查询缓存利用率 = (query_cache_size – Qcache_free_memory) / query_cache_size * 100%

21.查询缓存利用率在25%以下的话说明query_cache_size设置的过大，可适当减小；查询缓存利用率在80％以上而且Qcache_lowmem_prunes > 50的话说明query_cache_size可能有点小，要不就是碎片太多。

22.查询缓存命中率 = (Qcache_hits – Qcache_inserts) / Qcache_hits * 100%

23.示例服务器 查询缓存碎片率 ＝ 20.46％，查询缓存利用率 ＝ 62.26％，查询缓存命中率 ＝ 1.94％，命中率很差，可能写操作比较频繁吧，而且可能有些碎片。

## 排序使用情况
Sort_merge_passes 包括两步.
mysql首先会在内存中做排序	
文件打开数(open_files)
表锁情况
表扫描情况
