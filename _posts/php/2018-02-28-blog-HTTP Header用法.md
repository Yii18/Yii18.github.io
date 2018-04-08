---
layout: blog
banana: true
category: php
title:  php header函数用法
date:   2017-09-30 20:06:42
background-image: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3703404779,2975491938&fm=27&gp=0.jpg
tags:
- Redis
- php
- 负载均衡
---
* content
{:toc}

关于Ｗeb 交互中，Header　的相关信息.






## HTTP Request的Header信息
### 1. HTTP请求方式

|方 法|描 述|
|---|---|
|GET|向Web服务器请求一个文件|
|POST|向Web服务器发送数据让Web服务器进行处理|
|PUT|向Web服务器发送数据并存储在Web服务器内部|
|HEAD|检查一个对象是否存在|
|DELETE|从Web服务器上删除一个文件|
|CONNECT|对通道提供支持|
|TRACE|跟踪到服务器的路径|
|OPTIONS|查询Web服务器的性能|

**说明：**

主要使用到"GET"和"POST".

**实例：**

POST /test/tupian/cm HTTP/1.1
分成三部分：
1. POST：HTTP请求方式
2. /test/tupian/cm：请求Web服务器的目录地址（或者指令）
3. HTTP/1.1: URI（Uniform Resource Identifier，统一资源标识符）及其版本

**备注：**

在Ajax中，对应method属性设置。


### 2. Host

**说明：**

请求的web服务器域名地址

**实例：**

例如:
web请求URL：http://www.baidu.com:8888/test/1
Host就为www.baidu.com:8888


### 3. User-Agent

**说明：**

HTTP客户端运行的浏览器类型的详细信息。通过该头部信息，web服务器可以判断到当前HTTP请求的客户端浏览器类别。

**实例：**

User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.8.1.11) Gecko/20071127 Firefox/2.0.0.11

### 4. Accept

**说明：**

指定客户端能够接收的内容类型，内容类型中的先后次序表示客户端接收的先后次序。

**实例：**

Accept:text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5

**备注：**

