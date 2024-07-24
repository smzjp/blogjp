---
original: true
title: golang语法
description: 基本语法
date: 2022-12-20
slug: 
image: 
tags:
    -
categories:
    - Golang
---

这篇文章主要整理一些在做算法的过程中发现的一些需要注意的语法点。记录下来加深理解，查看。

##### 1.切片初始化

切片初始化一般有两种方式，根据是否需要初始值。

```go
	//方式1
	var arr01 []int
	fmt.Println("arr01:", arr01)

	//方式02
	arr02 := make([]int, 3)
	fmt.Println("arr02:", arr02)
	//方式03
	arr03 := []int{1, 2, 3}
	fmt.Println("arr03:", arr03)

//结果
arr01: []
arr02: [0 0 0]
arr03: [1 2 3]
```





