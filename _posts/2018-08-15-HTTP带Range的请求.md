---
layout:     post
title:      HTTP带Range的请求
subtitle:   HTTP头部
date:       2018-08-15
author:     Galvin
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - HTTP
---

在响应中存在Accept-Ranges头部，表示该服务器支持Range请求 <br>
以请求七牛存储资源为例
```
curl -H "Range: bytes=0-" http://huadong.yyl.qiniuts.com/huadong.mp4 -I

HTTP/1.1 206 Partial Content
...
Connection: keep-alive
Date: Wed, 15 Aug 2018 03:17:41 GMT
Accept-Ranges: bytes
...
```

curl模拟范围请求

```
curl -H "Range: bytes=0-" http://huadong.yyl.qiniuts.com/huadong.mp4 -Iv

*   Trying 202.127.76.209...
* TCP_NODELAY set
* Connected to huadong.yyl.qiniuts.com (202.127.76.209) port 80 (#0)
> HEAD /huadong.mp4 HTTP/1.1
> Host: huadong.yyl.qiniuts.com
> User-Agent: curl/7.54.0
> Accept: */*
> Range: bytes=0-
> 
< HTTP/1.1 206 Partial Content
HTTP/1.1 206 Partial Content
< Server: Tengine
...
```
#####  条件范围请求
当（中断之后）重新开始请求更多资源片段的时候，必须确保自从上一个片段被接收之后该资源没有进行过修改。

The If-Range 请求首部可以用来生成条件式范围请求：假如条件满足的话，条件请求就会生效，服务器会返回状态码为 206 Partial 的响应，以及相应的消息主体。假如条件未能得到满足，那么就会返回状态码为 200 OK 的响应，同时返回整个资源。该首部可以与  Last-Modified 验证器或者  ETag 一起使用，但是二者不能同时使用。

```
If-Range: Wed, 21 Oct 2015 07:28:00 GMT
```

##### 范围请求响应
与范围请求相关的有三种状态：

- 在请求成功的情况下，服务器会返回  206 Partial Content 状态码。
- 在请求的范围越界的情况下（范围值超过了资源的大小），服务器会返回 416 Requested Range Not Satisfiable （请求的范围无法满足） 状态码。
- 在不支持范围请求的情况下，服务器会返回 200 OK 状态码。

##### Range头域可以请求实体的一个或者多个子范围
表示头500个字节：bytes=0-499  
表示第二个500字节：bytes=500-999  
表示最后500个字节：bytes=-500  
表示500字节以后的范围：bytes=500-  
第一个和最后一个字节：bytes=0-0,-1  
同时指定几个范围：bytes=500-600,601-999
