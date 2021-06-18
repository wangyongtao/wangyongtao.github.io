---
title: Laravel环境安装与配置
date: 2017-04-01T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- PHP
- Laravel
categories:
- PHP
- Laravel
series:
- PHP Guide
image: images/common/laravel-1.jpeg
---

### 安装或升级 composer :

Composer 是 PHP 用来管理依赖（dependency）关系的工具。 

2020年10月24日已经发布了 Composer 2.0 版本。Composer 2.0 内部重构了依赖更新的方式，性能有了很大的提示，推荐升级。

```sh
$ composer self-update
Upgrading to version 2.1.3 (stable channel).
Use composer self-update --rollback to return to version 2.0.3
```

查看升级后的版本： 

```sh
composer --version
Composer version 2.1.3 2021-06-09 16:31:20
```

### 通过 composer 安装 Laravel:

要创建基于Composer的新项目，可以使用create-project命令。格式如下：

```sh
# 创建一个目录为 ExampleAppName 的项目
$ composer create-project laravel/laravel ExampleAppName
```

在这里，我们创建一个名为 Laravel 的项目：

```sh
$ composer create-project laravel/laravel Laravel
Creating a "laravel/laravel" project at "./Laravel"
Installing laravel/laravel (v8.5.20)
  - Downloading laravel/laravel (v8.5.20)
  - Installing laravel/laravel (v8.5.20): Extracting archive
Created project in /Users/wangtom/Code/Laravel
> @php -r "file_exists('.env') || copy('.env.example', '.env');"
Loading composer repositories with package information

Updating dependencies
Lock file operations: 104 installs, 0 updates, 0 removals
 (。。。略。。。)
  - Installing phpspec/prophecy (1.13.0): Extracting archive
  - Installing phar-io/version (3.1.0): Extracting archive
  - Installing phar-io/manifest (2.0.1): Extracting archive
  - Installing myclabs/deep-copy (1.10.2): Extracting archive
  - Installing phpunit/phpunit (9.5.5): Extracting archive
61 package suggestions were added by new dependencies, use `composer suggest` to see details.
Generating optimized autoload files
> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi
Discovered Package: facade/ignition
Discovered Package: fideloper/proxy
Discovered Package: fruitcake/laravel-cors
Discovered Package: laravel/sail
Discovered Package: laravel/tinker
Discovered Package: nesbot/carbon
Discovered Package: nunomaduro/collision
Package manifest generated successfully.
74 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
> @php artisan key:generate --ansi
Application key set successfully.
```

查看生成的 Laravel 目录结构

```sh
$ cd Laravel
$ ls -al .
total 632
drwxr-xr-x  26 wangtom  staff     832  6 16 18:31 .
drwxr-xr-x  60 wangtom  staff    1920  6 16 18:26 ..
-rw-r--r--   1 wangtom  staff     220  6 15 23:48 .editorconfig
-rw-r--r--   1 wangtom  staff     920  6 16 18:31 .env
-rw-r--r--   1 wangtom  staff     869  6 15 23:48 .env.example
-rw-r--r--   1 wangtom  staff     111  6 15 23:48 .gitattributes
-rw-r--r--   1 wangtom  staff     207  6 15 23:48 .gitignore
-rw-r--r--   1 wangtom  staff     181  6 15 23:48 .styleci.yml
-rw-r--r--   1 wangtom  staff    3810  6 15 23:48 README.md
drwxr-xr-x   7 wangtom  staff     224  6 15 23:48 app
-rwxr-xr-x   1 wangtom  staff    1686  6 15 23:48 artisan
drwxr-xr-x   4 wangtom  staff     128  6 15 23:48 bootstrap
-rw-r--r--   1 wangtom  staff    1624  6 15 23:48 composer.json
-rw-r--r--   1 wangtom  staff  268017  6 16 18:31 composer.lock
drwxr-xr-x  16 wangtom  staff     512  6 15 23:48 config
drwxr-xr-x   6 wangtom  staff     192  6 15 23:48 database
-rw-r--r--   1 wangtom  staff     473  6 15 23:48 package.json
-rw-r--r--   1 wangtom  staff    1202  6 15 23:48 phpunit.xml
drwxr-xr-x   7 wangtom  staff     224  6 15 23:48 public
drwxr-xr-x   6 wangtom  staff     192  6 15 23:48 resources
drwxr-xr-x   6 wangtom  staff     192  6 15 23:48 routes
-rw-r--r--   1 wangtom  staff     563  6 15 23:48 server.php
drwxr-xr-x   5 wangtom  staff     160  6 15 23:48 storage
drwxr-xr-x   6 wangtom  staff     192  6 15 23:48 tests
drwxr-xr-x  44 wangtom  staff    1408  6 16 18:31 vendor
-rw-r--r--   1 wangtom  staff     559  6 15 23:48 webpack.mix.js
```

使用 Laravel 内置命令启动服务：

```sh
$ php artisan serve
Starting Laravel development server: http://127.0.0.1:8000
[Wed Jun 16 18:43:45 2021] PHP 8.0.3 Development Server (http://127.0.0.1:8000) started
[Wed Jun 16 18:43:52 2021] 127.0.0.1:57783 Accepted
[Wed Jun 16 18:43:52 2021] 127.0.0.1:57784 Accepted
[Wed Jun 16 18:43:53 2021] 127.0.0.1:57783 Closing
[Wed Jun 16 18:43:53 2021] 127.0.0.1:57784 [200]: GET /favicon.ico
[Wed Jun 16 18:43:53 2021] 127.0.0.1:57784 Closing
```

打开浏览器，访问 `http://127.0.0.1:8000/` 即可以看到结果。


