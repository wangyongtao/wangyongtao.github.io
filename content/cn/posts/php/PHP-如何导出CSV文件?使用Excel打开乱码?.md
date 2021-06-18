---
title: PHP-如何导出CSV文件?使用Excel打开乱码怎么办?
date: 2016-09-08T17:00:00+08:00
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
image: images/common/php-1.jpeg
---

PHP-如何导出CSV文件?使用Excel打开乱码怎么办?

使用PHP代码，可以很方便的将数据导出成CSV格式的文件。

CSV格式的文件可以在 Office Excel 中打开，也可以很快速的转换成 xls/xlsx 格式的文件。

### 什么是CSV文件呢？

CSV，是Comma Separated Value（逗号分隔值）的英文缩写，通常都是使用英文逗号(",")分隔的纯文本表格文件。

### CSV可以使用什么软件打开?

CSV文件是纯文本的数据文件，可以使用任何文本/代码编辑器打开。
也可以使用 MS Office Excel、Mac Numbers.app 等软件打开。

### CSV文件的格式？

CSV文件格式： 
(1) 以行为单位，每一行记录一条数据。 
(2) 每一行的数据中，每一列数据以半角逗号（即英文逗号","）作为分隔符，如果列为空也要有逗号分隔。
(3) 列中的内容如存在半角引号（即"）,则替换成半角双引号（""）。就是半角单引号需要使用双引号包起来。 

### 如何将数据导出成CSV格式？

直接看代码:

```php
function downloadAsCsvFile( array &$csvTitle, array &$csvData, $fileName='' ) {
    if(empty($fileName)){ //保存的文件名称
      $fileName = 'download_'.date('Ymd_His');
    }
    header('Content-type: application/csv');
    header('Content-Transfer-Encoding: binary; charset=utf-8');
    header('Content-Disposition: attachment; filename='.$fileName.'.csv');
    $fp = fopen("php://output", 'w');
    //生成BOM头
    fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));
    //生成标题栏
    fputcsv($fp, $csvTitle);
    foreach ($csvData as $row) {
        fputcsv($fp, $row);
    }
    exit();
}
```

可以看到代码中有个生成BOM头的操作，这是为啥呢？  
我们平常编写代码，总是选择将代码文件保存成 "无BOM头的UTF8" 格式。
但是因为历史原因，Windows下的文本文件需使用 "有BOM头的UTF8" 格式，否则Excel打开就乱码了。  

> FROM [itelite博客]: 
> 为了识别 Unicode 文件，Microsoft 建议所有的 Unicode 文件应该以 ZERO WIDTH NOBREAK SPACE字符开头。
> 这作为一个”特征符”或”字节顺序标记（byte-order mark，BOM）”来识别文件中使用的编码和字节顺序（big-endian或little-endian）。
> 以UTF-8无BOM格式编码，因此要想导出Microsoft Excel可以正常显示的UTF-8的CSV文件，需要显式的输出BOM(EF BB BF)，然后再输出有效数据。


### 如何将数据导出成TXT格式？

同样我们也可以直接下载成TXT格式的文件，使用空格分隔数据。

```php
function downloadAsTxtFile( array &$txtTitle,  array &$txtData, $fileName='' ) {
    if(empty($fileName)){ //保存的文件名称
      $fileName = 'download_'.date('Ymd_His');
    }

    header("Content-Disposition: attachment; filename={$fileName}.txt");
    header("Content-Type: charset=utf-8");
    $fp = fopen("php://output", 'w');

    //生成BOM头
    fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));
    //生成标题栏
    $titleLine = implode("\t", $txtTitle)."\n";
    fputs($fp, $titleLine, strlen($titleLine));

    foreach ($txtData as $row) {
        $line = implode("\t", $row)."\n";
        fputs($fp, $line, strlen($line));
    }
    exit();
}
```

使用示例:

```php
//示例：$data为导出CSV数据
$data = array(
    0 => array('id'=>487, 'date'=>'201605', 'product_id'=>487, 'product_name'=>'法国皇家ROYALCANIN', 'create_time'=>1468931234,),
    1 => array('id'=>13866, 'date'=>'201606', 'product_id'=>13866, 'product_name'=>'BOTH室内挑嘴小型犬奶糕及幼犬500g微生态系列', 'create_time'=>1468810086,),
    2 => array('id'=>20374, 'date'=>'201607', 'product_id'=>20374, 'product_name'=>'Orijen渴望 无谷成犬配方狗粮2.27kg 香港直购', 'create_time'=>1488810010,),
);

$filename = 'TEST_'.date('Ymd').'_'.time();

$title = array('ID', '月份', '产品ID', '产品名称','新建时间');
$data  = array();

//拼装要导出的数据字段
foreach ($data as $key => $row) {
    $data[$key]['id']          = $row['id'];
    $data[$key]['date']        = $row['date'];
    $data[$key]['product_id']  = $row['product_id'];
    $data[$key]['product_name']= $row['product_name'];
    $data[$key]['create_time'] = ($row['create_time']) ? date('Y-m-d H:i:s', $row['create_time']) : 0; 
}
//导出TXT文件
//downloadAsTxtFile($download_title, $data, $filename);

//导出CSV文件
downloadAsCsvFile($title, $data, $filename);
```

### 导出的其他选择

数据导出除了导出CSV格式文件，也可以直接导出成Excel(xls/xlsx)格式文件，这就需要使用PHPExcel库。

(1) 使用 PHPOffice/PHPExcel 类库。（已归档不维护了） 

(2) 使用 PHPOffice/PhpSpreadsheet 类库。（PHPExcel的继承者） 

### 用到的 PHP 函数

> header() : Send a raw HTTP header   
> fopen() :  Opens file or URL   
> fclose() : Closes an open file pointer   
> fputs() :  Alias of fwrite()    
> fwrite() : Binary-safe file write   
> fputcsv() :  Format line as CSV and write to file pointer (PHP 5 >= 5.1.0, PHP 7)    


### 参考链接

http://www.creativyst.com/Doc/Articles/CSV/CSV01.htm#EmbedBRs  
http://www.cnblogs.com/itelite/archive/2012/11/28/2792545.html     
http://file.org/extension/csv  
PHPExcel: https://github.com/PHPOffice/PHPExcel 

### 更新记录

1. 20160729 新整理此文
