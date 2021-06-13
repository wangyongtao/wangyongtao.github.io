---
title: spring-rest-api
date: 2019-08-19T17:00:00+08:00
description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- Spring
categories:
- QuickStart
series:
- Themes Guide
image: 
---


```sh
$ sw_vers                             
ProductName:	Mac OS X
ProductVersion:	10.15.1
BuildVersion:	19B88
```


```sh
$ java --version
java 13.0.1 2019-10-15
Java(TM) SE Runtime Environment (build 13.0.1+9)
Java HotSpot(TM) 64-Bit Server VM (build 13.0.1+9, mixed mode, sharing)
```


安装 SDKMAN

```sh
$ curl -s "https://get.sdkman.io" | bash
```

查看 `sdk list gradle` 命令查看 `gradle` 的版本: 

```sh
$ sdk list gradle  
```

查看安装的版本： 

```sh 
$ sdk version
SDKMAN 5.7.4+362
```



直接安装 gradle： 

```sh
$ sdk install gradle
Downloading: gradle 6.0
In progress...
################################# 100.0%
Installing: gradle 6.0
Done installing!
Setting gradle 6.0 as default.
```

使用 `gradle --version` 查看 Gradle 的版本: 

```sh
$ gradle --version

Welcome to Gradle 6.0!

Here are the highlights of this release:
 - Substantial improvements in dependency management, including
   - Publishing Gradle Module Metadata in addition to pom.xml
   - Advanced control of transitive versions
   - Support for optional features and dependencies
   - Rules to tweak published metadata
 - Support for Java 13
 - Faster incremental Java and Groovy compilation
 - New Zinc compiler for Scala
 - VS2019 support
 - Support for Gradle Enterprise plugin 3.0

For more details see https://docs.gradle.org/6.0/release-notes.html

------------------------------------------------------------
Gradle 6.0
------------------------------------------------------------

Build time:   2019-11-08 18:12:12 UTC
Revision:     0a5b531749138f2f983f7c888fa7790bfc52d88a
Kotlin:       1.3.50
Groovy:       2.5.8
Ant:          Apache Ant(TM) version 1.10.7 compiled on September 1 2019
JVM:          13.0.1 (Oracle Corporation 13.0.1+9)
OS:           Mac OS X 10.15.1 x86_64
```



## Reference 

https://spring.io/guides/gs/rest-service/




[END]