---
layout: blog
banana: true
category: 负载均衡
title: elasticsearch安装 
date:   2018-01-15 22:06:42
background-image: https://lccdn.phphub.org/uploads/images/201804/19/10512/Ncx25uq2lR.png?imageView2/2/w/1240/h/0
tags:
- Redis
- 架构之路
- Nosql
- 负载均衡
- road	
---
laravel-china.org
Elastcisearch 的配置与使用，为了全文搜索 | Laravel China 社区
PHPHub
39 - 49 分钟

file

        最近公司项目要使用全文搜索引擎，之前使用过的 sphInx ,似乎没有那么好用了，而且中文分词也没有合适的 ，所以准备换个其它的来试试，老项目使用的是 thinkphp 3.1 框架，虽然框架老了点。但是新的想法还是可以用上的，这里只是简单演示下 elasticsearch 的上手体难，实际项目中还需要完善。

Elasticsearch 安装

因本文环境为 Laradock ,所以直接使用 elasticsearch的镜像即可，这里省略了 java 环境的安装及 elasticsearch 软件的安装，网上教程很多，请自行查找，后期会补上一个,这里默认已经安装好了。
浏览器打开 http://localhost:9200/ 或者终端执行

curl 'http:

你会看到如下响应

{
        name: "g2ODObY",
        cluster_name: "laradock-cluster",
        cluster_uuid: "w8Hhov2bQDi_Wo2DEx044Q",
        version: {
        number: "6.2.3",
        build_hash: "c59ff00",
        build_date: "2018-03-13T10:06:29.741383Z",
        build_snapshot: false,
        lucene_version: "7.2.1",
        minimum_wire_compatibility_version: "5.6.0",
        minimum_index_compatibility_version: "5.0.0"
        },
        tagline: "You Know, for Search"
}

如果响应正常显示，说明你安装成功了。
这里需注意一点，查看elasticsearch配置：vi config/elasticsearch.yml

network.host: 0.0.0.0  

Elasticsearch 中文插件

这里使用的是 analysis-ik 中文插件，项目地址，需根据不同的 Elasticsearch 版本选择插本版本，本项目使用的最新的 6.2.3 版本。
进入 elasticsearch 目录

./bin/elasticsearch-plugin install https:

查看是否安装成功(注意你的 elasticsearch 版本，版本不同命令不同)

./bin/elasticsearch-plugin list

返回结果

analysis-ik
ingest-geoip
ingest-user-agent
...

看到 analysis-ik 证明你插件安装成功了，你也可以到 plugins 目录下查看插件是否存在

$ cd plugins 

drwxr-xr-x  2 root          root 4096 Apr 17 03:08 analysis-ik
drwxrwxr-x  2 elasticsearch root 4096 Mar 13 11:35 ingest-geoip
drwxrwxr-x  2 elasticsearch root 4096 Mar 13 11:35 ingest-user-agent
drwxrwxr-x 11 elasticsearch root 4096 Mar 13 11:35 x-pack

Elasticsearch 索引的使用

Thinkphp 没有 laravel 那么方便，但是用个 trait 还是可以的。
新建一个traits

<?php
use Elasticsearch\ClientBuilder;
trait Elastic
{
    private $client;
    public function __construct()
    {
        $hosts=[
            env('ELASTICSEARCH_URL','localhost:9200')
        ];
        $this->client = ClientBuilder::create()
            ->setHosts($hosts)  
            ->build();
    }
}       

laravel 的 env 实现其它是用的 vlucas/phpdotenv，所以我在 Thinkphp中也把他拿了过来。
这里首先实例化一个 client。
创建索引

        $params = [
            'index' => $index,
            'body' => [
                'settings' => [
                    'number_of_shards' => 1, 
                    'number_of_replicas' => 0 
                ],
                'mappings' => [
                    $type => [  
                        '_all'=>[   
                            'enabled' => 'false'
                        ],
                        '_source' => [ 
                            'enabled' => true
                        ],
                        'properties' => [   
                            'id' => [
                                'type' => 'integer', 
                                

                            ],
                            'title' => [
                                'type' => 'text', 
                                "analyzer"=> "ik_max_word",
                                "search_analyzer"=> "ik_max_word",
                            ],
                            'body'  =>  [
                                'type'  => 'text',
                                "analyzer"=> "ik_max_word",
                                "search_analyzer"=> "ik_max_word",
                            ]
                        ]
                    ]
                ]
            ]
        ];
        return $this->client->indices()->create($params);

