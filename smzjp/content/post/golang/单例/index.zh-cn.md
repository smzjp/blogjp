---
original: true
title: 单例模式
description: golang中的单例
date: 2023-03-20
slug: 
image: 
categories:
    - Golang
---

​	单例有两种模式，一种是饿汉模式，另一种是懒汉模式

### 饿汉模式

饿汉模式，在调用之前初始化，所以一般在init中初始化。

```go
type singleton struct {
	count int
}

var Instance = new(singleton)

func (s *singleton) Add() int {
	s.count++
	return s.count
}

func main() {
  //使用，go一般使用饿汉模式
	fmt.Println(Instance.Add())
}
```



### 懒汉模式

懒汉模式，首次调用的时候初始化，使用懒加载的方式初始化。

```go
type singleton02 struct {
	count int
}

var (
	instance *singleton02
	mutext   sync.Mutex
)

func New() *singleton02 {

	if instance == nil {
    //需要加锁，保证线程安全
		mutext.Lock()
		if instance == nil {
			instance = new(singleton02)
		}
		mutext.Unlock()
	}

	return instance

}

func (s *singleton02) Add() int {
	s.count++
	return s.count
}
func main() {
  //使用的时候创建
	c2 := New()
	fmt.Println(c2.Add())
}

```







