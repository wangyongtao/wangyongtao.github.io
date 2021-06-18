---
title: rust-quick-start
date: 2019-08-19T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
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
image: 
---

Rust 是一种静态类型的编译语言，满足了大多数用户使用 C 或 C++ 能够实现的目标。 Rust 语言是内存安全且与操作系统无关的，这意味着它可以在任何计算机上运行。


Cargo: the Rust build tool and package manager

When you install Rustup you’ll also get the latest stable version of the Rust build tool and package manager, also known as Cargo. Cargo does lots of things:

build your project with cargo build
run your project with cargo run
test your project with cargo test
build documentation for your project with cargo doc
publish a library to crates.io with cargo publish
To test that you have Rust and Cargo installed, you can run this in your terminal of choice:

$ cargo --version
cargo 1.51.0 (43b129a20 2021-03-16)


```sh 
$ curl https://sh.rustup.rs -sSf | sh
```

source $HOME/.cargo/env

查看 rustc 版本 和 cargo 版本：

```sh
$ rustc --version
rustc 1.51.0 (2fd73fabe 2021-03-23)
```

```sh
$ cargo --version
cargo 1.51.0 (43b129a20 2021-03-16)
```

```sh
$ cargo new hello-rust
  Created binary (application) `hello-rust` package
```


```sh
$ tree hello-rust
hello-rust
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

Cargo.toml 文件是什么？它是 Cargo 用来构建程序的文件。

`Cargo.toml`: is the manifest file for Rust. It’s where you keep metadata for your project, as well as dependencies.

`src/main.rs` is where we’ll write our application code.


查看 `Cargo.toml` 内容: 

```sh
[package]
name = "hello-rust"
version = "0.1.0"
authors = ["wangyongtao <wangtom365@qq.com>"]
edition = "2018"

[dependencies]
```

cargo new generates a "Hello, world!" project for us! We can run this program by moving into the new directory that we made and running this in our terminal:

```sh 
$ cd hello-rust 
$ cargo run
   Compiling hello-rust v0.1.0 (/Users/wangtom/Code/openapi/online-test/rust/hello-rust)
    Finished dev [unoptimized + debuginfo] target(s) in 10.21s
     Running `target/debug/hello-rust`
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/hello-rust`
Hello, world!
```

第一次执行很慢。


```java
fn main() {
    println!("Hello, world!");
}
```

## 添加依赖

现在我们来为应用添加依赖。您可以在 crates.io，即 Rust 包的仓库中找到所有类别的库。在 Rust 中，我们通常把包称作“crates”。

在本项目中，我们使用了名为 ferris-says 的库。

我们在 Cargo.toml 文件中添加以下信息（从 crate 页面上获取）：

```sh
[dependencies]
ferris-says = "0.2"
```

接着运行：

```
$ cargo build
```

之后 Cargo 就会安装该依赖。

运行此命令会创建一个新文件 `Cargo.lock`，该文件记录了本地所用依赖库的精确版本。

要使用该依赖库，我们可以打开 `main.rs`，然后在其中添加下面这行代码：

use ferris_says::say;

这样我们就可以使用 ferris-says crate 中导出的 say 函数了。


##

现在我们用新的依赖库编写一个小应用。在 main.rs 中添加以下代码：


```java
use ferris_says::say;
use std::io::{stdout, BufWriter};

fn main() {
	println!("Hello, world!");


    let stdout = stdout();
    let message = String::from("Hello fellow Rustaceans!");
    let width = message.chars().count();

    let mut writer = BufWriter::new(stdout.lock());
    say(message.as_bytes(), width, &mut writer).unwrap();
}
```

保存完毕后，我们可以输入以下命令来运行此应用：

cargo run

如果一切正确，您会看到该应用将以下内容打印到了屏幕上：

```sh
$ cargo run
   Compiling hello-rust v0.1.0 (/Users/wangtom/Code/rust/hello-rust)
    Finished dev [unoptimized + debuginfo] target(s) in 0.98s
     Running `target/debug/hello-rust`
Hello, world!
 __________________________
< Hello fellow Rustaceans! >
 --------------------------
        \
         \
            _~^~^~_
        \) /  o o  \ (/
          '_   -   _'
          / '-----' \
```


## Reference
 
https://www.rust-lang.org/zh-CN/learn/get-started  
https://doc.rust-lang.org/cargo/getting-started/first-steps.html  
