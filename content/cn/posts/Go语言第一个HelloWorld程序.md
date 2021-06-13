---
title: Go语言第一个HelloWorld程序
date: 2019-08-19T17:00:00+08:00
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
- QuickStart
series:
- Themes Guide
image: images/common/golang-1.jpeg
---

Go语言第一个HelloWorld程序

Go，又称 Golang，是一个Google于2009年11月开源的编译型编程语言。

目前， Go语言最新版本为 go1.16.2 (released 2021/03/11) 。​

### 安装 Go

最简单的方式是，直接去官网(golang.org)下载安装包。

安装包下载地址为：https://golang.org/dl/。

如果打不开可以使用国内的这个地址：https://golang.google.cn/dl/。

Windows: xxx.windows-amd64.msi  
macOS: xxx.darwin-amd64.pkg  
Linux: xxx.linux-amd64.tar.gz  
Source源代码: xxx.src.tar.gz  

​根据不同的操作系统选择不同的​安装包进行安装即可。


查看当前系统的版本（macOS）： 

```sh
$ sw_vers                  
ProductName:	Mac OS X
ProductVersion:	10.15.7
BuildVersion:	19H2
```

查看go命令路径，看看go语言程序放在哪里了？ 

```
$ which go
/usr/local/go/bin/go
```

Go 被安装在了 `/usr/local/go` 路径下:

```sh
$ ll /usr/local/go
total 384
-rw-r--r--   1 root wheel  54K  3 12 01:15 AUTHORS
-rw-r--r--   1 root wheel 1.3K  3 12 01:15 CONTRIBUTING.md
-rw-r--r--   1 root wheel  99K  3 12 01:15 CONTRIBUTORS
-rw-r--r--   1 root wheel 1.4K  3 12 01:15 LICENSE
-rw-r--r--   1 root wheel 1.3K  3 12 01:15 PATENTS
-rw-r--r--   1 root wheel 1.4K  3 12 01:15 README.md
-rw-r--r--   1 root wheel 397B  3 12 01:15 SECURITY.md
-rw-r--r--   1 root wheel   8B  3 12 01:16 VERSION
drwxr-xr-x  22 root wheel 704B  3 12 01:15 api
drwxr-xr-x   4 root wheel 128B  3 12 02:02 bin
drwxr-xr-x   6 root wheel 192B  3 12 01:15 doc
-rw-r--r--   1 root wheel 5.6K  3 12 01:15 favicon.ico
drwxr-xr-x   3 root wheel  96B  3 12 01:15 lib
drwxr-xr-x  14 root wheel 448B  3 12 01:15 misc
drwxr-xr-x   6 root wheel 192B  3 12 01:18 pkg
-rw-r--r--   1 root wheel  26B  3 12 01:15 robots.txt
drwxr-xr-x  69 root wheel 2.2K  3 12 01:16 src
drwxr-xr-x 333 root wheel  10K  3 12 01:16 test
```

在 /usr/local/go/bin/ 目录下有2个可执行文件，一个为 go 一个为 gofmt (用了格式化代码)。

```bash
$ tree /usr/local/go/bin/
/usr/local/go/bin/
├── go
└── gofmt

0 directories, 2 files
```

使用 go version 命令，查看 go 语言的版本:

```sh
$ go version
go version go1.16.2 darwin/amd64

$ /usr/local/go/bin/go version
go version go1.16.2 darwin/amd64
```

目前安装的是 go1.16.2 版本。


### HelloWorld 程序

(1) 创建一个文件名为: `hello.go`，代码如下：

```c
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!");
}
```

(2) 使用 `go run` 命令运行 `hello.go` 程序: 

```sh
$ go run hello.go 
Hello, World!
```

go run 编译并直接运行程序。

(3) 使用 `go build` 构建代码，并执行编译后的可执行文件: 

```sh
$ go build hello.go
$ ./hello 
Hello, World!
```

go build 用于构建代码，主要检查是否会有编译错误，如果是一个可执行文件的源码（即是 main 包），就会直接生成一个可执行文件。

### 程序分析

文件 hello.go : 

```c
// 定义了包名 (可以看到包名和文件名)
package main

// 引入 fmt 包，fmt 包实现了格式化 IO（输入/输出）的函数
import "fmt"

// main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数
// 如果有 init() 函数则会先执行该函数。
func main() {
   // 将字符串输出到控制台，并在最后自动增加换行字符"\n"
   fmt.Println("Hello, World!")
}
```

### 基础语法简介

(1) 在 Go 程序中，一行代码就代表一个语句结束，不用加分号(;)，加上分号也能正常运行。

(2) 变量的声明:

Go 语言中变量的声明使用 var 关键字.

var age int 
var name string  
var title string = "hi wang123.net" 
var isProd bool = true 

简写：省略 var, 直接使用 := 进行赋值。

title := "WANG123NET" // 等于 var title string = "WANG123NET"

(3) 字符串拼接：可以使用加号​(+)

fmt.Println("hello " + " world")

(4) 常量 const

const WIDTH int = 3   
const LENGTH int = 4
const HEIGHT int = 5

或者写到一起： 

const (
    WIDTH = 3
    LENGTH = 4
    HEIGHT = 5
)

(5) 函数 func : Go 语言最少有个 main() 函数。

Go 语言函数定义格式如下：

func functionName( [parameter list] ) [returnTypes] {
   函数体
}


### 参考链接

https://golang.google.cn/  
https://www.runoob.com/go/go-tutorial.html  

### 更新记录

2018-12-08 发表与CSDN博客。  
2021-03-19 完善内容，更新到 go1.6.2 版本。  

[END]