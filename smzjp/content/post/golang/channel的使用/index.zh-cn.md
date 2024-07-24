---
original: true
title: channel的使用
description: 使用channel需要注意的事项
date: 2022-12-01
slug: 
image: 
tags:
    - channel
categories:
    - Golang
---

### channel注意事项

1. 在取值之前，如果管道关闭，可以一直取值不会报错，但是如果缓存区空了，会取到对应类型的零值。可以通过ok语句判断，值是否是缓冲区的还是零值。

   ```go
   //例1
   func main() {
     ch := make(chan int, 2)
   	ch <- 2
   	ch <- 0
   	close(ch)
     
     if v, ok := <-ch; ok {
       fmt.Println("管道值:", v)
   	}
     if v, ok := <-ch; ok {
       fmt.Println("管道值:", v)
   	}
     if v, ok := <-ch; ok {
       fmt.Println("管道值:", v)
   	}
     //打印结果
     //管道值: 2
   	//管道值: 0
     
   }
   ```

   或者也可以通过for语句取值，会把缓存区的值全部取出来，不需要ok判断。

   ```go
   //例子2
   func main() {
     ch := make(chan int, 2)
   	ch <- 2
   	ch <- 0
   	close(ch)
     
     for v := range ch {
       fmt.Println("通过range取值管道:", v)
   	}
     //打印结果
     //通过range取值管道: 2
   	//通过range取值管道: 0
     
   }
   
   ```

   对一个已关闭的channel，获取值不会阻塞，会打印相应的零值，可以通过ok语句来判断是否channel已关闭。比如下面的例子，会一直打印`a: 0 false`。因为协程里面有个for循环一直在取channel的值，直到`time.Sleep(time.Second * 1)`到时间。

   ```go
   //例子3
   func main() {
     ch := make(chan int)
   	go func() {
   		for {
   			a, ok := <-ch
   			fmt.Println("a:", a, ok)
   		}
   	}()
   	close(ch)
   	fmt.Println("关闭")
   	time.Sleep(time.Second * 1)
   }
   //打印结果
   //关闭
   //a: 0 false
   //.....
   ```

2. 管道没关闭，缓冲区如果满了，就不能再给管道发送值，否则会报错。必须取值后，再发送。

   ```go
   //例子1
   func main() {
     ch04 := make(chan int, 2)
   	ch04 <- 3
   	ch04 <- 4
     //这里在主线程阻塞了，又检测到没有其他的协程有接收此channel的值，所以死锁了。
   	ch04 <- 5
     //报错
     //fatal error: all goroutines are asleep - deadlock!
   }
   ```

   缓存如果满了，发送值的操作`ch <- count`会阻塞，直到有接收值的操作把值接收，继续执行下一次循环。这里阻塞因为是阻塞的协程，所以不会死锁。

   ```go
   func main() {
     ch := make(chan int, 1)
   	go func() {
   		count := 1
   		for {
   			fmt.Println("for循环前")
   			ch <- count
   			count++
   			fmt.Println("for循环后")
   		}
   	}()
   	fmt.Println(<-ch)
   	fmt.Println(<-ch)
   }
   
   //打印结果
   /*
   for循环前
   for循环后
   for循环前
   for循环后
   for循环前
   1
   2
   for循环后
   for循环前
   
   */
   ```

   

3. 管道没关闭，缓存区里面如果为空，就不能取值了，否则会报错。

   ```go
   //例子1
   func main() {
     ch02 := make(chan int, 2)
   	ch02 <- 2
   	ch02 <- 0
   	fmt.Println("管道02", <-ch02)
   	fmt.Println("管道02", <-ch02)
     //这里在主线程阻塞了，又检测到没有其他的协程有发送值，所以死锁了。
   	fmt.Println("管道02", <-ch02)
     //报错
     //fatal error: all goroutines are asleep - deadlock!
   }
   ```

   对于有缓存的channel，取值的时候会把缓冲的值取出来，如果缓存中没值了`a, ok := <-ch`会阻塞，直到有新的值发送给channel。下面的例子中，有个for循环会一直执行`a, ok := <-ch`,因为前五次都是有缓存值的，所以没阻塞，可以正常执行for循环及里面的代码。当第6次执行的时候，阻塞在了for循环中的`a, ok := <-ch`语句，当然这时候for循环也执行不了了。

   ```go
   //例子2
   func main() {
     ch := make(chan int, 5)
   	ch <- 1
   	ch <- 2
   	ch <- 3
   	ch <- 4
   	ch <- 5
   	go func() {
   		for {
   			fmt.Println("for循环")
   			a, ok := <-ch
   			fmt.Println("a:", a, ok)
   		}
   	}()
   	time.Sleep(time.Second * 4)
   }
   
   ```

4. 会引起panic的三种情况

   ```go
   //情况1 对nil执行关闭操作
   	ch := make(chan int, 1)
   	ch = nil
   	go func() {
   		close(ch)
   	}()
   	time.Sleep(time.Second * 4)
   //情况2 对已经关闭的channel再次执行close操作
   	ch := make(chan int, 1)
   	go func() {
   		close(ch)
       //再次执行close
   		close(ch)
   	}()
   	time.Sleep(time.Second * 4)
   
   //情况3 对已经关闭的channel执行发送操作
   	ch := make(chan int, 1)
   	go func() {
   		close(ch)
   		ch <- 2
   	}()
   	time.Sleep(time.Second * 4)
   
   
   ```

   
