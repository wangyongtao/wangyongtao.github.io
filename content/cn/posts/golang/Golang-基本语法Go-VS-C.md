---
title: Go语言基本语言与C语言对比
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
- Golang
- QuickStart
series:
- Themes Guide
image: images/common/golang-1.jpeg
---

Go语言基本语言与C语言对比

### C语言与Go语言简介

C 语言是一种面向过程式的编程语言。1972 年，为了移植与开发 UNIX 操作系统，丹尼斯·里奇在贝尔电话实验室设计开发了 C 语言。UNIX 操作系统，C编译器，和几乎所有的 UNIX 应用程序都是用 C 语言编写的。由于各种原因，C 语言现在已经成为一种广泛使用的专业语言。最新的C语言标准是C18（2018）。

Go，又称 Golang，是一个 Google 公司开源的静态的、编译型编程语言。Go语言起源于 2007 年，并在 2009 年正式对外开源，在2012年发布了 Go 1 稳定版本。Go语言早期（go1.4版本和以前）使用`C语言`和`汇编`编写的，从 Go 1.5 起开始Go语言实现了自举（Bootstrapping），即使用Go语言自己编写的Go语言。

### 查看版本

```sh
$ go version
go version go1.16.5 darwin/amd64
```

```sh
$ gcc --version
Configured with: 
 --prefix=/Applications/Xcode.app/Contents/Developer/usr 
 --with-gxx-include-dir=/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
Apple clang version 12.0.0 (clang-1200.0.32.29)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```

### Hello World

C 语言版的 hello world: 

```c
// 源代码文件名为： hello.c
#include <stdio.h>

int main(int argc, char **argv) {
  printf("Hello, World!\n");
}
```

C 语言编译并运行:

```sh
# 使用 gcc 编译，默认生成可执行文件 a.out 
$ gcc hello.c

# 直接运行 a.out
$ ./a.out
Hello, World!
```

Go 语言版的 hello world: 

```go
// 源代码文件名为： hello.go
package main

import "fmt"

func main() {
  fmt.Printf("Hello, World!\n")
}
```

Go 语言编译并运行:

```sh
$ go build hello.go 
$ ./hello 
Hello, World!
```

可以使用 go run 直接编译运行 go 程序:

```go
$ go run hello.go 
Hello, World!
```

C语言和Go语言都是以 `main` 函数作为程序的开始。
Go语言需要指定包名，且包名必须是 `main`。
C 语言没有包名或命名空间。C++ 有命名空间(namespace)。

C 语言使用文件包含命令(`#include`)来引入对应的头文件（`.h` 文件）。
Go 语言使用 `import` 命令来引入其他包的文件 (这一点同 Java、JavaScript 等语言)。

### 定义变量

c 语言定义变量：

```c
/* if inside function, memory allocated on stack: */
int i;
int j = 3;

/* memory allocated on heap: */
int *ptr = malloc(sizeof *ptr);
/* if malloc fails, it returns NULL and sets errno to ENOMEM */
*ptr = 7;
```

go 语言定义变量：

```c
// memory allocated on stack:
var i int

// allocated on stack; type inferred from literal:
j := 3

// memory allocated on heap:
ptr := new(int)
*ptr = 7
```

定义变量，Go可以使用 `:=` 进行更省略的写法(比如 a := 0 等于 var a = 0)。


空值在 c 语言中为 null，在 go 语言中为 nil。
Go 语言中不能给变量赋值为 nil

### 定义常量

```c
#define PI 3.14
```

```go
const Pi = 3.14
```


### 变量交换值

```c
int x = 11, y = 22, tmp;

tmp = x;
x = y;
y = tmp;
```

```go
x := 11;
y := 22;
x, y = y, x
```

### 流程控制语句

if 语句：

C语言和Go语言，都有 if / else if / else 关键字，Go 语言可以省略小括号。

```c
//// c语言
int signum;

if (i > 0) {
  signum = 1;
} else if (i == 0) {
  signum = 0;
} else {
  signum = -1;
}

//// go 语言
var signum int

if x > 0 {
  signum = 1
} else if x == 0 {
  signum = 0
} else {
  signum = -1
}
```

switch 语句：

```c
// ====> c 语言：
/* switch expression must be an integer */
switch (i) {
case 0:
case 1:
  printf("i is boolean\n");
  break;
default:
  printf("i is not a boolean\n");
  break;
}

// ====> go 语言：
// switch expression can have any type
switch i {
case 0, 1:
  fmt.Println("i is boolean")
default:
  fmt.Println("i is not a boolean")
}
```

Go语言开始支持switch下的多条件判断，简化了很多代码。  
Go 编程语言中 select 语句的语法如下：

```c
// ====> go 语言：
select {
    case communication clause :
       	statement(s);      
    case communication clause :
      	statement(s);
    	// 可以定义任意数量的 case  
       	// default 可选 
    default : 
       	statement(s);
}
```

select 随机执行一个可运行的 case。如果没有 case 可运行，它将阻塞，直到有 case 可运行。一个默认的子句应该总是可运行的。


### 循环语句

for 循环： 

for 循环是一个循环控制结构，可以执行指定次数的循环。 Go 语言的 for 循环有 3 种形式，只有其中的一种使用分号。

（1）和 C 语言的 for循环 一样：

for init; condition; post { }

```c
// ====> c 语言：
int i, n;
for (i = 1, n = 1; i <= 10; ++i) {
  n *= i;
}

// ====> go 语言：
var i, n int
// Initialization and afterthought must be single
// statements; there is no comma operator.
n = 1
for i = 1; i <= 10; i++ {
  n *= i;
}
```

（2）和 C 语言的 `while` 一样：for condition { }

```c
// ====> c 语言：
int i = 0;
while (i < 10) {
  ++i;
}

// ====> go 语言：
i := 0
for i < 10 {
  i++
}
```

（3）和 C 语言的 `for(;;)` 一样，死循环：`for { }`

```c
// c 语言 for 死循环：
for (;;) {
  // some code
}
// c 语言 while 死循环：
while (1) {
  // some code
}

// go 语言 for 死循环：
for {
  // some code
}
```

Go语言有for .. range ..简化代码


### 参考链接

https://hyperpolyglot.org/c  
https://baijiahao.baidu.com/s?id=1698155444836695087  
https://www.runoob.com/go/go-tutorial.html  

[END]