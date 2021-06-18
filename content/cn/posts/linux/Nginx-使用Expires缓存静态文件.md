---
title: Nginx-使用Expires缓存静态文件
date: 2017-04-13T17:00:00+08:00
# # description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Nginx
- Linux
categories:
- Linux
- QuickStart
series:
- Linux Guide
image: images/common/nginx-1.jpeg
---

Nginx-使用Expires缓存静态文件

The ngx_http_headers_module module allows adding the “Expires” and “Cache-Control” header fields, and arbitrary fields, to a response header.

```sh
Syntax: expires [modified] time;
        expires epoch | max | off;
Default: expires off;
Context: http, server, location, if in location
```

缓存静态文件

可以用以下的时间单位：
ms: 毫秒
s: 秒
m: 分钟
h: 小时
d: 天
w: 星期
M: 月 (30 天)
y: 年 (365 天)

Example Configuration
 
```sh
expires    24h;
expires    modified +24h;
expires    @24h;
expires    0;
expires    -1;
expires    epoch;
expires    $expires;
add_header Cache-Control private;
```

The max parameter sets “Expires” to the value “Thu, 31 Dec 2037 23:55:55 GMT”, and “Cache-Control” to 10 years.

 
```sh
location ~* \.(?:manifest|appcache|html?|xml|json)$ {
  expires -1;
}
# Feed
location ~* \.(?:rss|atom)$ {
  expires 1h;
}
# Media: images, icons, video, audio, swf, flv
location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp3|mp4|mmf|ogg|ogv|webm|wma|wmv|asf|swf|flv)$ {
   expires 1M;
   access_log off;
   add_header Cache-Control "public";
}
# CSS and Javascript
location ~* \.(?:css|js)$ {
   expires 1y;
   access_log off;
}
# WebFonts
location ~* \.(?:ttf|ttc|otf|eot|woff|woff2)$ {
  expires 1M;
  access_log off;
}
```

修改实例

使用360奇云测给网站打分，看到如下提示:

```
指标描述: CSS、JS、图片资源都应该明确的指定一个缓存时间，避免了接下来的页面访问中不必要的HTTP请求。
评分规则: 如果有静态文件没有设置缓存，将会得到警告。
``` 

查看请求 favicon.ico 文件的响应头：

使用 curl -I 命令只查看请求的响应头:
 
```sh
$ curl -I http://wang123.net/favicon.ico
HTTP/1.1 200 OK
Server: nginx/1.10.2
Date: Thu, 13 Apr 2017 02:31:38 GMT
Content-Type: image/x-icon
Content-Length: 4286
Last-Modified: Mon, 20 Mar 2017 15:01:10 GMT
Connection: keep-alive
ETag: "58cfeeb6-10be"
Accept-Ranges: bytes
```

修改Nginx配置文件，然后重启服务:

在Nginx配置文件中增加以下配置，设置静态文件的缓存时间:

```sh
// 编辑服务器配置文件
$ vi /usr/local/webserver/nginx/conf/wang123_net.conf
# CSS, Javascript, images, icons, video, audio, 
location ~* \.(?:css|js|jpg|jpeg|gif|png|ico|svg|svgz|mp3|mp4|ogg|ogv|webm|wma|wmv|swf|flv)$ {
   expires 1M;
   access_log off;
   add_header Cache-Control "public";
}
```

重启服务: 

```sh
$ sudo /usr/local/webserver/nginx/nginx -t
nginx: the configuration file /usr/local/webserver/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/webserver/nginx/nginx.conf test is successful
$ sudo /usr/local/webserver/nginx/nginx -s reload
```

重新查看请求favicon.ico文件的响应头:

```php
$ curl -I http://wang123.net/favicon.ico
HTTP/1.1 200 OK
Server: nginx/1.10.2
Date: Thu, 13 Apr 2017 02:29:58 GMT
Content-Type: image/x-icon
Content-Length: 4286
Last-Modified: Mon, 20 Mar 2017 15:01:10 GMT
Connection: keep-alive
ETag: "58cfeeb6-10be"
Expires: Sat, 13 May 2017 02:29:58 GMT
Cache-Control: max-age=2592000
Cache-Control: public
Accept-Ranges: bytes
```

可以看到，返回结果中已经存在了 Expires、Cache-Control，说明我们配置成功。
重新使用360奇云测给网站打分， 可以看到就不会再有这个警告了。

### Reference

http://nginx.org/en/docs/http/ngx_http_headers_module.html  
https://github.com/h5bp/server-configs-nginx  
http://www.cnblogs.com/lpfuture/p/5798811.html  

[END]

