---
title: HTTP 表单提交中 Request Payload 和 FormData 有什么区别
date: 2021-03-20T12:00:06+09:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Web
- Request Payload
- FormData
categories:
- themes
- syntax
series:
- Themes Guide
image: images/feature1/markdown.png
---

HTTP 表单提交中 Request Payload 和 FormData 有什么区别？

FormData 和 Request Payload 是浏览器传给服务端接口的两种格式，这两种方式浏览器是通过 `Content-Type` 来进行区分的。

如果是 enctype="application/x-www-form-urlencoded" 或 enctype="multipart/form-data" 的话，则为 `FormData` 方式。

如果是 application/json 或 enctype="text/plain" 的话，则为 Request Payload 的方式。

### Html代码查看 FormData 

使用 HTML 直接提交数据。HTML代码如下： 

```html
<form method="POST" enctype="application/x-www-form-urlencoded"> -->
<form method="POST" enctype="text/plain">
<!-- <form method="POST" enctype="multipart/form-data"> -->
  name: 
  <input type="text" name="name" value="formdata1"> 
  <br/>

  description: 
  <input type="text" name="description" value="测试测试测试...">
  <br/>

  tag: 
  <input type="checkbox" name="tags[]" checked="checked" value="tag1"> tag1 
  <input type="checkbox" name="tags[]" checked="checked" value="tag2"> tag2 
  <input type="checkbox" name="tags[]" checked="checked" value="tag3"> tag3 
  <br/>

  action: <br/>
  <input type="checkbox" name="action[blog][key]" checked="checked" value="blog"> blog 
  <input type="text" name="action[blog][value]" value="value111"> 
  <br/>
  <input type="checkbox" name="action[bbs][key]" checked="checked" value="bbs"> bbs 
  <input type="text" name="action[bbs][value]" value="value222">  
  <br/>
  
  <button type="submit"> submit </button>
</form>
```

通过 Chrome 控制台 Network 查看请求头：

```js
// FormData 数据
name: formdata1
description: 测试测试测试...
tags[]: tag1
tags[]: tag2
tags[]: tag3
action[blog][key]: blog
action[blog][value]: value111
action[bbs][key]: bbs
action[bbs][value]: value222
```

后端(PHP)接收到数据格式如下：

```php
// index.php 文件输出的内容：
Array (
    [URLSearchParams] => 
    [name] => formdata1
    [description] => 测试测试测试...
    [tags] => Array (
        [0] => tag1
        [1] => tag2
        [2] => tag3
    )

    [action] => Array (
        [blog] => Array(
            [key] => blog
            [value] => value111
        )

        [bbs] => Array(
            [key] => bbs
            [value] => value222
        )
    )
)
```


### 使用 JS 发送请求数据

在前端开发中，我们一般不直接使用 HTML 提交数据到后台，而需要 JS 处理后台（比如进行表单数据校验、数据转换等）后，直接使用 JS 提交数据。

在之前，我们可以使用 jQuery 进行提交数据。

```js
var form = $("#myForm").serialize();
$.ajax({
    type: 'POST',
    url: 'https://wang123.net/api/test',
    data: form,
    dataType: 'json', 
    success: function (res) {
    	// 请求成功后处理 
        console.log(res);
   }
});
```

现在 vuejs 开发中，一半使用 axios.js 请求库。

```js
// axios.js 的默认请求使用 payload 传输 json数据
const formElement = document.querySelector('#myForm');

const formData = new FormData(formElement);

axios.post('http://localhost:8003?Payload', {
	type: '我是用payload提交的json数据',
	name: formData.get('name'),
	description: formData.get('description'),
	tags: formData.getAll('tags[]'),
	action: {
		blog: {key: 111, value: 'asdf'},
		bbs: {key: 222, value: 'ghjk'},
	},
})
.then(function (response) {
	console.log(response);
})
.catch(function (error) {
	console.log(error);
});
```
 
### 服务端如何接受数据呢？


预定义变量 `$_REQUEST` 、`$_POST`、 `$_GET` 只能获取 `FormData` 格式数据，即请求。
 
(1)预定义的 `$_POST` 变量用于收集来自 `method="post"` 的表单中的值。  
(2)预定义的 `$_GET` 变量用于收集来自 `method="get"` 的表单中的值。  
(3)预定义的 `$_REQUEST` 变量包含了 `$_GET`、`$_POST` 和 `$_COOKIE` 的内容。   

想要获取 `application/json` 的原始数据，需要直接从 `php://input` 获取请求`body`的数据，自己反序列化成数组或对象。

