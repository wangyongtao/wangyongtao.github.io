---
title: MySQL-多表update-SQL语句总结
date: 2014-10-11T17:00:00+08:00
# description: Sample article showcasing basic Markdown syntax and formatting for HTML elements.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: WYT
authorEmoji: 🧑
tags:
- MySQL
categories:
- MySQL
- QuickStart
series:
- MySQL Guide
image: images/common/mysql-1.jpeg
---

MySQL-多表update-SQL语句总结

假定我们有两张表:
一张表为Product表存放产品信息，其中有产品价格列Price；
另外一张表是ProductPrice表，我们要将ProductPrice表中的价格字段Price更新为Price表中价格字段的80%。

在 MySQL 中我们有几种手段可以做到这一点，一种是 update table1 t1, table2 ts …的方式：

```sql
UPDATE product p, productPrice pp
 SET pp.price = pp.price * 0.8
 WHERE p.productId = pp.productId AND p.dateCreated < ‘2004-01-01’
```

另外一种方法是使用inner join然后更新：

```sql
UPDATE product p
 INNER JOIN productPrice pp ON p.productId = pp.productId
 SET pp.price = pp.price * 0.8
 WHERE p.dateCreated < ‘2004-01-01’
```

另外我们也可以使用left outer join来做多表update，比方说如果ProductPrice表中没有产品价格记录的话，将Product表的isDeleted字段置为1，如下sql语句：

```sql
UPDATE product p
 LEFT JOIN productPrice pp ON p.productId = pp.productId
 SET p.deleted = 1
 WHERE pp.productId IS null
```


另外，上面的几个例子都是两张表之间做关联，但是只更新一张表中的记录，其实是可以同时更新两张表的，如下sql：

```sql
UPDATE product p
INNER JOIN productPrice pp ON p.productId = pp.productId
SET pp.price = pp.price * 0.8, p.dateUpdate = CURDATE()
WHERE p.dateCreated < ‘2004-01-01’
```

两张表做关联，更新了ProductPrice表的price字段和Product表字段的dateUpdate两个字段。

[END]