---
title: PHP-PHP常量详解define和const的区别?
date: 2013-07-29T17:00:00+08:00
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
image: images/common/php-2.jpeg
---


PHP常量详解：define和const的区别

常量是一个简单值的标识符（名字）。
如同其名称所暗示的，在脚本执行期间该值不能改变（除了所谓的魔术常量，它们其实不是常量）。
常量默认为大小写敏感。通常常量标识符总是大写的。

可以用 define() 函数来定义常量。

在 PHP 5.3.0 以后，可以使用 const 关键字在类定义的外部定义常量，先前版本const 关键字只能在类（class）中使用。
一个常量一旦被定义，就不能再改变或者取消定义。

常量只能包含标量数据（boolean，integer，float 和 string）。 
可以定义 resource 常量，但应尽量避免，因为会造成不可预料的结果。
可以简单的通过指定其名字来取得常量的值，与变量不同，不应该在常量前面加上 $ 符号。
如果常量名是动态的，也可以用函数constant() 来获取常量的值。
用get_defined_constants() 可以获得所有已定义的常量列表。

常量和变量有如下不同：

·常量前面没有美元符号（$）；
·常量只能用 define() 函数定义，而不能通过赋值语句；
·常量可以不用理会变量的作用域而在任何地方定义和访问；
·常量一旦定义就不能被重新定义或者取消定义；
·常量的值只能是标量。

Example #1 定义常量

```php
<？php
define("CONSTANT", "Hello world.");
echo CONSTANT; // outputs "Hello world."
echo Constant; // 输出 "Constant" 并发出一个提示性信息
?>
```

Example #2 使用关键字 const 定义常量

```php
<？php
// 以下代码在 PHP 5.3.0 后可以正常工作
const CONSTANT = 'Hello World';
echo CONSTANT;
?>
```

Example #3 合法与非法的常量名

```php
<？php
// 合法的常量名
define("FOO",     "something");
define("FOO2",    "something else");
define("FOO_BAR", "something more");
// 非法的常量名
define("2FOO",    "something");
// 下面的定义是合法的，但应该避免这样做：(自定义常量不要以__开头)
// 也许将来有一天PHP会定义一个__FOO__的魔术常量
// 这样就会与你的代码相冲突
define("__FOO__", "something");
?>
```

【问】在php中定义常量时,const与define的区别?
【答】使用const使得代码简单易读，const本身就是一个语言结构，而define是一个函数。另外const在编译时要比define快很多。

(1).const用于类成员变量的定义，一经定义，不可修改。define不可用于类成员变量的定义，可用于全局常量。
(2).const可在类中使用，define不能。
(3).const不能在条件语句中定义常量。
例如：

```php
if (...){
    const FOO = 'BAR';    // 无效的invalid
} 
if (...) {
    define('FOO', 'BAR'); // 有效的valid
}
```

(4).const采用一个普通的常量名称，define可以采用表达式作为名称。

```php
const  FOO = 'BAR'; 
for ($i = 0; $i < 32; ++$i) {
    define('BIT_' . $i, 1 << $i);
}
```

(5).const只能接受静态的标量，而define可以采用任何表达式。

例如：

```php
const BIT_5 = 1 << 5;    // 无效的invalid 
define('BIT_5', 1 << 5); // 有效的valid
```

(6).const定义的常量时大小写敏感的，而define可通过第三个参数（为true表示大小写不敏感）来指定大小写是否敏感。

例如：

```php
define('FOO', 'BAR', true); 
echo FOO; // BAR
echo foo; // BAR
```

### 相关函数：

define — 定义一个常量

说明：
bool define ( string $name , mixed $value [, bool $case_insensitive = false ]

参数：
name ：常量名。
value ：常量的值；仅允许标量和 null。标量的类型是 integer， float，string 或者 boolean。 也能够定义常量值的类型为 resource ，但并不推荐这么做，可能会导致未
知状况的发生。
case_insensitive ：如果设置为 TRUE，该常量则大小写不敏感。默认是大小写敏感的。比如， CONSTANT 和 Constant 代表了不同的值。（Note: 大小写不敏感的常量以小写
的方式储存。）

返回值：成功时返回 TRUE， 或者在失败时返回 FALSE.

constant — 返回一个常量的值

说明：
mixed constant ( string $name )
通过 name 返回常量的值。当你不知道常量名，却需要获取常量的值时，constant() 就很有用了。也就是常量名储存在一个变量里，或者由函数返回常量名。该函数也适用
class constants。

参数：
name ：常量名。

返回值：
返回常量的值。如果常量未定义则返回 NULL。

defined — 检查某个名称的常量是否存在

说明：
bool defined ( string $name )
检查该名称的常量是否已定义。
Note: 如果你要检查一个变量是否存在，请使用 isset()。 defined() 函数仅对 constants 有效。如果你要检测一个函数是否存在，使用 function_exists()。

参数：
name ：常量的名称。

返回值：
如果该名称的常量已定义，返回 TRUE；未定义则返回 FALSE。
get_defined_constants:

Returns an associative array with the names of all the constants and their values
以关联数组返回常量名和常量的值。这包括那些由扩展以及由define()函数创建的常量。

### 参考链接：

php中定义常量define和const区别 http://www.dewen.org/q/4280

### 更新记录：

2013-07-29 新增此文档 (wangyt)
2017-04-12 重新整理本文档 (wangyt)

[END]