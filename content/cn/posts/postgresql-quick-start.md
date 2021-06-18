---
title: PostgreSQL入门-安装与基本使用（Ubuntu）
date: 2019-08-19T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
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
- Databases
- QuickStart
series:
- Themes Guide
image: images/common/postgresql-1.jpeg
---

PostgreSQL入门-安装与基本使用（Ubuntu）

PostgreSQL 是一个免费的对象-关系数据库服务器(ORDBMS)，号称是 "世界上最先进的开源关系型数据库"。

PostgreSQL 是以加州大学计算机系开发的 POSTGRES 4.2版本为基础的对象关系型数据库。

今天在Ubuntu系统上，我们一起来安装并简单使用一下PostgreSQL数据库。

## 1.查看当前系统版本:

```sh
$ cat /etc/issue
Ubuntu 16.04.6 LTS \n \l

$ sudo lsb_release -a
LSB Version:	
core-9.20160110
ubuntu0.2-amd64:core-9.20160110
ubuntu0.2-noarch:security-9.20160110
ubuntu0.2-amd64:security-9.20160110
ubuntu0.2-noarch
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.6 LTS
Release:	16.04
Codename:	xenial
```

系统是 Ubuntu 16.04.6 LTS。

## 2.安装 PostgreSQL 

```sh
$ sudo apt-get install postgresql
```

执行实例如下: 

```sh
$ sudo apt-get install postgresql
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libpq5 
  postgresql-9.5 
  postgresql-client-9.5 
  postgresql-client-common 
  postgresql-common 
  postgresql-contrib-9.5 
  ssl-cert
 … …
Creating config file /etc/postgresql-common/createcluster.conf with new version
Creating config file /etc/logrotate.d/postgresql-common with new version
Building PostgreSQL dictionaries from installed myspell/hunspell packages...
Removing obsolete dictionary files:
Setting up postgresql-9.5 (9.5.19-0ubuntu0.16.04.1) ...
Creating new cluster 9.5/main ...
  config /etc/postgresql/9.5/main
  data   /var/lib/postgresql/9.5/main
  locale en_US.UTF-8
  socket /var/run/postgresql
  port   5432
update-alternatives: using /usr/share/postgresql/9.5/man/man1/postmaster.1.gz to provide /usr/share/man/man1/postmaster.1.gz (postmaster.1.gz) in auto mode
Setting up postgresql (9.5+173ubuntu0.2) ...
Setting up postgresql-contrib-9.5 (9.5.19-0ubuntu0.16.04.1) ...
Processing triggers for libc-bin (2.23-0ubuntu11) ...
Processing triggers for ureadahead (0.100.0-19.1) ...
Processing triggers for systemd (229-4ubuntu21.21) ...
```

默认已经安装了 postgresql 的服务器(postgresql-9.5)和客户端(postgresql-client-9.5)。

