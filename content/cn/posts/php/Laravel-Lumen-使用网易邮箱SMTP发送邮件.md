---
title: Laravel-Lumen-使用网易邮箱SMTP发送邮件
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
- CSV
categories:
- PHP
- QuickStart
series:
- PHP Guide
image: images/common/laravel-1.jpeg
---

Laravel-Lumen-使用网易邮箱SMTP发送邮件

Laravel 是目前最流行的PHP框架，而 Lumen 是 Laravel 的精简版，主要用于后端接口开发。

在 Laravel 框架汇总，邮件发送服务是基于 Symfony 组件 Swift Mailer 的。

本文记录了在 Lumen / Laravel 环境中，使用网易邮箱 SMTP 发送邮件的主要步骤，希望对大家有一些参考价值。


### 获取网易邮箱的服务器和授权码:

登录网易邮箱:  http://mail.163.com/

获取服务器地址：点击【设置】 > 【POP3/SMTP/IMAP】选项

可以查看到，服务器地址:

```
POP3 服务器: pop.163.com     
SMTP 服务器: smtp.163.com     
IMAP 服务器: imap.163.com
```

POP3 是Post Office Protocol 3的简称，即邮局协议的第3个版本，用来接收电子邮件的。POP3协议允许电子邮件客户端下载服务器上的邮件，但是在客户端的操作（如移动邮件、标记已读等），不会反馈到服务器上。

SMTP 的全称是“Simple Mail Transfer Protocol”，即简单邮件传输协议。

IMAP 全称是Internet Mail Access Protocol，即交互式邮件存取协议，它是跟POP3类似邮件访问标准协议之一。IMAP提供webmail 与电子邮件客户端之间的双向通信，客户端的操作都会反馈到服务器上，对邮件进行的操作，服务器上的邮件也会做相应的动作。

### 获取客户端授权密码：

开启 IMAP 服务： 

（1）点击【设置】，选择【POP3/SMTP/IMAP】；
（2）在【开启服务】里，选择开启【IMAP/SMTP服务】；
（3）会让手机发送短信验证，开启后记录下【客户端授权密码】，稍后在发邮件的时候要用到这个密码。

在【授权密码管理】部分，也可以增加密码。

> 授权密码管理: 
> 授权码是用于登录第三方邮件客户端的专用密码。
> 适用于登录以下服务: POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务。
 0000000000000000000000..................
 333333333333333333333333333333
 3333333333333333333333333333333333333333333333333333222222222222222222222222222222222222222222222222222222222222222222222222222222

### 添加发邮件模块

Laravel 框架已经包含了邮件模块，不需要安装。

由于 Lumen 是简化版的 Laravel, 需要添加 illuminate/mail 模块:

执行 “composer require” 命令， 安装 `illuminate/mail`模块。

```sh
$ composer require illuminate/mail
```

或者，修改 composer.json 文件中 require 部分，再执行 composer up 安装，文件 composer.json 的 require 部分配置如下:

```json
"require": {
    "php": "^7.3|^8.0",
    "cnwyt/user-agent-parser": "dev-master",
    "guzzlehttp/guzzle": "^7.3",
    "illuminate/mail": "^8.47",
    "laravel/lumen-framework": "^8.0",
}
```

### 配置文件修改:


（1）修改配置文件 `.env`:

Laravel/Lumen 的系统配置一般都配置项目根目录的 “.env” 文件中，然后再代码中通过 `env()` 方法获取。

打开配置文件“.env”文件，新增以下配置:

```sh
MAIL_MAILER=smtp
MAIL_HOST=smtp.163.com
MAIL_PORT=25
MAIL_USERNAME=cnwytnet@163.com
MAIL_PASSWORD=MAIL_PASSWORD_BXVQLZ
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=cnwytnet@163.com
MAIL_FROM_NAME="${APP_NAME}"
```

注意，MAIL_PASSWORD 的配置就是上边获取的【客户端授权密码】。

（2）需要增加 mail.php 配置文件：

确保 Laravel 或 Luemn 项目中存在 app/config/mail.php 配置文件。

Laravel 框架已经包含了此配置文件，不需新增。Lumen 项目可能不存在，需要从 Laravel 代码中复制一份， 或者直接添加以下配置代码:

