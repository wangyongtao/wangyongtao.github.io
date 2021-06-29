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

写入 csv 文件，可以使用 `w.Write()`与`w.Flush()`方法 或 `w.WriteAll()` 方法。


直接看代码:

```c
package main

import (
	"encoding/csv"
	"log"
	"os"
)

func main() {

	// 创建文件
	f, err := os.Create("test.csv")
	if err != nil {
		panic(err)
	}
	defer f.Close()

	// 写入UTF-8 BOM
	f.WriteString("\xEF\xBB\xBF") // 写入UTF-8 BOM

	w := csv.NewWriter(f)
	data := [][]string{
		{"1", "安徽", "合肥,霸都", "0551"},
		{"2", "北京", "北京,帝都", "010"},
		{"3", "浙江", "杭州,电商之都", "0571"},
		{"4", "上海", "上海,魔都", "021"},
	}
	// 使用 w.WriteAll() 写入多条数据
	// 该方法会内部之间调用 w.Flush()，之间写入数据
	w.WriteAll(data)
	// w.Flush()
	if err := w.Error(); err != nil {
		log.Fatalln("=> error writing csv:", err)
	}
}
```

可以看到代码中有个写入 UTF-8 BOM 头的操作，这是为啥呢？  

如在Windows环境中编写代码，总是选择将代码文件保存成 "无BOM头的UTF8" 格式(比如用Notepad++文本编辑器时)。
但是因为历史原因，Windows下的文本文件需使用 "有BOM头的UTF8" 格式，否则Excel打开就乱码了。  

### 如何读取CSV文件？
 
读取 csv 文件，可以使用 `r.Read()` 或 `r.ReadAll()` 方法。

使用示例:

```php
package main

import (
	"encoding/csv"
	"io"
	"log"
	"os"
)

func main() {

	log.Println("-- encoding/csv --")

	// (1)从字符串中读取csv格式数据
	// 	in := `
	// first_name,last_name,username
	// "Rob","Pike",rob
	// Ken,Thompson,ken
	// "Robert","Griesemer","gri"
	// `
	// r := csv.NewReader(strings.NewReader(in))

	// (2)从文件中读取csv格式数据
	// 打开csv文件
	fs, err := os.Open("test.csv")
	if err != nil {
		log.Fatalf("can not open the file, err is %+v", err)
	}
	defer fs.Close()
	r := csv.NewReader(fs)

	// 如果数据不多，可以一次读取所有内容
	log.Println("===> 使用 r.Read() 单行读取: ")
	for {
		record, err := r.Read()
		if err == io.EOF {
			break
		}
		if err != nil {
			log.Fatal(err)
		}

		log.Println("----->", record)
	}

	// 如果数据不多，可以一次读取所有内容
	log.Println("===> 使用 r.ReadAll() 批量读取: ")
	records, err := r.ReadAll()
	if err != nil {
		log.Fatal(err)
	}
	log.Print(records)
}
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

### 参考链接

https://golang.google.cn/pkg/encoding/csv/

[END]