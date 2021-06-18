---
title: Go语言第一个HelloWorld程序
date: 2019-08-19T17:00:00+08:00
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
- themes
- QuickStart
series:
- Themes Guide
image: images/common/golang-1.jpeg
---

Go语言第一个HelloWorld程序

### Go语言简介

Go，又称 Golang，是一个 Google 公司开源的静态的、编译型编程语言。

Go语言起源于 2007 年，并在 2009 年正式对外开源，在2012年发布了 Go 1 稳定版本。

Go语言早期（go1.4版本和以前）使用C语言和汇编编写，从 Go 1.5 起开始Go语言实现了自举（Bootstrapping），即使用Go语言自己编写的Go语言。

### 安装 Go 

最简单的方式是，直接去官网(golang.org)下载安装包。

安装包下载地址为：https://golang.org/dl/。

如果打不开可以使用国内的这个地址：https://golang.google.cn/dl/。

目前， Go语言最新版本为 `go1.16.5` (released 2021-06-03) 。

```
Windows: go1.16.5.windows-amd64.msi (119MB)
macOS:   go1.16.5.darwin-amd64.pkg (125MB)
Linux:   go1.16.5.linux-amd64.tar.gz (123MB)
Source:  go1.16.5.src.tar.gz (20MB)
```

下载对应操作系统的可执行文件，直接点击下一步安装即可。

本次示例在 Mac 机器上，下载的是 go1.16.5.darwin-amd64.pkg 文件。

查看当前系统的版本（macOS）： 

```sh
$ sw_vers                  
ProductName: Mac OS X
ProductVersion: 10.15.7
BuildVersion: 19H2
```

查看go命令路径，看看go语言程序放在哪里了？ 

```sh
$ which go
/usr/local/go/bin/go
```

Go 被安装在了 `/usr/local/go` 路径下。 查看一下其目录结构如下：

```sh
$ls -al /usr/local/go
total 384
drwxr-xr-x   20 root  wheel     640  6  4 01:18 .
drwxr-xr-x   23 root  wheel     736  6 18 14:33 ..
-rw-r--r--    1 root  wheel   55669  6  4 01:18 AUTHORS
-rw-r--r--    1 root  wheel    1339  6  4 01:18 CONTRIBUTING.md
-rw-r--r--    1 root  wheel  101673  6  4 01:18 CONTRIBUTORS
-rw-r--r--    1 root  wheel    1479  6  4 01:18 LICENSE
-rw-r--r--    1 root  wheel    1303  6  4 01:18 PATENTS
-rw-r--r--    1 root  wheel    1480  6  4 01:18 README.md
-rw-r--r--    1 root  wheel     397  6  4 01:18 SECURITY.md
-rw-r--r--    1 root  wheel       8  6  4 01:18 VERSION
drwxr-xr-x   22 root  wheel     704  6  4 01:18 api
drwxr-xr-x    4 root  wheel     128  6  4 02:36 bin
drwxr-xr-x    6 root  wheel     192  6  4 01:18 doc
-rw-r--r--    1 root  wheel    5686  6  4 01:18 favicon.ico
drwxr-xr-x    3 root  wheel      96  6  4 01:18 lib
drwxr-xr-x   14 root  wheel     448  6  4 01:18 misc
drwxr-xr-x    6 root  wheel     192  6  4 01:20 pkg
-rw-r--r--    1 root  wheel      26  6  4 01:18 robots.txt
drwxr-xr-x   69 root  wheel    2208  6  4 01:18 src
drwxr-xr-x  333 root  wheel   10656  6  4 01:18 test
```

在 /usr/local/go/bin/ 目录下有2个可执行文件，一个为 `go`,一个为 `gofmt` (格式化代码)。

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
go version go1.16.5 darwin/amd64

$ /usr/local/go/bin/go version
go version go1.16.5 darwin/amd64
```

### 代码编辑器选择

Go 采用的是 UTF-8 编码的文本文件存放源代码。随便选择一款常用的文本编辑器就可以做Go语言开发了。

这里推荐三款编辑器： 

(1) `VSCode`: 即 Visual Studio Code，是微软公司开源的一款轻量级、跨平台的代码编辑器。有丰富的插件生态系统。  

(2) `SublimeText`: 一个轻量、简洁、高效、跨平台的编辑器。可以无限期试用，不定时有激活弹出框提示。  

(3) `Goland`: 由 JetBrains 公司开发的一款收费的Go语言IDE。  


### 第一个 HelloWorld 程序

(1) 可以在任意目录下，创建一个文件名为: `hello.go`，代码如下：

```c
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!");
}
```

(2) 使用 go run 命令运行 `hello.go` 程序: 

```sh
$ go run hello.go 
Hello, World!
```

使用 go run 命令可以编译并直接运行程序。

(3) 也可以使用 `go build` 构建代码，并执行编译后的可执行文件: 

```sh
$ go build hello.go
$ ./hello 
Hello, World!
```

使用 go build 用于构建代码，主要检查是否会有编译错误，如果是一个可执行文件的源码（即是 main 包），就会直接生成一个可执行文件。

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

### 参考链接

https://golang.google.cn/  
https://www.runoob.com/go/go-tutorial.html  

### 更新记录

2018-12-08 编辑此文。  
2021-03-19 完善内容，更新到 go1.6.2 版本。  
2021-06-18 删除部分内容，更新到 go1.6.5 版本。  

[END]