```php
<?php

return [
    // 默认使用的邮件服务类型
    'default' => env('MAIL_MAILER', 'smtp'),
    // 邮件服务配置
    'mailers' => [
        'smtp' => [
            'transport' => 'smtp',
            'host' => env('MAIL_HOST', 'smtp.mailgun.org'),
            'port' => env('MAIL_PORT', 587),
            'encryption' => env('MAIL_ENCRYPTION', 'tls'),
            'username' => env('MAIL_USERNAME'),
            'password' => env('MAIL_PASSWORD'),
            'timeout' => null,
            'auth_mode' => null
        ],
        'ses' => [
            'transport' => 'ses'
        ],
        'mailgun' => [
            'transport' => 'mailgun'
        ],
        'postmark' => [
            'transport' => 'postmark'
        ],
        'sendmail' => [
            'transport' => 'sendmail',
            'path' => '/usr/sbin/sendmail -bs'
        ],
        'log' => [
            'transport' => 'log',
            'channel' => env('MAIL_LOG_CHANNEL')
        ],
        'array' => [
            'transport' => 'array'
        ]
    ],
    // Markdown Mail Settings
    'markdown' => [
        'theme' => 'default',
        'paths' => [
            resource_path('views/vendor/mail')
        ]
    ]
];
```

注意，该配置文件的结构在 Laravel 5+ 与 Laravel 8+ 中略有不同。

### 创建发邮件脚本


在 Laravel 框架中，可以使用 php artisan 命令创建脚本文件:

```sh
$ php artisan make:command SendEmailCommand
Console command created successfully.
```

该命令会在自动创建一个类名为 “SendEmailCommand” 的脚本文件。 

其路径是: app/Console/Command/SendMailCommand.php 

该脚本的具体初始内容如下:

```php
<?php
namespace App\Console\Commands;

use Illuminate\Console\Command;

class SendEmailCommand extends Command
{
	// 脚本的命令名称
    protected $signature = 'command:name';
	// 脚本的描述
    protected $description = 'Command description';
	// 构造方法
    public function __construct()
    {
        parent::__construct();
    }
	// 具体的脚本内容写在这里
    public function handle()
    {
        return 0;
    }
}
```


在 Lumen 框架中，可以手动创建该脚本文件，用来发送邮件。

打开文件，引入 `Mail` 门面 (facade)， 使用 `Mail::raw()` 方法发送邮件: 

其代码内容如下:

```
// 发送 纯文本邮件
Mail::raw($content, function ($message) use ($toMail, $subject) {             
    $message->subject($subject);             
    $message->to($toMail);   
});
```

### 注册脚本

使用低版本Laravel 或者 Lumen 框架时，需要手动将脚本文件加入到 app/Console/Kernel.php 中，才可以执行命令:

```php
protected $commands = [         
    Commands\SendMailCommand::class, //测试发邮件脚本     
];
```

在 Laravel 5.5以上版本中，已默认注册了所有 Commands 目录下的脚步文件了，不需要再手动添加在 Kernel.php 中了:

```
/**
 * Register the commands.
 *
 * @return void
 */
protected function commands()
{
    // 加载所有 Commands 下脚本
    $this->load(__DIR__.'/Commands');

    require base_path('routes/console.php');
}
```


### 执行发邮件操作

使用 php artisan 命令可以查看目前可用的脚本列表, 可以看到我们新加的脚本命令 “test:send-mail”:

```
$ php artisan
test
  test:send-mail  SendMail:测试邮件发送
```

执行发送邮件脚本:

```
$ php artisan test:send-mail
```

不出意外的话，邮件发送成功。

查看发件人的发件箱，或者查看收件人的收件箱，确认一下吧。


### 使用模板邮件

上边我们发送的是纯文本的邮件，但是我们常用的都是带有模板的邮件。

在 app/resources/views 目录下， 创建一个 emails 目录，创建一个 test.blade.php 邮件模板文件: 


使用 Mail::send() 方法发送: 

