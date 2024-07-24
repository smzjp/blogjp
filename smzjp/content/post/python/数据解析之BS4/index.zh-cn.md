---
original: true
title: 数据解析之BS4
description: Beautiful Soup的常用语法
date: 2022-08-21
slug: 
image: 
tags:
    - Beautiful Soup
    - bs4
categories:
    - Python
---

Beautiful Soup是python的一个库，最主要的功能是从网页抓取数据。

#### 安装

```python
# pip3 install bs4 安装
 
from bs4 import BeautifulSoup
```

Beautiful Soup支持Python标准库中的HTML解析器,还支持一些第三方的解析器，如果我们不安装它，则 Python 会使用 Python默认的解析器。

lxml 解析器更加强大，速度更快，推荐安装。

```python
# 先下载 pip3 install lxml
# 使用
BeautifulSoup(markup,'lxml')
```

另一个可供选择的解析器是纯Python实现的 html5lib , html5lib的解析方式与浏览器相同,可以选择下列方法来安装html5lib:

```python
# 先下载pip3 install html5lib
# 使用
BeautifulSoup(markup,'html5lib')
```

#### 初始化

```python
from bs4 import BeautifulSoup

# 第一个参数markup 是被解析的html字符串或者文件内容
# 第二个参数features 是解析器类型
soup = BeautifulSoup(markup,features)

```

#### 四种对象

```python
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc, 'html.parser')
```

Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种`BeautifulSoup`， `Tag` , `NavigableString` ,   `Comment`

##### Tag对象

是`bs4.element.Tag`类型，对应html中的标签

```python
print(soup.head, type(soup.head))
print(soup.title, type(soup.title))
# 第一个a标签，如果想获取所有a标签要用到soup.find_all('a')
print(soup.a, type(soup.a))  
print(soup.p.b, type(soup.p.b))
print(soup.b, type(soup.b))

# 结果
# <head><title>The Dormouse's story</title></head> <class 'bs4.element.Tag'>
# <title>The Dormouse's story</title> <class 'bs4.element.Tag'>
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a> <class 'bs4.element.Tag'>
# <b>The Dormouse's story</b> <class 'bs4.element.Tag'>
# <b>The Dormouse's story</b> <class 'bs4.element.Tag'>
```

1. 查找tag对象的属性和标签名

```python
print(soup.a.name)
print(soup.p.b.name)
print(soup.a["href"])
print(soup.a["class"])
print(soup.a.attrs)

# 结果
# a
# b
# http://example.com/elsie
# ['sister']
# {'href': 'http://example.com/elsie', 'class': ['sister'], 'id': 'link1'}

```

2. 获取标签对象的文本内容

文本内容是一个`NavigableString`对象

```python
# 第一个找到的p下的文本只有一个时，取到，否则为None
# 如果 <p class="title"><b>The Dormouse's story</b><b>The Dormouse's story2</b></p>是这样，就需要用strings的才可以获取到文本
print(soup.p.string, type(soup.p.string))
# 拿到一个生成器对象, 取到p下所有的文本内容
print(soup.p.strings) 
for i in soup.p.strings:
    print(i)
    
    
# 结果
# The Dormouse's story <class 'bs4.element.NavigableString'>
# <generator object Tag._all_strings at 0x1051dc240>
# The Dormouse's story	
```

3. text和string的区别

```python
#如果内容是 <p class="title"><b>The Dormouse's story</b><b>The Dormouse's story2</b></p>这样时

print(soup.p.string)
# 取到p下所有的文本内容,会把内容合并成一个字符串返回
# 一般text用的比较多,并且它可以直接过滤掉注释
print(soup.p.text)

# 结果
# None
# The Dormouse's storyThe Dormouse's story2



```

##### Comment对象

如果标签内容是注释的时候，会把输出注释的内容，不会输出注释的符号，其对象是`bs4.element.Comment`

```python
markup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
soup = BeautifulSoup(markup,"html.parser")
comment = soup.b.string
print(comment, type(comment))
print(soup.b.text)

# 结果
# Hey, buddy. Want to buy a used parser? <class 'bs4.element.Comment'>

# 从这里也可以看出string和text的区别，text会过滤掉注释，不会输出
```

#### 遍历方法

```python
# 1. 嵌套选择
print(soup.head.title.text)
print(soup.body.a.text)
# 结果
# The Dormouse's story
# Elsie

# 2. 子节点
# p下所有子节点
print(soup.p.contents)
# 得到一个迭代器,包含p下所有子节点
print(soup.p.children)
for i, child in enumerate(soup.p.children, 1):
    print(i, child)
# 结果
# [<b>The Dormouse's story</b>]
# <list_iterator object at 0x100e684c0>
# 1 <b>The Dormouse's story</b>

# 3. 子孙节点
# 获取子孙节点,p下所有的标签都会选择出来
print(soup.p.descendants)
for i, child in enumerate(soup.p.descendants, 1):
    print(i, child)
# 结果
# 1 <b>The Dormouse's story</b>
# 2 The Dormouse's story

#4. 父节点、祖先节点
# 获取a标签的父节点
print(soup.a.parent)
# 找到a标签所有的祖先节点，父亲的父亲，父亲的父亲的父亲...
print(soup.a.parents)
#结果
# <p class="story">Once upon a time there were three little sisters; and their names were
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
# and they lived at the bottom of a well.</p>
# <generator object PageElement.parents at 0x1037a8700>

# 5. 兄弟节点
# 下一个兄弟,类型：<class 'bs4.element.NavigableString'>
print(soup.a.next_sibling)
#结果
# ,

print(soup.a.next_sibling.next_sibling)
# 结果
# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>

# 上一个兄弟
print(soup.a.previous_sibling)
# 结果
# Once upon a time there were three little sisters; and their names were
```

