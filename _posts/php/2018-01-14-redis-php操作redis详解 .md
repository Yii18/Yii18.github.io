---
layout: blog
banana: true
category: phpredis
title:  "php操作redis详解"
date:   2018-01-14 20:06:42
background-image: https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=3081598215,534061352&fm=27&gp=0.jpg
tags:
- redis
- mysql
- laravel
- PHP性能调优
---

php redis 详细操作

### 1.Connection

	 $redis = ne

	//短链接，本地host，端口为6379，超过1秒放弃链接w Redis(); 
	$redis->connect(‘127.0.0.1’,6379,1);
	
	//短链接(同上) 
	 $redis->open(‘127.0.0.1’,6379,1);

	//长链接，本地host，端口为6379，超过1秒放弃链接 
	$redis->pconnect(‘127.0.0.1’,6379,1);

	//长链接(同上) 
	$redis->popen(‘127.0.0.1’,6379,1);

	//登录验证密码，返回【true | false】
	$redis->auth(‘password’);

	//选择redis库,0~15 共16个库 
	 $redis->select(0);

	//释放资源 
	$redis->close();

	//检查是否还再链接,[+pong] 
	$redis->ping(); 

	//查看失效时间[-1 | timestamps] 
	$redis->ttl(‘key’);

	//移除失效时间[ 1 | 0]
	$redis->persist(‘key’);

	//返回或保存给定列表、集合、有序集合key中经过排序的元素，$array为参数limit等！【配合$array很强大】 [array|false]
	 $redis->sort(‘key’,[$array]);

### 2.共性的运算归类

	//设置失效时间[true | false] 
	$redis->expire(‘key’,10);

	//把当前库中的key移动到15库中[0|1]
	$redis->move(‘key’,15);
	
	//string 
	//获取当前key的长度 
	$redis->strlen(‘key’);

	//把string追加到key现有的value中[>追加后的个数] 
	$redis->append(‘key’,’string’);

	//自增1，如不存在key,赋值为1(只对整数有效,存储以10进制64位，redis中为str)[new_num | false]
	$redis->incr(‘key’);

	//自增$num,不存在为赋值,值需为整数[new_num | false] 
	 $redis->incrby(‘key’,$num);

	//自减1，[new_num | false]
	$redis->decr(‘key’);

	//自减$num，[ new_num | false] 
	 $redis->decrby(‘key’,$num);

	//key=value，有效期为10秒[true] 
	$redis->setex(‘key’,10,’value’);
	
	//list 
	//返回列表key的长度,不存在key返回0， [ len | 0] 
	$redis->llen(‘key’);
	
	//set

	//返回集合key的基数(集合中元素的数量)。[num | 0] 
	 $redis->scard(‘key’);

	//移动，将member元素从key1集合移动到key2集合。[1 | 0] 
	$redis->sMove(‘key1’, ‘key2’, ‘member’);

	//Zset
	//返回集合key的基数(集合中元素的数量)。[num | 0]
	 $redis->zcard(‘key’);

	//返回有序集key中，score值在min和max之间(默认包括score值等于min或max)的成员。[num | 0] 
	 $redis->zcount(‘key’,0,-1);

	
	//hash 
	//查看hash中是否存在field,[1 | 0] 
	$redis->hexists(‘key’,’field’);

	//为哈希表key中的域field的值加上量(+|-)num,[new_num | false] $redis->hlen(‘key’);//返回哈希表key中域的数量。[ num | 0]
	$redis->hincrby(‘key’,’field’,$int_num);
 
### 3.Server

	//返回当前库中的key的个数
	 $redis->dbSize();

	//清空整个redis[总true] 
	 $redis->flushAll();

	//清空当前redis库[总true] 
	$redis->flushDB();

	//同步??把数据存储到磁盘-dump.rdb[true]
	$redis->save();

	//异步？？把数据存储到磁盘-dump.rdb[true]
	 $redis->bgsave();

	//查询当前redis的状态 [verson:2.4.5….] 
	 $redis->info();

	//上次存储时间key的时间[timestamp]
	$redis->lastSave();

	//监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断 [true] 
	$redis->watch(‘key’,’keyn’);

	//取消监视一个(或多个) key [true]
	$redis->unwatch(‘key’,’keyn’);

	//开启事务，事务块内的多条命令会按照先后顺序被放进一个队列当中，最后由 EXEC 命令在一个原子时间内执行。 
	 $redis->multi(Redis::MULTI);

	//开启管道，事务块内的多条命令会按照先后顺序被放进一个队列当中，最后由 EXEC 命令在一个原子时间内执行。
	$redis->multi(Redis::PIPELINE);

	//执行所有事务块内的命令，；【事务块内所有命令的返回值，按命令执行的先后顺序排列，当操作被打断时，返回空值 false】
	 $redis->exec();
	 
### 4.String，键值对，创建更新同操作

	//设置表前缀为hf_ 
	$redis->setOption(Redis::OPT_PREFIX,’hf_’);

	//设置key=aa value=1 [true] 
	$redis->set(‘key’,1);

	//设置一个或多个键值[true] 
	$redis->mset($arr);

	//key=value,key存在返回false[|true] 
	$redis->setnx(‘key’,’value’);

	//获取key [value]
	$redis->get(‘key’);

	//(string|arr),返回所查询键的值 
	 $redis->mget($arr);

	//(string|arr)删除key，支持数组批量删除【返回删除个数】
	$redis->del($key_arr);

	//删除keys,[del_num] 
	$redis->delete($key_str,$key2,$key3);

	//先获得key的值，然后重新赋值,[old_value | false]
	$redis->getset(‘old_key’,’new_value’);
 