```php
// 邮件模板文件
$view  = 'emails.test';
// 模板展示数据
// $data  = ['content' => $content,];
$data  = [
    'content' => $content,
    'logo'    => 'https://gitee.com/phpspace/php-demo/raw/master/Laravel-Demo/public/images/qrcode_344.jpg',
];
// 添加附件
$attach = "/Users/wangtest/code/php-demo/laravel-demo/public/robots.txt";

return Mail::send($view, $data, function ($message) use ($toMail, $subject, $attach) {             
    $message->subject($subject);             
    $message->to($toMail); 
    $message->attach($attach);   
});
```

![邮件发送截图](https://oscimg.oschina.net/oscnet/4b9117421cd0f8485b980505d881c52e5ed.jpg)

### 常见的报错与解决办法

这里收集了一些常见的错误，可能不同的 Laravel 版本，提示信息略有不同。

报错1: 没有正常设置配置文件，报530错误 (Lavavel5.5): 

```php 
In AbstractSmtpTransport.php line 419:  
Expected response code 250 but got code "530", with message "530 5.7.1 Authentication required"
```

未正确配置 `.env` 配置文件。 比如 MAIL_HOST，在`.env`中配置默认的邮件服务配置是:`MAIL_HOST=mailhog`，需要修改。

```php
# 执行发送邮件脚本
$ php artisan test:send-mail
  Swift_TransportException 
  Connection could not be established with host mailhog :
  stream_socket_client(): php_network_getaddresses: getaddrinfo failed: 
  nodename nor servname provided, or not known
```

报错2: 授权码认证失败: 

授权码错误 (Lavavel5.5): 

```php
In AuthHandler.php line 181:
Failed to authenticate on SMTP server with username "cnwytnet@163.com" using 2 possible authenticators 
```

不填授权码 MAIL_PASSWORD 或者 MAIL_PASSWORD 错误 (Lavavel5.4):

```php 
[Swift_TransportException] 
Failed to authenticate on SMTP server with username “cnwytnet@163.com” using 2 possible authenticators
```

注意 MAIL_PASSWORD 不是邮箱的密码，而是授权码。

报错3: 邮件地址 MAIL_FROM_ADDRESS 必须和 MAIL_USERNAME 不一致:

```php
# 执行发送邮件脚本
$ php artisan test:send-mail
 Swift_TransportException 
 Failed to authenticate on SMTP server with username "cnwytnet@163.com" using 2 possible authenticators. 
  Authenticator LOGIN returned Expected response code 235 but got code "535", 
  with message "535 Error: authentication failed".
```


## 调试邮件:
    
可以在配置文件中，将邮件驱动改成 MAIL_DRIVER=log, 就可以在本地日志中看到邮件内容了，这在测试的时候会很有用。

打开配置文件 `.env`，修改邮件驱动为 MAIL_DRIVER=log， 执行邮件发送脚本，将会把邮件发送内容保存到 storage/logs/laravel.log 中。 

比如，发送纯文本邮件时，实例内容如下:

```
[2018-06-13 02:52:17] local.DEBUG: Message-ID: <c75569f9a301cbb32b6ef7b0b6c78d09@swift.generated>
Date: Wed, 13 Jun 2018 02:52:17 +0000
Subject: =?utf-8?Q?=5BTEST=5D=E6=B5=8B=E8=AF=95?=
 =?utf-8?Q?=E9=82=AE=E4=BB=B6=E6=A0=87=E9=A2=98?= SendMail - 2018-06-13
 02:52:17
From: cnwytnet <cnwytnet@163.com>
To: wangtom365@qq.com
MIME-Version: 1.0
Content-Type: text/plain; charset=utf-8
Content-Transfer-Encoding: quoted-printable

Hi, 这是一封来自Laravel的测试邮件.  
```

具体代码可以在码云查看: https://gitee.com/phpspace/php-demo

## 拓展内容


Swift Mailer  

Swift Mailer， 是由 symfony 开发的一个邮件发送类库。其网址是: swiftmailer.symfony.com。

### 参考链接

https://laravel.com/docs/5.4/mail  
https://laravel.com/docs/8.x/mail
http://laravelacademy.org/post/1986.html  
http://help.163.com/10/0312/13/61J0LI3200752CLQ.html  
http://help.163.com/09/1223/14/5R7P6CJ600753VB8.html

### 更新记录

2017-04-01 基于 Lumen 5.4 新增文章  
2017-06-13 基于 Laravel 5.5/5.6 完善内容  
2021-06-17 基于 Laravel 8+ 完善内容  