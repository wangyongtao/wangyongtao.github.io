Golang-如何读取和导出CSV文件?

使用Go代码，可以很方便的读取CSV文件或将数据导出成CSV文件。

CSV格式的文件可以在 Office Excel 中打开，也可以很快速的转换成 xls/xlsx 格式的文件。

### 什么是CSV文件呢？

CSV，是Comma Separated Value（逗号分隔值）的英文缩写，通常都是使用英文逗号(",")分隔的纯文本表格文件。

CSV 文件保存任意条数据记录。

CSV 是一种通用的、比较简单的文件格式，被各个领域广泛应用，比如下载数据、导入数据等。

### CSV可以使用什么软件打开?

CSV文件是纯文本的数据文件，可以使用任何文本/代码编辑器打开。

(1) 使用文本编辑器打开： 比如 Sublime Text，VScode, Notepad++ 等。

(2) 使用 Office Excel、Mac Numbers.app、WPS 等软件打开。

### CSV文件的格式？

CSV文件格式： 

(1) 以行为单位，每一行记录一条数据。 
(2) 每一行的数据中，每一列数据以半角逗号（即英文逗号","）作为分隔符，如果列为空也要有逗号分隔。
(3) 列中的内容如存在半角引号（即"）,则替换成半角双引号（""）。就是半角单引号需要使用双引号包起来。 

一个简单的CSV文件内容如下：

```csv
id, name, remark
100, Huck, "Main Character"
101, Jim , "Main Character too"
102, "Tom,Swayer", "This is a boy."
```

### 如何导出CSV文件？

使用 "encoding/csv"

直接看代码:

```c


```

可以看到代码中有个生成BOM头的操作，这是为啥呢？  

如在Windows环境中编写代码，总是选择将代码文件保存成 "无BOM头的UTF8" 格式(比如用Notepad++文本编辑器时)。
但是因为历史原因，Windows下的文本文件需使用 "有BOM头的UTF8" 格式，否则Excel打开就乱码了。  

### 如何读取CSV文件？
 
使用示例:

```php
// 示例：data为导出CSV数据

```

### 用到的 Package csv 接口

Package csv reads and writes comma-separated values (CSV) files.

```c
// 错误
type ParseError
    func (e *ParseError) Error() string
    func (e *ParseError) Unwrap() error

// 读
type Reader
    func NewReader(r io.Reader) *Reader
    func (r *Reader) Read() (record []string, err error)
    func (r *Reader) ReadAll() (records [][]string, err error)

// 写
type Writer
    func NewWriter(w io.Writer) *Writer
    func (w *Writer) Error() error
    // 将缓存区的数据写入底层io.Writer中（或文件中）
    func (w *Writer) Flush()
    // 一次写入一条数据，写入到缓存区。
    func (w *Writer) Write(record []string) error
    // 一次写入多条数据（批量）
    func (w *Writer) WriteAll(records [][]string) error
```

参考链接

https://golang.google.cn/pkg/encoding/csv/

[END]