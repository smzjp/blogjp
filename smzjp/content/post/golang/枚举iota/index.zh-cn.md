---
original: true
title: 关键字iota
description: golang中的iota
date: 2023-03-20
slug: 
image: 
tags:
categories:
    - Golang
---


### 案例1

```go
const (
	x = iota
	_
	y
	z = "zz"
	k
	p = iota
)
func main() {
  fmt.Println("枚举", x, y, z, k, p)
	//输出： 枚举 0 2 zz zz 5
}

```

这里要注意两个地方

1. iota可以理解为const的行索引或行下标，下标从0开始。
2. 如果某一行没赋值，默认是跟上一行的表达式一样。

### 案例2

```go
const (
	//这个要注意 0左移一位是1
	A1 = 1 << iota
	B1
	C1
	D1
	E1 = iota * iota
	F1
	G1
)
func main() {
  fmt.Println("A1:", A1, "B1:", B1, "C1:", C1, "D1:", D1, "E1:", E1)
  // A1: 1 B1: 2 C1: 4 D1: 8 E1: 16
}
```

这里需要注意一个地方

1. `1 << iota` 相当于 `1 << 0` ，0左移一位是1

### 案例3

```go
const (
	a bool = true
	b      = iota
	c      = iota
	d
)
func main() {
  fmt.Println("a--:", a, "b:", b, "c:", c, "d:", d)
	//a--: 1 b: 1 c: 2 d: 3
}
```

这里需要注意一个地方

1. b的值是1，iota出现在第二行，对应的行下标是1，所以值是1。