#### 搜索方法

##### find_all

```python
find_all( name , attrs , recursive , string , **kwargs )
```

* **name参数**

表示四种过滤器

```python
soup = BeautifulSoup(html_doc, 'lxml')

# 一、 name 四种过滤器: 字符串、正则表达式、列表、方法
# 1、字符串：即标签名
print(soup.find_all(name='a'))

# 2、正则表达式

print(soup.find_all(name=re.compile('^b')))  # 找出b开头的标签，结果有body和b标签

# 3、列表：如果传入列表参数,Beautiful Soup会将与列表中任一元素匹配的内容返回.下面代码找到文档中所有<a>标签和<b>标签:
print(soup.find_all(name=['a', 'b']))


# 4、方法:如果没有合适过滤器,那么还可以定义一个方法,方法只接受一个元素参数 ,如果这个方法返回 True 表示当前元素匹配并且被找到,如果不是则反回 False
def has_class_and_id(tag):
    return tag.has_attr('class') and tag.has_attr('id')


print(soup.find_all(name=has_class_and_id))
```

* **keyword 参数**

根据指定的标签属性搜索，如果一个指定名字的参数不是搜索内置的参数名,搜索时会把该参数当作指定名字tag的`属性`来搜索。

```python
print(soup.find_all(href="http://example.com/tillie"))
print(soup.find_all(href=re.compile("^http://")))
print(soup.find_all(id=True)) # 拥有id属性的tag
# 多个属性 且的关系，都要满足
print(soup.find_all(href=re.compile("http://"), id='link1')
# 注意，class是Python的关键字，所以class属性用class_
print(soup.find_all("a", class_="sister"))
# 或者也可以这样
print(soup.find_all("a", attrs={"class":"sister"}))
print(soup.find_all("a",attrs={"href": re.compile("^http://"), "id": re.compile("^link[12]")}))      
# 通过 find_all() 方法的 attrs 参数定义一个字典参数来搜索包含特殊属性的tag:
data_soup.find_all(attrs={"data-foo": "value"})    
```

* **string参数**

根据标签的文本内容搜索

```python
import re

print(soup.find_all(string="Elsie"))
# ['Elsie']

print(soup.find_all(string=["Tillie", "Elsie", "Lacie"]))
# ['Elsie', 'Lacie', 'Tillie']

# 只要包含Dormouse就可以
print(soup.find_all(string=re.compile("Dormouse")))
# ["The Dormouse's story", "The Dormouse's story"]
```

* **limit参数**

`find_all()` 方法返回全部的搜索结构,如果文档树很大那么搜索会很慢.如果我们不需要全部结果,可以使用 `limit` 参数限制返回结果的数量.效果与SQL中的limit关键字类似,当搜索到的结果数量达到 `limit` 的限制时,就停止搜索返回结果.

````python
print(soup.find_all("a",limit=2))
````

* **recursive 参数**

调用tag的 `find_all()` 方法时,Beautiful Soup会检索当前tag的所有子孙节点,如果只想搜索tag的直接子节点,可以使用参数 `recursive=False` 

##### find

```python
find( name , attrs , recursive , string , **kwargs )
```

`find()`等价于 `find_all()` 方法并设置 `limit=1` 参数

#### CSS选择器

##### select

css选择器的方法为select

```python
soup.select("title")
# [<title>The Dormouse's story</title>]

soup.select("p nth-of-type(3)")
# [<p class="story">...</p>]

soup.select("body a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("html head title")
# [<title>The Dormouse's story</title>]


soup.select("head > title")
# [<title>The Dormouse's story</title>]

soup.select("p > a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("p > a:nth-of-type(2)")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

soup.select("p > #link1")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select("body > a")

soup.select("#link1 ~ .sister")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie"  id="link3">Tillie</a>]

soup.select("#link1 + .sister")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]


soup.select(".sister")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("[class~=sister]")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("#link1")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select("a#link2")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]


soup.select("#link1,#link2")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]


soup.select('a[href]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href="http://example.com/elsie"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select('a[href^="http://example.com/"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href$="tillie"]')
# [<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href*=".com/el"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
multilingual_markup = """
 <p lang="en">Hello</p>
 <p lang="en-us">Howdy, y'all</p>
 <p lang="en-gb">Pip-pip, old fruit</p>
 <p lang="fr">Bonjour mes amis</p>
"""
multilingual_soup = BeautifulSoup(multilingual_markup)
multilingual_soup.select('p[lang|=en]')
# [<p lang="en">Hello</p>,
#  <p lang="en-us">Howdy, y'all</p>,
#  <p lang="en-gb">Pip-pip, old fruit</p>]
```

##### select_one

返回查找到的元素的第一个

