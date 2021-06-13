---
title: Golang中string和int类型相互转换
date: 2021-03-19T17:00:00+08:00
description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Golang
categories:
- themes
- syntax
series:
- Themes Guide
image: images/common/golang-1.jpeg
---

Golang中string和int类型相互转换


### string 转成 int

string 转成 int：

```c
int, err := strconv.Atoi(string)
```

string 转成 int64：

```c
int64, err := strconv.ParseInt(string, 10, 64)
```

string 转成 uint64：

```c
uint64, err := strconv.ParseUint(string, 10, 64)
```

### int 转成 string

int 转成 string：

```c
string := strconv.Itoa(int)
```

int64 转成 string：

```c
string := strconv.FormatInt(int64,10)
```

uint64 转成 string：

```c
string := strconv.FormatUint(uint64,10)
```


数字类型

```c
1 uint8 : 无符号 8 位整型 (0 到 255)
2 uint16 : 无符号 16 位整型 (0 到 65535)
3 uint32 : 无符号 32 位整型 (0 到 4294967295)
4 uint64 : 无符号 64 位整型 (0 到 18446744073709551615)
5 int8 : 有符号 8 位整型 (-128 到 127)
6 int16 : 有符号 16 位整型 (-32768 到 32767)
7 int32 : 有符号 32 位整型 (-2147483648 到 2147483647)
8 int64 : 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807)
```

浮点型

```c
1 float32 IEEE-754 32位浮点型数
2 float64 IEEE-754 64位浮点型数
3 complex64 32 位实数和虚数
4 complex128 64 位实数和虚数
```

其他数字类型


```c
1 byte 类似 uint8
2 rune 类似 int32
3 uint 32 或 64 位
4 int 与 uint 一样大小
5 uintptr 无符号整型，用于存放一个指针
```


### References

