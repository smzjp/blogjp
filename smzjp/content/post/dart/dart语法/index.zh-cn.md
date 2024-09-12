---
original: true
title: dart语法
description: 基本语法
date: 2024-08-06
slug: 
image: 
tags:
    -
categories:
    - Dart
---



##### 1. 数组的初始化

 ```swift
 //方式1 创建空数组
 //两种都可以看习惯
 var arr01: [[Int]] = []
 print("arr01:\(arr01)")
 //
 var arr02 = [[Int]]()
 print("arr02:\(arr02)")
 
 
 //方式2 创建包含初始值的数组
 //初始值都一样的情况下，可以用这个方法创建
 var arr03 = Array(repeating: [], count: 3)
 print("arr03:\(arr03)")
 //指定初始值
 var arr04 = [1,2,3]
 print("arr04:\(arr04)")
 
 //结果
 arr01:[]
 arr02:[]
 arr03:[[], [], []]
 arr04:[1, 2, 3]
 ```





