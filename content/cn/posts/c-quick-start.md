---
title: 第一个c语言程序
date: 2019-08-19T17:00:00+08:00
description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Rust
categories:
- Rust
- QuickStart
series:
- Themes Guide
image: images/common/clang-1.jpeg
---

C语言是一门面向过程的的通用程序设计语言，广泛应用于底层开发。

C语言是普适性最强的一种计算机程序编辑语言，它不仅可以发挥出高级编程语言的功用，还具有汇编语言的优点。

C语言诞生于美国的贝尔实验室，由 D.M.Ritchie 以B语言为基础发展而来。在它的主体设计完成后，Thompson 和 Ritchie 用它完全重写了 UNIX 操作系统.

1989年，ANSI 发布了第一个完整的 C语言 标准 —— ANSI X3.159—1989，简称“C89”，人们习惯称其为“ANSI C”。

C89在1990年被国际标准组织 ISO(International Standard Organization)采纳，给予的名称为：ISO/IEC 9899，所以ISO/IEC9899: 1990也通常被简称为“C90”。

1999年，在做了一些必要的修正和完善后，ISO发布了新的C语言标准，命名为ISO/IEC 9899：1999，简称“C99”。

在2011年12月8日，ISO又正式发布了新的标准，称为ISO/IEC9899: 2011，简称为“C11”。
 

## 第一个c语言程序： 


hell.c 文件内容:

```c
#include <stdio.h>
 
// 代码从 main() 函数开始执行。 
int main()
{
    // 我的第一个 C语言 程序
    printf("Hello, World! \n");
 
    return 0;
}
```


## 常见的编译器简介

（1）GCC编译器

GCC原名为GNU C语言编译器（GNU C Compiler），只能处理C语言。但其很快扩展，变得可处理C++，后来又扩展为能够支持更多编程语言，如Fortran、Pascal、Objective -C、Java、Ada、Go以及各类处理器架构上的汇编语言等，所以改名GNU编译器套件（GNU Compiler Collection）。


（2）Clang 编译器 
Low Level Virtual Machine (LLVM) 是一个开源的编译器架构，它已经被成功应用到多个应用领域。Clang 是 LLVM 的一个编译器前端，它目前支持 C, C++, Objective-C 以及 Objective-C++ 等编程语言。


（3）VC6.0  (已淘汰) 

Microsoft Visual C++ 6.0，简称VC6.0，是微软于1998年推出的一款C++编译器。Microsoft Visual C++ 6.0对windows7和windows8的兼容性较差。

（4）Turbo C (已淘汰)  

Turbo C 是美国Borland公司的产品，Borland公司是一家专门从事软件开发、研制的公司。 1987年首次推出Turbo C 1.0 产品, 其中使用了全然一新的集成开发环境,  大大方便了程序的开发。Turbo C 2.0 则是Borland公司1989年出版的。

（5）Visual Studio 

Microsoft Visual Studio（简称VS）是美国微软公司的开发工具包系列产品。2017年3月8日，微软发布 Visual Studio 2017。 2019年4月2日，微软发布Visual Studio2019.


### 使用 GCC 编译并执行

查看 gcc 版本： 


```sh
$ gcc --version
Configured with: 
--prefix=/Library/Developer/CommandLineTools/usr 
--with-gxx-include-dir=/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
Apple clang version 12.0.0 (clang-1200.0.32.2)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```

使用 gcc 编译并执行：

```sh
$ gcc hello.c && ./a.out
Hello, World! 
```

说明，命令 `gcc hello.c` 表示编译 hello.c 文件里的代码。 `./a.out` 表示执行一个可执行文件 `a.out`。

`&&` 表示 and，即执行完第一个命令后，继续执行第二个命令。

可执行文件 a.out 是，编译c语言代码后生成的默认的可执行文件。

我们可以指定生成的可执行文件的名称，使用 `-o` 参数即可。

```sh
$ gcc hello.c -o hello2020
$ ./hello2020 
Hello, World! 
```

这里我们使用了 `-o` 参数指定生成的可执行文件名称为 `hello2020`。


### 使用 Clang 编译并执行

查看 clang 版本： 

```sh
$ clang --version
Apple clang version 12.0.0 (clang-1200.0.32.2)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```

使用 clang 编译并执行：

```sh
$ clang hello.c && ./a.out 
Hello, World! 
```


## 参考 Reference

http://c.biancheng.net/c/ 
https://www.runoob.com/cprogramming/c-tutorial.html   
https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80  
