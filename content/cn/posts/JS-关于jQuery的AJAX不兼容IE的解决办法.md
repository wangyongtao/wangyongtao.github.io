---
title: JS-关于jQuery的AJAX不兼容IE的解决办法
date: 2012-11-02T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- JavaScritp
categories:
- JavaScritp
- QuickStart
series:
- JavaScritp
image: images/common/www-1.jpeg
---

关于jQuery的AJAX不兼容IE的解决办法

在使用jQuery的AJAX：get方法去检测数据是否存在时，会发现IE会出现不兼容的情况。

用AJAX:post方法时，使用Chrome/FireFox/IE均能出现正确的结果，但是在使用AJAX:get方法时，IE却不能返回正确的结果。

### 解决过程

难道是数据超出了get方法的限制的长度，这个也不可能，我总共才传了一点点数据。排除。

网上一些网友说是IE缓存的问题，在请求数据后边加上随机数就行，比如加上时间数new Date().getTime()。
先前的代码中我已经添加了随机数，用的是“Math.random()”也不行。难道用时间比较靠谱？

那就改成获取时间试试，在参数后加“new Date().getTime()”后反复测试还是不行，真是百思不得其解！这个错误也排除了。

反复查看手册后发现请求的数据格式还是有一种JSON格式，如{foo:[“bar1”, “bar2”]} ，然后就按照这种格式书写，还真的返回了正确的查询结果。真不知道IE还有这点要求。（完）

先前的格式：

```js
type: "get",
data: "bid="+my_bid+"&name_cn="+name_cn+"&timeStamp="+new Date().getTime(),
```

修改后格式：

```js
type: "get",
data: {'bid':my_bid,'name_cn':name_cn,'timeStamp':new Date().getTime()},
```

在jQuery手册中是这样描述的：

data Object,String

发送到服务器的数据。将自动转换为请求字符串格式。GET 请求中将附加在 URL 后。
查看 processData 选项说明以禁止此自动转换。必须为 Key/Value 格式。
如果为数组，jQuery 将自动为不同值对应同一个名称。如 {foo:["bar1", "bar2"]} 转换为 "&foo=bar1&foo=bar2"。

代码片段如下:

```js
var siteUrl="http://blog.sina.com.cn/cnwyt"; 
jQuery.ajax({
	type: "get",
	url: siteUrl+"cosmetics/product/ajax_check?",
	//data: "bid="+my_bid+"&name_cn="+name_cn+"&timeStamp=" + new Date().getTime(),
	data: {'bid':my_bid,'name_cn':name_cn,'timeStamp':new Date().getTime()},
	dataType: 'json',
	error: function (err) { 
		alert('网络故障,请与管理员联系！');
	},
	success: function (message) {
		if (message!=false){
			//ture的代码
		} else {
			//false的代码
		}
	}
});
```

问题描述：`jQuery.get()` 方法在 FireFox 下可以正常获取值并显示，但是在 IE 下不行。

### 原因1：因为IE的缓存问题

解决：在URL后面加一个随机数或时间参数

1
url = "chek.php?timeStamp="+new Date().getTime();
或者写成：

```javascript
data:{
	'type': type,
	'name': name,
	'timeStamp': new Date().getTime()
},
```

获取随机数 Math.random()

获取时间数 new Date().getTime()

### 原因2：数据格式问题

先前的格式：

```javascript
type:"get",
data:"bid="+my_bid+"&name_cn="+name_cn+"&timeStamp="+new
Date().getTime(),
dataType:'json',
```

修改后的格式：

```javascript
type:"get",
data:{'bid':my_bid,'name_cn':name_cn,'timeStamp':new
Date().getTime()},
dataType:'json',
```

### 参考链接：

jQuery 的 .get和.post和.ajax方法IE的兼容问题
http://blog.csdn.net/muziduoxi/article/details/7541800

jquery ajax在IE下失效
http://www.im87.cn/blog/256

### 更新记录

2012-11-02 新增此文档 (wangyt)
2017-04-12 重新整理本文档 (wangyt)

[END]

