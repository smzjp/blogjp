---
original: true
title: 数据解析之xpath
description: xpath的常用语法
date: 2022-08-22
slug: 
image: 
tags:
    - xpath
categories:
    - Python
---

xpath 全称为**XML Path Language** 一种小型的查询语言，其优点是可在XML中查找信息 ，支持HTML的查找，通过元素和属性进行导航。

#### 安装

由于XPath属于lxml库模块，所以首先要安装库lxml。

```python
# pip3 install lxml 安装
```

#### 初始化

```python
from lxml import etree
selector=etree.HTML(源码) #将源码转化为能被XPath匹配的格式
selector.xpath(表达式) #返回为一列表
```

#### 路径表达式

| 表达式 | 描述                                                     | 实例           | 解析                                |
| :----- | :------------------------------------------------------- | -------------- | ----------------------------------- |
| /      | 从根节点选取                                             | `/body/div[1]` | 选取根结点下的body下的第一个div标签 |
| //     | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置 | `//a`          | 选取文档中所有的a标签               |
| ./     | 当前节点再次进行xpath                                    | `./a`          | 选取当前节点下的所有a标签           |
| @      | 选取属性                                                 | `//@calss`     | 选取所有的class属性                 |

#### 谓语（Predicates）

谓语用来查找某个特定的节点或者包含某个指定的值的节点。

**谓语被嵌在方括号中**。

在下面的表格中，我们列出了带有谓语的一些路径表达式，以及表达式的结果：

| 路径表达式                    | 结果                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| /ul/li[1]                     | 选取属于 ul子元素的第一个 li元素。                           |
| /ul/li[last()]                | 选取属于 ul子元素的最后一个 li元素。                         |
| /ul/li[last()-1]              | 选取属于 ul子元素的倒数第二个 li元素。                       |
| //ul/li[position()<3]         | 选取最前面的两个属于 ul元素的子元素的 li元素。               |
| //a[@title]                   | 选取所有拥有名为 title的属性的 a元素。                       |
| //a[@title='xx']              | 选取所有 a元素，且这些元素拥有值为 xx的 title属性。          |
| //a[@title>10] `> < >= <= !=` | 选取 a元素的所有 title元素，且其中的 title元素的值须大于 10。 |
| /body/div[@price>35.00]       | 选取body下price元素值大于35的div节点                         |
| //dd[text()="原创"]           | 选取文本是原创的dd标签                                       |

#### 选取未知节点

XPath 通配符可用来选取未知的 XML 元素。

| 通配符 | 描述                 |
| :----- | :------------------- |
| *      | 匹配任何元素节点。   |
| @*     | 匹配任何属性节点。   |
| node() | 匹配任何类型的节点。 |

**实例**

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

| 路径表达式  | 结果                              |
| :---------- | :-------------------------------- |
| /ul/*       | 选取 bookstore 元素的所有子元素。 |
| //*         | 选取文档中的所有元素。            |
| //title[@*] | 选取所有带有属性的 title 元素。   |
| //node()    | 获取所有节点                      |

#### 选取若干路径

通过在路径表达式中使用“|”运算符，您可以选取若干个路径。

**实例**

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

| 路径表达式                       | 结果                                                         |
| :------------------------------- | :----------------------------------------------------------- |
| //book/title \| //book/price     | 选取 book 元素的所有 title 和 price 元素。                   |
| //title \| //price               | 选取文档中的所有 title 和 price 元素。                       |
| /bookstore/book/title \| //price | 选取属于 bookstore 元素的 book 元素的所有 title 元素，以及文档中所有的 price 元素。 |

- 逻辑运算

  ```python
  //div[@id="head" and @class="s_down"] # 查找所有id属性等于head并且class属性等于s_down的div标签
  //title | //price # 选取文档中的所有 title 和 price 元素,“|”两边必须是完整的xpath路径
  ```

- 属性查询

  ```python
  //div[@id] # 找所有包含id属性的div节点
  //div[@id="maincontent"]  # 查找所有id属性等于maincontent的div标签
  //@class
  //li[@name="xx"]//text()  # 获取li标签name为xx的里面的文本内容
  ```

- 获取第几个标签 索引从1开始

  ```python
  tree.xpath('//li[1]/a/text()')  # 获取第一个
  tree.xpath('//li[last()]/a/text()')  # 获取最后一个
  tree.xpath('//li[last()-1]/a/text()')  # 获取倒数第二个
  ```

- 模糊查询

  ```python
  //div[contains(@id, "he")]  # 查询所有id属性中包含he的div标签
  //div[starts-with(@id, "he")] # 查询所有id属性中包以he开头的div标签
  //div/h1/text()  # 查找所有div标签下的直接子节点h1的内容
  //div/a/@href   # 获取a里面的href属性值 
  //*  #获取所有
  //*[@class="xx"]  #获取所有class为xx的标签
  
  # 获取节点内容转换成字符串
  c = tree.xpath('//li/a')[0]
  result=etree.tostring(c, encoding='utf-8')
  print(result.decode('UTF-8'))
  ```

#### 案例

```python
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body><p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
<a href="http://example.com/tillie" class="sister">Bbcda</a>
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

from lxml import etree
tree = etree.HTML(html_doc)
# 选取文档中所有的a标签
print(tree.xpath('//a'))
# 获取含有id属性的a标签
print(tree.xpath("//a[@id]"))
print(tree.xpath("//a[@id='link3']"))
print(tree.xpath("//a[text()='Tillie']"))
# 从根结点开始选择
print(tree.xpath("/html/body/p"))
print(tree.xpath("/html/body/p[3]/text()"))

```

