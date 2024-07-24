---
original: true
title: css选择器
description: css选择器主要语法
date: 2023-01-22
slug: 
image: 
tags:
    - css
categories:
    - JavaScript
    - Html
---

### 通配选择器

#### 语法

`*{样式声明}`

#### 例子

```css
/*所有的元素都会匹配到，会生效*/
*{color:red;}
```

通配选择器一般不建议使用，性能最低，会遍历所有元素。

### 元素选择器

#### 语法

`元素 {样式声明}`

#### 例子

```css
/*匹配所有的span元素*/
span {
		background-color: skyblue;
}
```

### 类选择器

#### 语法

`.类名 { 样式声明 }`

#### 例子

```css
/* 匹配所有 class="spacious" 的元素 */
.spacious {
  margin: 2em;
}

/* class可以有多个名称 匹配所有class即=“spacious”又=“elegant”的li元素*/
/*首先需要是li元素，其次class要=“spacious”和=“elegant”*/
/*元素选择器和类选择器组合*/
li.spacious.elegant {
  margin: 2em;
}
```

### ID选择器

#### 语法

`#id名 { 样式声明 }`

#### 例子

```css
/* 匹配所有id=“identified”的元素 */
#identified {
  background-color: skyblue;
}
```

### 属性选择器

#### 语法

1. `[属性名]`
2. `[属性名=值]`