5.List栈的结构,注意表头表尾,创建更新分开操作

	//增，只能将一个值value插入到列表key的表头，不存在就创建 [列表的长度 |false]
	 $redis->lpush(‘key’,’value’);

	//增，只能将一个值value插入到列表key的表尾 [列表的长度 |false]  $redis->rpush(‘key’,’value’);

	//增，将值value插入到列表key当中，位于值value之前或之后。[new_len | false]
	$redis->lInsert(‘key’, Redis::AFTER, ‘value’, ‘new_value’);

	//增，只能将一个值value插入到列表key的表头，不存在不创建 [列表的长度 |false]
	$redis->lpushx(‘key’,’value’);

	//增，只能将一个值value插入到列表key的表尾，不存在不创建 [列表的长度 |false] $redis->rpushx(‘key’,’value’);

	//删，移除并返回列表key的头元素,[被删元素 | false] 
	$redis->lpop(‘key’);

	//删，移除并返回列表key的尾元素,[被删元素 | false] 
	 $redis->rpop(‘key’);

	//删，根据参数count的值，移除列表中与参数value相等的元素count=(0|-n表头向尾|+n表尾向头移除n个value) [被移除的数量 | 0] 
	$redis->lrem(‘key’,’value’,0);

	//删，列表修剪，保留(start,end)之间的值 [true|false]
	$redis->ltrim(‘key’,start,end);

	//改，从表头数，将列表key下标为第index的元素的值为new_v, [true | false] 
	 $redis->lset(‘key’,index,’new_v’);

	//查，返回列表key中，下标为index的元素[value|false] 
	$redis->lindex(‘key’,index);

	//查，(start,stop|0,-1)返回列表key中指定区间内的元素，区间以偏移量start和stop指定。[array|false]
	$redis->lrange(‘key’,0,-1);
	
### /*6.Set，没有重复的member，创建更新同操作*/ 

	//增，改，将一个或多个member元素加入到集合key当中，已经存在于集合的member元素将被忽略。[insert_num] 
	$redis->sadd(‘key’,’value1′,’value2′,’valuen’);

	//删，移除集合key中的一个或多个member元素，不存在的member元素会被忽略 [del_num | false] 
	$redis->srem(‘key’,’value1′,’value2′,’valuen’);


	//查，返回集合key中的所有成员 [array | ”] 
	$redis->smembers(‘key’);

	//判断member元素是否是集合key的成员 [1 | 0] 
	$redis->sismember(‘key’,’member’);

	//删，移除并返回集合中的一个随机元素 [member | false] 
	$redis->spop(‘key’);

	//查，返回集合中的一个随机元素 [member | false] 
	$redis->srandmember(‘key’);
	
	//查，返回所有给定集合的交集 [array | false]
	$redis->sinter(‘key1′,’key2′,’keyn’);

	//查，返回所有给定集合的并集 [array | false] 
	 $redis->sunion(‘key1′,’key2′,’keyn’);

	//查，返回所有给定集合的差集 [array | false]
	$redis->sdiff(‘key1′,’key2′,’keyn’);

7.Zset，没有重复的member，有排序顺序,创建更新同操作
	
	//增，改，将一个或多个member元素及其score值加入到有序集key当中。[num | 0] 
	$redis->zAdd(‘key’,$score1,$member1,$scoreN,$memberN);
	
	//删，移除有序集key中的一个或多个成员，不存在的成员将被忽略。[del_num | 0]
	$redis->zrem(‘key’,’member1′,’membern’);
	 
	//查,通过值反拿权 [num | null]
	$redis->zscore(‘key’,’member’);
	
	//查，通过(score从小到大)【排序名次范围】拿member值，返回有序集key中，【指定区间内】的成员 [array | null] 
	$redis->zrange(‘key’,$start,$stop);

	//查，通过(score从大到小)【排序名次范围】拿member值，返回有序集key中，【指定区间内】的成员 [array | null] 
	$redis->zrevrange(‘key’,$start,$stop);
	
	//查，通过scroe权范围拿member值，返回有序集key中，指定区间内的(从小到大排)成员[array | null] 
	$redis->zrangebyscore(‘key’,$min,$max[,$config]);
	
	//查，通过scroe权范围拿member值，返回有序集key中，指定区间内的(从大到小排)
	成员[array | null] 
	$redis->zrevrangebyscore(‘key’,$max,$min[,$config]);

	//查，通过member值查(score从小到大)排名结果中的【member排序名次】[order | null]
	$redis->zrank(‘key’,’member’);
	
	//查，通过member值查(score从大到小)排名结果中的【member排序名次】[order | null] 
	$redis->zrevrank(‘key’,’member’);

	//交集 $redis->ZUNIONSTORE();//差集
	$redis->ZINTERSTORE();

### 8.Hash，表结构，创建更新同操作

	//增，改，将哈希表key中的域field的值设为value,不存在创建,存在就覆盖【1 | 0】
	$redis->hset(‘key’,’field’,’value’);

	 //查，取值【value|false】 
	$redis->hget(‘key’,’field’);

	$arr = array(‘one’=>1,2,3);
	$arr2 = array(‘one’,0,1);

	//增，改，设置多值$arr为(索引|关联)数组,$arr[key]=field, [ true ] 
	 $redis->hmset(‘key’,$arr);

	//查，获取指定下标的field，[$arr | false]
	$redis->hmget(‘key’,$arr2);

	//查，返回哈希表key中的所有域和值。[当key不存在时，返回一个空表]
	$redis->hgetall(‘key’);

	//查，返回哈希表key中的所有域。[当key不存在时，返回一个空表] 
	 $redis->hkeys(‘key’);

	//查，返回哈希表key中的所有值。[当key不存在时，返回一个空表] 
	$redis->hvals(‘key’);

	//删，删除指定下标的field,不存在的域将被忽略,[num | false]
	$redis->hdel(‘key’,$arr2);
