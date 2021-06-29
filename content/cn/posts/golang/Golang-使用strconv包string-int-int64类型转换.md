---
title: Golang-使用strconv包string/int/int64类型转换
date: 2019-04-30T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Golang
categories:
- Golang
- QuickStart
series:
- Golang Guide
image: images/common/golang-1.jpeg
---

Go不会对数据进行隐式的类型转换，只能手动去执行转换操作。

strconv 包提供了简单数据类型之间的类型转换功能。

以下是常用的一些转换函数： 

## 将 int 类型转成 string 类型: (Itoa)

```js
num := 3311
str := strconv.Itoa(num)

fmt.Printf("--> 类型: %T, 值: %v \n", num, num) 
// 输出结果: “--> 类型: string, 值: 3311”

fmt.Printf("--> 类型: %T, 值: %v \n", str, str) 
// 输出结果: “--> 类型: string, 值: 3311”
```

## 将 int64 类型转换成 string 类型: (FormatInt)

`func FormatInt(i int64, base int) string`

```js
v := int64(-4235)

s10 := strconv.FormatInt(v, 10)
fmt.Printf("--> %T, %v\n", s10, s10)
// 输出结果: string, -4235
```

## 将 string 类型转成 int 类型 (Atoi)

Atoi: 将 string 类型转成 int 类型

```js
// Atoi: 将 string 类型转成 int 类型
fmt.Println("---- Atoi -----")
num2 := "1012"
if s, err := strconv.Atoi(num2); err == nil {
    fmt.Printf("--> 类型: %T, 值: %v \n", s, s) 
    // 输出结果: “--> 类型: int, 值: 1012”
}
```

## 将 string 类型转换到 int64 类型: 

将 string 类型转换到 int64 类型

```js
v32 := "-354634382"
if s, err := strconv.ParseInt(v32, 10, 32); err == nil {
    fmt.Printf("%T, %v\n", s, s) 
    // 输出: int64, -354634382
}

v64 := "-3546343826724305832"
if s, err := strconv.ParseInt(v64, 10, 64); err == nil {
    fmt.Printf("%T, %v\n", s, s) 
    // 输出: int64, -3546343826724305832
}
```

### 代码实例

```java
package main

import (
    "fmt"
    "strconv"
)

func main() {
    fmt.Println("--> Hello, World!");

    tips := "这里是my类型转换?"
    fmt.Println("--> tips: " + tips);

    // int 类型转成 string 类型
    fmt.Println("---- Itoa -----")
    num := 3311
    str := strconv.Itoa(num)
    fmt.Printf("--> 类型: %T, 值: %v \n", num, num) 
    // 输出结果: “--> 类型: string, 值: 3311”
    fmt.Printf("--> 类型: %T, 值: %v \n", str, str) 
    // 输出结果: “--> 类型: string, 值: 3311”

    // Atoi: 将 string 类型转成 int 类型
    fmt.Println("---- Atoi -----")
    num2 := "1012"
    if s, err := strconv.Atoi(num2); err == nil {
        fmt.Printf("--> 类型: %T, 值: %v \n", s, s) 
        // 输出结果: “--> 类型: int, 值: 1012”
    }

    // ParseFloat: 将字符串转换成浮点数
    fmt.Println("---- ParseFloat -----")
    v3 := "3.1415926535"
    if s, err := strconv.ParseFloat(v3, 32); err == nil {
        // 输出结果: “--> 类型: float64, 值: 3.1415927410125732”
        fmt.Printf("--> 类型: %T, 值: %v \n", s, s) 
    }
    if s, err := strconv.ParseFloat(v3, 64); err == nil {
        // 输出结果: “--> 类型: float64, 值: 3.1415926535”
        fmt.Printf("--> 类型: %T, 值: %v \n", s, s)
    } 

    // 特殊字符转义
    fmt.Println("---- Quote -----")
    // there is a tab character inside the string literal
    s := strconv.Quote(`"Fran & \n \t Freddie's Diner   ☺"`) 
    // 输出结果: “"\"Fran & \\n \\t Freddie's Diner\t☺\""”
    fmt.Println(s)
    // QuoteToASCII 将字符串转换为“双引号”引起来的 ASCII 字符串
    fmt.Println(strconv.QuoteToASCII("to quote Shakespeare 引用莎士比亚的话"))

    // 特殊字符取消转义
    fmt.Println("---- Unquote -----")
    s1 := "`Hello   世界！`"             // 解析反引号字符串
    s2 := `"Hello\t\u4e16\u754c\uff01"` // 解析双引号字符串
    s3 := `"to quote Shakespeare \u5f15\u7528\u838e\u58eb\u6bd4\u4e9a\u7684\u8bdd"` 
    // 解析双引号字符串
    fmt.Println(strconv.Unquote(s1))    // Hello    世界！ <nil>
    fmt.Println(strconv.Unquote(s2))    // Hello    世界！ <nil>
    fmt.Println(strconv.Unquote(s3))    // to quote Shakespeare 引用莎士比亚的话 <nil>
}
```

运行代码

```sh
$ go run ~/development/golang/go-string-strconv.go
```

代码输出结果如下:

```sh
--> Hello, World!
--> tips: 这里是my类型转换?
---- Itoa -----
--> 类型: int, 值: 3311 
--> 类型: string, 值: 3311 
---- Atoi -----
--> 类型: int, 值: 1012 
---- ParseFloat -----
--> 类型: float64, 值: 3.1415927410125732 
--> 类型: float64, 值: 3.1415926535 
---- Quote -----
"\"Fran & \\n \\t Freddie's Diner\t☺\""
"to quote Shakespeare \u5f15\u7528\u838e\u58eb\u6bd4\u4e9a\u7684\u8bdd"
---- Unquote -----
Hello	世界！ <nil>
Hello	世界！ <nil>
to quote Shakespeare 引用莎士比亚的话 <nil>
```

### Reference

https://golang.google.cn/pkg/strconv/   
https://www.cnblogs.com/golove/p/3262925.html  
https://www.cnblogs.com/f-ck-need-u/p/9863915.html  

[END]