这里需要注意的是 analyzer, IK插件目前只支持两种： ik_max_word 和ik_smart，

    ik_max_word: 会将文本做最细粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,中华人民,中华,华人,人民共和国,人民,人,民,共和国,共和,和,国国,国歌”，会穷尽各种可能的组合；
    ik_smart : 会做最粗粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,国歌”。

返回如下信息说明创建成功

array:3 [▼
  "acknowledged" => true
  "shards_acknowledged" => true
  "index" => "my_index"
]

这里使用的 dd打印的结果，之后的结果承现一样用 dd打印。
删除索引

$params = [
            'index' => 'my_index',
        ];
 return $this->client->indices()->delete($params);

返回如下

array:1 [▼
  "acknowledged" => true
]

查看索引设置


$params = ['index' => 'my_index'];
$response = $client->indices()->getSettings($params);


$params = [
    'index' => [ 'my_index', 'my_index2' ]
];
$response = $client->indices()->getSettings($params);

返回信息如下

array:1 [▼
  "my_index" => array:1 [▼
    "settings" => array:1 [▼
      "index" => array:6 [▼
        "creation_date" => "1524037463950"
        "number_of_shards" => "1"
        "number_of_replicas" => "0"
        "uuid" => "okYiWK0WRiqebMAHUCsvzA"
        "version" => array:1 [▼
          "created" => "6020399"
        ]
        "provided_name" => "my_index"
      ]
    ]
  ]
]

查看 mapping 信息


$response = $client->indices()->getMapping();


$params = ['index' => 'my_index'];
$response = $client->indices()->getMapping($params);


$params = ['type' => 'my_type' ];
$response = $client->indices()->getMapping($params);


$params = [
    'index' => 'my_index'
    'type' => 'my_type'
];
$response = $client->indices()->getMapping($params);


$params = [
    'index' => [ 'my_index', 'my_index2' ]
];
$response = $client->indices()->getMapping($params);

返回如下代码

array:1 [▼
  "my_index" => array:1 [▼
    "mappings" => array:1 [▼
      "my_type" => array:2 [▼
        "_all" => array:1 [▼
          "enabled" => false
        ]
        "properties" => array:3 [▼
          "body" => array:2 [▼
            "type" => "text"
            "analyzer" => "ik_max_word"
          ]
          "id" => array:1 [▼
            "type" => "integer"
          ]
          "title" => array:2 [▼
            "type" => "text"
            "analyzer" => "ik_max_word"
          ]
        ]
      ]
    ]
  ]
]

Elasticsearch 的增删改查
增加数据

1.增加单条数据

$data=[
            'title' => '我爱北京天安门',
            'body'  =>  '天安门上太阳升'
        ];
$params = [
            'index' => 'my_index',
            'type' => 'my_type',
           
            'body' => $data
        ];
        return $this->client->index($params);

返回如下

array:8 [▼
  "_index" => "my_index"
  "_type" => "my_type"
  "_id" => "WKXd12IBwuLBOSSKe5k_" 
  "_version" => 1
  "result" => "created"
  "_shards" => array:3 [▼
    "total" => 2
    "successful" => 1
    "failed" => 0
  ]
  "_seq_no" => 0
  "_primary_term" => 1
]

2.批量增加多条数据

$dataList =[
            [
                'id'    =>  '10001',
                'title' => '北京',
                'body' => '我们是首都',

            ],[
                'id'    =>  '10002',
                'title' => '上海',
                'body' => '啊啦是上海人',
            ],[
                'id'    =>  '10003',
                'title' => '广州',
                'body' => '我们有小蛮腰',

            ],[
                'id'    =>  '10004',
                'title' => '深圳',
                'body' => '我们啥也没有，来了就是深圳人。',
            ],
        ];

foreach($dataList as $value){
    $params['body'][] = [
        'index' => [
            '_index' => 'my_index',
            '_type' => 'my_type',
            '_id'  =>$value['id']
        ]
    ];
    $params['body'][] = [
        'id' => $value['id'],
        'title' => $value['title'],
        'body' => $value['body'],
    ];
}
return $this->client->bulk($params);

这里需注意，批量增加多条数据时并不是直接将数组扔进去，而是要进行处理，生成对应的数组后使用 bulk 方法批量创建。
返回如下