在Prototyp（1.5）的Ajax代码封装中，将Accept默认设置为“text/javascript, text/html, application/xml, text/xml, */*”。这是因为Ajax默认获取服务器返回的Json数据模式。

在Ajax代码中，可以使用XMLHttpRequest 对象中setRequestHeader函数方法来动态设置这些Header信息。

### 5. Accept-Language

**说明：**

指定HTTP客户端浏览器用来展示返回信息所优先选择的语言。

**实例：**

Accept-Language: zh-cn,zh;q=0.5 (这里默认为中文)


### 6. Accept-Encoding

**说明：**

指定客户端浏览器可以支持的web服务器返回内容压缩编码类型。表示允许服务器在将输出内容发送到客户端以前进行压缩，以节约带宽。而这里设置的就是客户端浏览器所能够支持的返回压缩格式。

**实例：**

Accept-Encoding: gzip,deflate

### 7. Accept-Charset

**说明：**

浏览器可以接受的字符编码集。

**实例：**

Accept-Charset: gb2312,utf-8;q=0.7,*;q=0.7


### 8. Content-Type

**说明：**

显示此HTTP请求提交的内容类型。一般只有post提交时才需要设置该属性。

**实例：**

Content-type: application/x-www-form-urlencoded;charset:UTF-8

**备注：**

有关Content-Type属性值可以如下两种编码类型：
1. "application/x-www-form-urlencoded"： 表单数据向服务器提交时所采用的编码类型，默认的缺省值就是“application/x-www-form-urlencoded”。 然而，在向服务器发送大量的文本、包含非ASCII字符的文本或二进制数据时这种编码方式效率很低。
2. "multipart/form-data"： 在文件上传时，所使用的编码类型应当是"multipart/form-data"，它既可以发送文本数据，也支持二进制数据上传。

当提交为单单数据时，可以使用“application/x-www-form-urlencoded”；当提交的是文件时，就需要使用“multipart/form-data”编码类型。
在Content-Type属性当中还是指定提交内容的charset字符编码。一般不进行设置，它只是告诉web服务器post提交的数据采用的何种字符编码。
一般在开发过程，是由前端工程与后端UI工程师商量好使用什么字符编码格式来post提交的，然后后端ui工程师按照固定的字符编码来解析提交的数据。所以这里设置的charset没有多大作用。


### ９. Connection

**说明：**

表示是否需要持久连接。如果web服务器端看到这里的值为“Keep-Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接），它就可以利用持久连接的优点，当页面包含多个元素时（例如Applet，图片），显著地减少下载所需要的时间。要实现这一点， web服务器需要在返回给客户端HTTP头信息中发送一个Content-Length（返回信息正文的长度）头，最简单的实现方法是：先把内容写入ByteArrayOutputStream，然 后在正式写出内容之前计算它的大小。

**实例：**

Connection: keep-alive


### 10. Keep-Alive

**说明：**

显示此HTTP连接的Keep-Alive时间。使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。
以前HTTP请求是一站式连接，从HTTP/1.1协议之后，就有了长连接，即在规定的Keep-Alive时间内，连接是不会断开的。

**实例：**

Keep-Alive: 300

### 11. cookie
**说明：**

HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器。


### 12. Referer
**说明：**

包含一个URL，用户从该URL代表的页面出发访问当前请求的页面。


## 服务器端返回HTTP头部信息
### 1. Content-Length
**说明：**

表示web服务器返回消息正文的长度


### 2. Content-Type
**说明：**

返回数据的类型（例如text/html文本类型）和字符编码格式。

**实例：**

Content-Type: text/html;charset=utf-8

### 3. Date
**说明：**

显示当前的时间



## 服务器常用Header设置
### header()定义
header() 函数向客户端发送原始的 HTTP 报头

### 语法
header(string,replace,http_response_code)

|参数|描述|
|---|---|
|string|必选,规定要发送的报头字符串|
|replace|可选,指示该报头是否替换之前的报头,或添加第二个报头.默认是 true（替换）;false（允许相同类型的多个报头）|
|http_response_code|可选,把 HTTP 响应代码强制为指定的值|


1. 页面没找到 Not Found
```php
header('HTTP/1.1 404 Not Found');
```

2. 用这个header指令来解决URL重写产生的404 header
```php
header('HTTP/1.1 200 OK');
```

3. 访问受限
```php
header('HTTP/1.1 403 Forbidden');
```
　　
4. 页面被永久删除，可以告诉搜索引擎更新它们的urls
```php
header('HTTP/1.1 301 Moved Permanently');
```

5. 服务器错误
```php
header('HTTP/1.1 500 Internal Server Error');
```

6. 重定向到一个新的位置
```php
header('Location: .example.org/');
```
　　
7. 延迟一段时间后重定向
```php
header('Refresh: 10; url=.example.org/');
```

8. 提示用户保存一个生成的 PDF 文件
```php
header("Content-type:application/pdf");
// 文件将被称为 downloaded.pdfheader("Content-Disposition:attachment;filename='downloaded.pdf'");
readfile("original.pdf");
// PDF 源在 original.pdf 中
```
　
9. 可以使用HTML语法来实现延迟
```php
header('Content-Transfer-Encoding: binary');
```

10. 禁止缓存当前文档
```php
header('Content-Transfer-Encoding: binary');
header（'Cache-Control: no-cache, no-store, max-age=0, must-revalidate'）;
header（'Expires: Mon, 26 Jul 2010 05:00:00 GMT'）;
header（'Pragma: no-cache'）;
```

11. 显示登录对话框,可以用来进行HTTP认证
```php
header（'HTTP/1.1 401 Unauthorized'）;
header（'WWW-Authenticate: Basic realm="Top Secret"'）;
```

12. 设置内容类型
```php
header（'Content-Type: text/html; charset=iso-8859-1'）;
header（'Content-Type: text/html; charset=utf-8'）;
header（'Content-Type: text/plain'）; // plain text file
header（'Content-Type: image/jpeg'）; // JPG picture
header（'Content-Type: application/zip'）; // ZIP file
header（'Content-Type: application/pdf'）; // PDF file
header（'Content-Type: audio/mpeg'）; // Audio MPEG （MP3,…） file
header（'Content-Type: application/x-shockwave-flash'）; // Flash animation
```
