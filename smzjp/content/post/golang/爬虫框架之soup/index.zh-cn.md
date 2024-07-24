---
original: true
title: 爬虫框架之soup
description: soup的使用方法
date: 2023-04-10
slug: 
image: 
tags:
    - 
categories:
    - Golang
    - 爬虫
---

soup是用来解析，提取html数据的库，它是基于html解析库golang.org/x/net/html。类似于python中的BeautifulSoup库，这个库比较小，总共只有500多行代码。

下面是其所提供的所有方法

```go
var Headers map[string]string // Set headers as a map of key-value pairs, an alternative to calling Header() individually
var Cookies map[string]string // Set cookies as a map of key-value  pairs, an alternative to calling Cookie() individually
func Get(string) (string,error) {} // Takes the url as an argument, returns HTML string
func GetWithClient(string, *http.Client) {} // Takes the url and a custom HTTP client as arguments, returns HTML string
func Post(string, string, interface{}) (string, error) {} // Takes the url, bodyType, and payload as an argument, returns HTML string
func PostForm(string, url.Values) {} // Takes the url and body. bodyType is set to "application/x-www-form-urlencoded"
func Header(string, string) {} // Takes key,value pair to set as headers for the HTTP request made in Get()
func Cookie(string, string) {} // Takes key, value pair to set as cookies to be sent with the HTTP request in Get()
func HTMLParse(string) Root {} // Takes the HTML string as an argument, returns a pointer to the DOM constructed
func Find([]string) Root {} // Element tag,(attribute key-value pair) as argument, pointer to first occurence returned
func FindAll([]string) []Root {} // Same as Find(), but pointers to all occurrences returned
func FindStrict([]string) Root {} //  Element tag,(attribute key-value pair) as argument, pointer to first occurence returned with exact matching values
func FindAllStrict([]string) []Root {} // Same as FindStrict(), but pointers to all occurrences returned
func FindNextSibling() Root {} // Pointer to the next sibling of the Element in the DOM returned
func FindNextElementSibling() Root {} // Pointer to the next element sibling of the Element in the DOM returned
func FindPrevSibling() Root {} // Pointer to the previous sibling of the Element in the DOM returned
func FindPrevElementSibling() Root {} // Pointer to the previous element sibling of the Element in the DOM returned
func Children() []Root {} // Find all direct children of this DOM element
func Attrs() map[string]string {} // Map returned with all the attributes of the Element as lookup to their respective values
func Text() string {} // Full text inside a non-nested tag returned, first half returned in a nested one
func FullText() string {} // Full text inside a nested/non-nested tag returned
func SetDebug(bool) {} // Sets the debug mode to true or false; false by default
func HTML() {} // HTML returns the HTML code for the specific element
```

### 安装

`go get -u github.com/anaskhan96/soup`

### 使用

```go
package main

import (
	"fmt"
	"github.com/anaskhan96/soup"
	"log"
	"strings"
)

func main() {

	url := "https://movie.douban.com/top250?start=25"
	soup.Headers = map[string]string{
		"User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36",
	}
	//下载
	source, err := soup.Get(url)
	if err != nil {
		log.Fatal(err)
	}
	//解析
	doc := soup.HTMLParse(source)
	for _, root := range doc.Find("ol", "class", "grid_view").FindAll("div", "class", "hd") {
		movieUrl, _ := root.Find("a").Attrs()["href"]
		title := root.Find("span", "class", "title").Text()
		fmt.Println(strings.Split(movieUrl, "/")[4], title)
	}

}
```

#### 定位标签

1. `Find("ol", "class", "grid_view")` 是找符合条件的第一个标签，第一个参数表示你要找的标签名，第二个参数表示属性，第三个参数表示属性对应的值。如果要找的标签比较深， `Find`函数可以链式调用。其返回值也是`Root`类，跟`soup.HTMLParse(source)`的返回值一样。可以发现soup查找标签没有用到css选择器，而是自己定义的规则。
2. `FindAll("div", "class", "hd")` 是找到符合条件的所有标签，返回的是一个数组[]Root。

#### 查找属性

1. 如果要查找标签的属性可以使用 `root.Attrs()["href"]` 方法，`Attrs()`是类`Root`的方法。

#### 查找内容

1. 如果要查找标签的内容可以使用`root.Find("span", "class", "title").Text()` 方法，`Text()`是类`Root`的方法。

