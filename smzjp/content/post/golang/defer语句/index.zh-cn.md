---
original: true
title: 关键字defer
description: defer延迟函数
date: 2022-12-20
slug: 
image: 
tags:
    - defer
categories:
    - Golang
---

defer延迟函数,在函数或方法执行完成的最后调用，defer函数可以有多个。defer函数执行过程可以分两部分，多个defer函数先按照先后顺序入栈，最后执行的时候从栈中取出执行，多个defer的执行顺序是先入后出的。

### 案例1

```go
func f1() int {
	var i int
	defer func() {
		i++
	}()
	return i
}

func main() {
	fmt.Println(f1())
  //执行结果
  0
}

```

匿名返回值的时候，执行 `return i`的时候会重新创建一个局部变量，用来给返回值赋值，类似 `result:=i` ,这个result就是最终的返回值，因为defer是在return后面执行，所以在defer里面的`i++`是在赋值之后的语句，不会影响返回值。

### 案例2

```go
func f2() (r int) {

	defer func() {

		r++

	}()
	return r

}
func main() {
	fmt.Println(f2())
  //执行结果
  1
}

```

返回值不是匿名的，不需要赋值。所以defer里面修改值会影响到最终的返回值。

### 案例3

```go
func f3() (r int) {
	defer func(a int) {
		fmt.Println("r:", r)
		r *= a
		fmt.Println("a:", a)
	}(r)
	return 2
}
func main() {
	fmt.Println(f2())
  //执行结果
  r: 2
	a: 0
	0
}
```

执行步骤

1. defer先入栈，defer函数带有参数，入栈的时候入参的值也会一起带入，r还没赋值，所以是零值0，defer的入参是0。
2. 执行 `return 2`,给r赋值2。
3. defer出栈，执行defer函数体，`r *= a` ,给r赋值0。所以最终结果是0。

