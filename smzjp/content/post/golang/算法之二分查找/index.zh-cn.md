---
original: true
title: 算法之二分查找
description: 边界
date: 2023-11-11
slug: 
image: 
tags:
    - 二分查找
categories:
    - Golang
    - 算法
---



1. 有序数组的元素不重复，查找指定的目标元素

```go
	arr := []int{1, 2, 3, 4, 6, 9}
	target := 2

	left := 0
	right := len(arr) - 1

	for left < right {

		mid := (left + right) / 2

		fmt.Println("mid:", mid)
		if arr[mid] == target {
			right = mid
		} else if arr[mid] < target {
			left = mid + 1
		} else if arr[mid] > target {
			right = mid - 1
		}

	}

	fmt.Println("left:", left)


```

2. 有序数组的元素会重复，查找指定的目标元素的左边界。

```go
	arr := []int{1, 2, 2, 2, 2, 3, 4, 6, 9}
	target := 2

	if len(arr) == 0 {
		fmt.Println("-1")
	}

	left := 0
	right := len(arr)

	for left < right {

		mid := (left + right) / 2

		fmt.Println("mid:", mid)
		if arr[mid] == target {
			right = mid
		} else if arr[mid] < target {
			left = mid + 1
		} else if arr[mid] > target {
			right = mid
		}

	}

	fmt.Println("left:", left)

```

3. 有序数组的元素会重复，查找指定的目标元素的右边界。

```go
arr := []int{1, 1, 1, 1, 2, 2, 2, 3}
	target := 2

	if len(arr) == 0 {
		fmt.Println("-1")
	}

	left := 0
	right := len(arr)

	for left < right {

		mid := (left + right) / 2

		fmt.Println("mid:", mid)
		if arr[mid] == target {
			left = mid + 1
		} else if arr[mid] < target {
			left = mid + 1
		} else if arr[mid] > target {
			right = mid
		}

	}

	fmt.Println("left:", left-1)

```

