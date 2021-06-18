---
title: PHP与Node中使用bcrypt算法存储密码
date: 2019-08-19T17:00:00+08:00
# # description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- PHP
- Node
- bcrypt
categories:
- themes
- syntax
series:
- Themes Guide
image: images/common/php-1.jpeg
---

在软件开发过程中，对于如密码这样的信息，我们不能直接以明文的方式存储在数据库中

在Node中我们若要利用bcrypt算法对数据加密，可以使用第三方模块bcryptjs

npm install bcryptjs

password_verify() 函数用于验证密码是否和散列值匹配。
password_hash() 函数用于创建密码的散列（hash）。

bool password_verify ( string $password , string $hash )

参数说明：
password: 用户的密码。
hash: 一个由 password_hash() 创建的散列值。


string password_hash ( string $password , int $algo [, array $options ] )

password: 一个由 password_hash() 创建的散列值。
algo: 一个用来在散列密码时指示算法的密码算法常量。
options: 一个包含有选项的关联数组。

$algo : 支持的算法 PASSWORD_DEFAULT - 使用 bcrypt 算法 (PHP 5.5.0 默认)。 


```php
function login(string $loginName, string $loginPassword) : bool
{
    // 从数据库查询用户信息
    // (new User())->where('name', $loginName)->where('valid', 1)->first();
    $userInfo = [
		'name' => 'admin111',
		'password' => '$2y$07$BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq',
	];

    // 校验输入的用户密码 与 数据库存储的是否一致
    if (password_verify($loginPassword, $userInfo['password'])) {
        return true;
    }

    return false;
}

$userName = 'admin111';
$userPass = 'rasmuslerdorf';

$result = login($userName, $userPass);
var_dump($result);
```

```php
$password = password_hash("rasmuslerdorf", PASSWORD_DEFAULT);
var_dump($password);

$pwd1 = '$2y$10$BawJulWXzhL3kGVwAHDSieKklqr.01/p3Qix2Ly3EpSaDMT.NefvO';
$pwd2 = '$2y$10$Z47m4aKOPQ.6hPQyraGv1u5COtUTn51t1TYVfSlH.xKDjchpPoJNy';

var_dump(password_verify('rasmuslerdorf', $pwd1)); // bool(true)
var_dump(password_verify('rasmuslerdorf', $pwd2)); // bool(true)
```



```
db.getCollection('users').find({'username': 'admin111'}).sort({'_id':-1})
```

```
{
    "_id" : ObjectId("5de780d9655694f90649d28a"),
    "createdAt" : ISODate("2019-12-04T09:48:09.314Z"),
    "updatedAt" : ISODate("2021-01-27T02:00:25.036Z"),
    "username" : "admin111",
    "realname" : "Web后端技术",
    "email" : "admin111@wang123.net",
    "phone" : "13811112222",
    "password" : "$2a$10$1wBgc8M4wwJAnzD4Ptcx6eGQ1gIP8DUqL5.cvTyJCaM7o5ezSMJQ2",
    "roles" : [ 
        ObjectId("5588ccc679faf34f2cfc214d")
    ],
    "enabled" : true
}
```

```
$pwd = '$2a$10$1wBgc8M4wwJAnzD4Ptcx6eGQ1gIP8DUqL5.cvTyJCaM7o5ezSMJQ2';

var_dump(password_verify('12345678', $pwd)); // bool(true)
```

用户 admin111 的密码为 `12345678` (不建议用这么简单的密码)。

MongoDB 保存的加密后的密码为：$2a$10$1wBgc8M4wwJAnzD4Ptcx6eGQ1gIP8DUqL5.cvTyJCaM7o5ezSMJQ2

在终端用 php 的 password_verify 函数校验一下： 

```
$ php -a 
Interactive shell

php > $pwd = '$2a$10$1wBgc8M4wwJAnzD4Ptcx6eGQ1gIP8DUqL5.cvTyJCaM7o5ezSMJQ2';
php > 
php > var_dump(password_verify('12345678', $pwd));
bool(true)
php > 
```

可以看到， 校验通过。