2019年10月03日，已经发布了PostgreSQL 12，如果想安装最新版的，需要更新一下源，参加 [PostgreSQL Apt Repository](https://www.postgresql.org/download/linux/ubuntu/)


可以使用 `psql --version` 来查看当前安装的版本:

```sh
$ psql --version
psql (PostgreSQL) 9.5.19
```

安装后会默认生成一个名为 `postgres`的数据库和一个名为`postgres`的数据库用户。

同时还生成了一个名为 `postgres` 的 Linux 系统用户。

可以使用以下命令查看:

```sh
#查看用户
$ cat /etc/passwd

#查看用户组  
$ cat /etc/group
```

## 3.使用PostgreSQL控制台修改 postgres 数据库用户密码

默认生成的 postgres 的数据库用户没有密码，现在我们使用 postgres Linux用户的身份来登录到管理控制台中。

```sh
# 切换到postgres用户。
$ sudo su - postgres
postgres@iZm5e8p54dk31rre6t96xuZ:~$ 
postgres@iZm5e8p54dk31rre6t96xuZ:~$ whoami
postgres
```

Linux 用户 postgres 以同名的 postgres 数据库用户的身份登录，不用输入密码的。

```sh
postgres@iZm5e8p54dk31rre6t96xuZ:~$ psql
psql (9.5.19)
Type "help" for help.

postgres=#
```

使用 `\password` 命令，为 `postgres` 用户设置一个密码

```sh
postgres=# 
postgres=# CREATE USER db_user WITH PASSWORD 'PWD123456';
CREATE ROLE
postgres=# 
```

创建用户数据库，这里为testdb，并指定所有者为db_user。

```sh
postgres=# CREATE DATABASE testdb OWNER db_user;
CREATE DATABASE
postgres=# 
```

将 testdb 数据库的所有权限都赋予 db_user 数据库用户， 否则 db_user 只能登录控制台，没有数据库操作权限。

```sh
postgres=# GRANT ALL PRIVILEGES ON DATABASE testdb TO db_user;
GRANT
```

使用 `\du` 查看当前的数据库用户:

```sh
postgres=# \du;
               List of roles
Role name |    Attributes                      | Member of 
-----------+------------------------------------------------+-----------
db_user   |                                                       | {}
postgres  | Superuser,Create role,Create DB,Replication,Bypass RLS | {}
```

最后，使用 `\q` 命令退出控制台， 并使用 `exit` 命令退出当前 `db_user` Linux用户。

```sh
postgres=# \q
postgres@iZm5e8p54dk31rre6t96xuZ:~$ 
postgres@iZm5e8p54dk31rre6t96xuZ:~$ exit
logout
```

## 4.数据库基本操作实例

创建数据库与删除数据库: 

```sql
# 创建数据库
postgres=# CREATE DATABASE lusiadas;
CREATE DATABASE

# 删除数据库
postgres=# DROP DATABASE lusiadas;
DROP DATABASE
```

使用 `\c` 切换数据库: 

```sql
postgres=# CREATE DATABASE testdb;
CREATE DATABASE

postgres=# \c testdb;
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
You are now connected to database "testdb" as user "postgres".
```

新建表与删除表: 

```sql
# 创建一个表 tb_test：(两个字段，其中id 为自增ID)
testdb=> CREATE TABLE tb_test(id bigserial, name VARCHAR(20));
CREATE TABLE
# 删除一个表 tb_test
testdb=> DROP table tb_test;
DROP TABLE
```

增删改查操作:

```sql
# 创建一个用户表 tb_users(三个字段，其中id 为自增ID)
testdb=> CREATE TABLE tb_users(id bigserial, age INT DEFAULT 0, name VARCHAR(20));
CREATE TABLE
 
# 使用 INSERT 语句插入数据 
testdb=> INSERT INTO tb_users(name, age) VALUES('张三丰', 212);
INSERT 0 1
testdb=> INSERT INTO tb_users(name, age) VALUES('李四光', 83);
INSERT 0 1
testdb=> INSERT INTO tb_users(name, age) VALUES('王重阳', 58);
INSERT 0 1

# 查询数据
testdb=> select * from tb_users;
 id | age |  name  
----+-----+--------
  1 | 212 | 张三丰
  2 |  83 | 李四光
  3 |  58 | 王重阳
(3 rows)
testdb=> select * from tb_users WHERE id=3;
 id | age |  name  
----+-----+--------
  3 |  58 | 王重阳
(1 row)

# 更新数据 (执行后输出更新的条数，第二次执行失败所以输出为`UPDATE 0`)
testdb=> UPDATE tb_users set name = '全真派王重阳' WHERE name = '王重阳';
UPDATE 1
testdb=> UPDATE tb_users set name = '全真派王重阳' WHERE name = '王重阳';
UPDATE 0

# 插入2条数据
testdb=> INSERT INTO tb_users(name, age) VALUES('赵四', 0);
INSERT 0 1
testdb=> INSERT INTO tb_users(name, age) VALUES('赵五娘', 0);
INSERT 0 1

# 模糊查询
testdb=> SELECT * FROM tb_users WHERE name LIKE '赵%';
 id | age |  name  
----+-----+--------
  4 |   0 | 赵五娘
  5 |   0 | 赵四
(2 rows)

# 修改表结构: 新增字段 
testdb=# ALTER TABLE tb_users ADD email VARCHAR(50);
ALTER TABLE

# 修改表结构: 修改字段 
testdb=# ALTER TABLE tb_users ALTER COLUMN email TYPE VARCHAR(100);
ALTER TABLE

# 删除字段
testdb=# ALTER TABLE tb_users DROP COLUMN email;
ALTER TABLE

# 删除记录
testdb=> DELETE FROM tb_users WHERE id = 5;
DELETE 1
```


使用 pg_database_size() 查看数据库的大小: 

```sql
testdb=# select pg_database_size('testdb');
 pg_database_size 
------------------
          7991967
(1 row)
testdb=# select pg_size_pretty(pg_database_size('testdb'));
 pg_size_pretty 
----------------
 7805 kB
(1 row)
```
 
## 5.PostgreSQL 的 timestamp 类型

查询 current_timestamp

```sql
testdb=# select current_timestamp;
       current_timestamp       
-------------------------------
 2019-11-11 08:33:35.369887+00
(1 row)
```

使用 current_timestamp(0) 定义时间类型精度为0：(有时区)

```sql
testdb=# select current_timestamp(0);
   current_timestamp    
------------------------
 2019-11-11 08:31:08+00
(1 row)
```

使用 current_timestamp(0) 定义时间类型精度为0：(去掉时区) 

```sql
testdb=# select current_timestamp(0)::timestamp without time zone;
  current_timestamp  
---------------------
 2019-11-11 08:31:20
(1 row)

testdb=# select cast (current_timestamp(0) as  timestamp without time zone);
  current_timestamp  
---------------------
 2019-11-11 08:32:26
(1 row)
```

时间戳: 

```sh
testdb=# select extract(epoch from now());
    date_part     
------------------
 1573461495.47821
(1 row)
```

设置数据库时区: 

视图 pg_timezone_names 保存了所有可供选择的时区:

```sh
# 查看时区  
select * from pg_timezone_names;  
```

比如可以选择上海 `Asia/Shanghai` 或重庆 `Asia/Chongqing`， 最简单的直接 `PRC`: 

```sh
testdb=# set time zone 'PRC'; 
SET
testdb=# show time zone;
 TimeZone 
----------
 PRC
(1 row)
testdb=# SELECT LOCALTIMESTAMP(0);
   localtimestamp    
---------------------
 2019-11-11 16:42:54
(1 row)
```

## Reference

https://www.postgresql.org/docs/8.4/sql-altertable.html   
http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html  

[END]