php://input 是个可以访问请求的原始数据的只读流。
php://input 不能用于 `enctype="multipart/form-data"`的表单数据。

第一种： 原生PHP处理 Request Payload 数据示例： 

```php
<?php
// 文件路径：demo/index.php

// 服务端接受表单数据 
// $_REQUEST 、$_POST、 $_GET 只能获取 FormData 格式数据
print_r($_REQUEST);

// 需要直接从 php://input 获取请求body的原始数据，自己反序列化成数组或对象。
$requestBody = file_get_contents('php://input');
var_dump($requestBody);

$formData = json_decode($requestBody, true);
print_r($formData);
```

测试环境可以使用 `php -S localhost:8003 -t ./demo/` 启动服务。

第二种： 使用现代框架

如果使用 Web 框架，框架里一般都已经提供了 Request Payload 解析，可以直接使用。 

比如 PHP Laravel 框架，可以直接从 Request 对象中取到数据。

第三中方法: 修改 axios 等类库，转成 FormData 提交数据。

(1)可以引入 qs.js 类库来提交FormData数据（略）  
(2)可以使用URLSearchParams API， 提交 FormData 数据  

```js
// 引入 qs.js 类库
// 请求接口 使用 Qs.stringify 转换
axios.post('index.php?Act=comm&do=query', 
	Qs.stringify(queryData), 
	{ timeout: 5000 }
)
.then(function (response) {
	console.log(response);
})
.catch(function (error) {
	console.log(error);
});

// 使用URLSearchParams API
const params = new URLSearchParams();
for (let [key, value] of formData) {
	// console.log('formData: ', key, value);
	params.append(key, value);
}

axios.post('http://localhost:8003?URLSearchParams', params)
.then(function (response) {
	console.log(response);
})
.catch(function (error) {
	console.log(error);
});
```

完整代码如下：

```html
<!DOCTYPE html>
<html>
<head>
  <title>FormData测试</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=0">
  <script src="./axios.min.js"></script>
</head>
<body>
  <div id="app">
    <!-- <form method="POST" enctype="application/x-www-form-urlencoded"> -->
    <form method="POST" enctype="multipart/form-data">
    <!-- <form id="myForm" method="POST" onsubmit='return checkForm()'> -->
      name: 
      <input type="text" name="name" value="formdata1"> 
      <br/>

      description: 
      <input type="text" name="description" value="测试测试测试...">
      <br/>

      tag: 
      <input type="checkbox" name="tags[]" checked="checked" value="tag1"> tag1 
      <input type="checkbox" name="tags[]" checked="checked" value="tag2"> tag2 
      <input type="checkbox" name="tags[]" checked="checked" value="tag3"> tag3 
      <br/>

      action: <br/>
      <input type="checkbox" name="action[blog][key]" checked="checked" value="blog"> blog 
      <input type="text" name="action[blog][value]" value="value111"> 
      <br/>
      <input type="checkbox" name="action[bbs][key]" checked="checked" value="bbs"> bbs 
      <input type="text" name="action[bbs][value]" value="value222">  
      <br/>
      
      <button type="submit"> submit </button>
    </form>
  </div>

  <script>
    function checkForm() {
      console.log('---> 表单提交：');

      const formElement = document.querySelector('#myForm');

      const formData = new FormData(formElement);


      console.log("---> formElement", formElement)
      console.log("---> formData", formData)
      console.log("---> formData.getAll name", formData.getAll('name'))

      // axios 默认使用 payload 传递数据，将对象序列化为JSON
      axios.post('http://localhost:8003?Payload', {
         type: '我是用payload提交的json数据',
         name: formData.get('name'),
         description: formData.get('description'),
         tags: formData.getAll('tags[]'),
         action: {
            blog: {key: 111, value: 'asdf'},
            bbs: {key: 222, value: 'ghjk'},
         },
      })
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
 
      // axios 提交 FormData 数据
      axios.post('http://localhost:8003?formData', formData)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });

      
      // (1)可以引入 qs.js 类库来提交FormData数据（略）
      // (2)可以使用URLSearchParams API， 提交FormData数据

      const params = new URLSearchParams();
      for (let [key, value] of formData) {
        // console.log('formData: ', key, value);
        params.append(key, value);
      }

      axios.post('http://localhost:8003?URLSearchParams', params)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });

      return true;
    }
  </script>
</body>
</html>
```


### 参考资料

https://www.cnblogs.com/tugenhua0707/p/8975615.html  
https://gomakethings.com/how-to-serialize-form-data-with-vanilla-js/   
[END]