array:3 [▼
  "took" => 27
  "errors" => false
  "items" => array:4 [▼
    0 => array:1 [▼
      "index" => array:9 [▼
        "_index" => "my_index"
        "_type" => "my_type"
        "_id" => "10001"
        "_version" => 1
        "result" => "created"
        "_shards" => array:3 [▼
          "total" => 2
          "successful" => 1
          "failed" => 0
        ]
        "_seq_no" => 1
        "_primary_term" => 1
        "status" => 201
      ]
    ]
    1 => array:1 [▼
      "index" => array:9 [▼
        "_index" => "my_index"
        "_type" => "my_type"
        "_id" => "10002"
        "_version" => 1
        "result" => "created"
        "_shards" => array:3 [▼
          "total" => 2
          "successful" => 1
          "failed" => 0
        ]
        "_seq_no" => 2
        "_primary_term" => 1
        "status" => 201
      ]
    ]
        ...

这里使用了 $dataList 自带的 id，在实际项目中建议使用数据的 id,用做数据的唯一 id，方便通过 id 查询数据。
删除数据

删除文档只能单条删除，需指定数据 ID

$param = [
            'index' => 'my_index',
            'type' => 'my_type',
            'id'    => 'my_id' 
        ];
        return  $this->client->delete($param);

返回如下

array:1 [▼
  "acknowledged" => true
]

查询数据

查询数据需指定数据 ID

$params = [
            'index' => 'my_index',
            'type' => 'my_type',
                            'id' => 'WKXd12IBwuLBOSSKe5k_' 
        ];
        return $this->client->get($params);

返回如下

array:6 [▼
  "_index" => "my_index"
  "_type" => "my_type"
  "_id" => "WKXd12IBwuLBOSSKe5k_"
  "_version" => 1
  "found" => true
  "_source" => array:2 [▼
    "title" => "我爱北京天安门"
    "body" => "天安门上太阳升"
  ]
]

数据修改

$params = [
            'index' => 'my_index',
            'type' => 'my_type',
            'id' => 'WKXd12IBwuLBOSSKe5k_',
            'body' => [
                'doc' => [  
                    'age' => 150
                ]
            ]
        ];
return  $this->client->update($params);

返回如下

array:8 [▼
  "_index" => "my_index"
  "_type" => "my_type"
  "_id" => "WKXd12IBwuLBOSSKe5k_"
  "_version" => 2
  "result" => "updated"
  "_shards" => array:3 [▼
    "total" => 2
    "successful" => 1
    "failed" => 0
  ]
  "_seq_no" => 4
  "_primary_term" => 1
]

再次查询数据

array:6 [▼
  "_index" => "my_index"
  "_type" => "my_type"
  "_id" => "WKXd12IBwuLBOSSKe5k_"
  "_version" => 2
  "found" => true
  "_source" => array:3 [▼
    "title" => "我爱北京天安门"
    "body" => "天安门上太阳升"
    "age" => 150
  ]
]

发现返回数据中多了 age 字段， 修改成功。
搜索数据

先来个简单的.

$params = [
    'index' => 'my_index', 
    'type' => 'my_type',    
    'body' => [
        'query'=>[
            'match'=>[
                "title"    =>  '北京',
            ],
        ],
    ]
];
return  $this->client->search($params);

返回如下

array:4 [▼
  "took" => 188
  "timed_out" => false
  "_shards" => array:4 [▼
    "total" => 5
    "successful" => 5
    "skipped" => 0
    "failed" => 0
  ]
  "hits" => array:3 [▼
    "total" => 2
    "max_score" => 1.6451461
    "hits" => array:2 [▼
      0 => array:5 [▼
        "_index" => "my_index"
        "_type" => "my_type"
        "_id" => "10001"
        "_score" => 1.6451461
        "_source" => array:3 [▼
          "id" => "10001"
          "title" => "北京"
          "body" => "我们是首都"
        ]
      ]
      1 => array:5 [▼
        "_index" => "my_index"
        "_type" => "my_type"
        "_id" => "WKXd12IBwuLBOSSKe5k_"
        "_score" => 0.94175816
        "_source" => array:3 [▼
          "title" => "我爱北京天安门"
          "body" => "天安门上太阳升"
          "age" => 150
        ]
      ]
    ]
  ]
]

可以看到，title 中包含北京 的数据已经全部返回了。

Elasticsearch 最重要的也是最灵活的就是搜索了，你能想到的方法基本上Elasticsearch 都已经帮你做好，比如：
term,match,multi_match，range.prefix,wildcard,regexp.fuzzy,match_phrase,match_phrase_prefix,exists等等，了解具体方法的使用请参考：

    Elasticsearch: 权威指南 » 深入搜索
    Elasticsearch Reference » Query DSL

原文地址：http://www.qiehe.net/posts/4/the-use-and-configuration-of-elastcisearch-for-full-text-search

Good Good Study , Day Day Up!!


