---
title: PHP-如何不使用临时变量来交换两个数值变量
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

PHP-如何不使用临时变量来交换两个数值变量？

如何在PHP中不使用临时变量来交换两个数值变量？

### 解析如下：

一般情况下，交换两个变量的值应该使用中间变量：

```php
function swap($a, $b){
   $temp = $a;
   $a = $b;
   $b = $temp;
}
```

1.这个方法很容易想到，但是只限于交换数值类型的变量：

```php
function swap (&$a,&$b){
    $a = $a+$b;
    $b = $a-$b;
    $a = $a-$b;
}
```

2.使用 list 语言结构。

```php
list($a, $b) = array($b, $a);

// 自 PHP 5.4 起
list($a, $b) = [$b, $a];
```

注：list — 把数组中的值赋给一些变量。像 array() 一样，这不是真正的函数，而是语言结构。 list() 可以在单次操作内就为一组变量赋值。

3.通过数组函数 array_reverse

```php
$arr = array($a,$b);
$arr = array_reverse($arr);
$a = $arr[0];
$b = $arr[1];
```

注：array_reverse — 返回一个单元顺序相反的数组

4.直接使用数组操作：

```php
$a = "aaa";
$b = "bbb";
$b = array($a, $b);
$a = $b[1];
$b = $b[0];
```

### 更新记录

2012-06-01 新增此文档 (wangyt)  
2017-04-12 重新整理本文档 (wangyt) 

[END]

