---
title: rust-quick-start
date: 2019-08-19T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: ð§
tags:
- Rust
categories:
- Rust
- QuickStart
series:
- Themes Guide
image: 
---

Rust æ¯ä¸ç§éæç±»åçç¼è¯è¯­è¨ï¼æ»¡è¶³äºå¤§å¤æ°ç¨æ·ä½¿ç¨ C æ C++ è½å¤å®ç°çç®æ ã Rust è¯­è¨æ¯åå­å®å¨ä¸ä¸æä½ç³»ç»æ å³çï¼è¿æå³çå®å¯ä»¥å¨ä»»ä½è®¡ç®æºä¸è¿è¡ã


Cargo: the Rust build tool and package manager

When you install Rustup youâll also get the latest stable version of the Rust build tool and package manager, also known as Cargo. Cargo does lots of things:

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

æ¥ç rustc çæ¬ å cargo çæ¬ï¼

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
âââ Cargo.toml
âââ src
    âââ main.rs

1 directory, 2 files
```

Cargo.toml æä»¶æ¯ä»ä¹ï¼å®æ¯ Cargo ç¨æ¥æå»ºç¨åºçæä»¶ã

`Cargo.toml`: is the manifest file for Rust. Itâs where you keep metadata for your project, as well as dependencies.

`src/main.rs` is where weâll write our application code.


æ¥ç `Cargo.toml` åå®¹: 

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

ç¬¬ä¸æ¬¡æ§è¡å¾æ¢ã


```java
fn main() {
    println!("Hello, world!");
}
```

## æ·»å ä¾èµ

ç°å¨æä»¬æ¥ä¸ºåºç¨æ·»å ä¾èµãæ¨å¯ä»¥å¨ crates.ioï¼å³ Rust åçä»åºä¸­æ¾å°ææç±»å«çåºãå¨ Rust ä¸­ï¼æä»¬éå¸¸æåç§°ä½âcratesâã

å¨æ¬é¡¹ç®ä¸­ï¼æä»¬ä½¿ç¨äºåä¸º ferris-says çåºã

æä»¬å¨ Cargo.toml æä»¶ä¸­æ·»å ä»¥ä¸ä¿¡æ¯ï¼ä» crate é¡µé¢ä¸è·åï¼ï¼

```sh
[dependencies]
ferris-says = "0.2"
```

æ¥çè¿è¡ï¼

```
$ cargo build
```

ä¹å Cargo å°±ä¼å®è£è¯¥ä¾èµã

è¿è¡æ­¤å½ä»¤ä¼åå»ºä¸ä¸ªæ°æä»¶ `Cargo.lock`ï¼è¯¥æä»¶è®°å½äºæ¬å°æç¨ä¾èµåºçç²¾ç¡®çæ¬ã

è¦ä½¿ç¨è¯¥ä¾èµåºï¼æä»¬å¯ä»¥æå¼ `main.rs`ï¼ç¶åå¨å¶ä¸­æ·»å ä¸é¢è¿è¡ä»£ç ï¼

use ferris_says::say;

è¿æ ·æä»¬å°±å¯ä»¥ä½¿ç¨ ferris-says crate ä¸­å¯¼åºç say å½æ°äºã


##

ç°å¨æä»¬ç¨æ°çä¾èµåºç¼åä¸ä¸ªå°åºç¨ãå¨ main.rs ä¸­æ·»å ä»¥ä¸ä»£ç ï¼


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

ä¿å­å®æ¯åï¼æä»¬å¯ä»¥è¾å¥ä»¥ä¸å½ä»¤æ¥è¿è¡æ­¤åºç¨ï¼

cargo run

å¦æä¸åæ­£ç¡®ï¼æ¨ä¼çå°è¯¥åºç¨å°ä»¥ä¸åå®¹æå°å°äºå±å¹ä¸ï¼

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
