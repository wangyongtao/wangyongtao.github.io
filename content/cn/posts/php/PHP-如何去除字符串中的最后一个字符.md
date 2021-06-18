---
title: PHP-如何去除字符串中的最后一个字符
date: 2012-06-01T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- PHP
categories:
- PHP
- QuickStart
series:
- PHP Guide
image: images/common/php-1.jpeg
---


PHP如何去除字符串中的最后一个字符呢？

可以使用语言提供的函数 trim 或 substr等。

比如：字符串 “aaaa,bbb,ccc,ddd,eee,”， 删除最后的逗号“，”：

```php
<?php

//PHP去除字符串中的最后一个字符
$str = "aaaa,bbb,ccc,ddd,eee,";

// 第一种方法 trim($str,$chsrlist)去除两边的
echo rtrim($str,','); //第一种方法 trim($str,$chsrlist)去除两边的
echo PHP_EOL;

// 第二种方法
echo substr($str,0,strlen($str)-1); 
echo PHP_EOL;

// 第三种方法
echo substr($str,0,-1); 
echo PHP_EOL;

// 第四种方法
echo $str[strlen($str)-1] == ',' ? substr($str, 0, -1) : $str; 
echo PHP_EOL;
```

使用 `rtrim()` 函数还是比较好的，推荐使用！

可以指定需要去除的字符，比如去除右边的逗号：

```php
$new_str = rtrim( trim($str), ',' );
```

也可以指定多个需要去除的字符，去除多个字符：

```php
$new_str = rtrim( trim($str),',.;' );
```

### 拓展阅读：

代码中用到的函数：

trim — 去除字符串首尾处的空白字符（或者其他字符）

ltrim — 删除字符串开头的空白字符（或其他字符） (left trim)

rtrim — 删除字符串末端的空白字符（或者其他字符） (right trim)

substr — 返回字符串的子串 (substring)

strlen — 获取字符串长度 (string length)

[END]