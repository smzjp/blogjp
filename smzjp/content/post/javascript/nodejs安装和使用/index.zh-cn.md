---
original: true
title: nodejs的安装和使用
description: 了解nodejs的项目结构
date: 2022-12-23
slug: 
image: 
tags:
    - node
    - nodejs
    - npm
categories:
    - JavaScript
---

### 概念

1. 浏览器和nodejs
   1. 浏览器，google 浏览器使用的是 v8 javascript的内核
   2. Nodejs是一个非阻塞，事件驱动的javascript运行时环境，nodejs在浏览器之外运行v8 javascript引擎
2. javascript



- **定义协程**

  ```python
  import asyncio
  async def main():
      print('hello')
      # 模拟I/O操作
      await asyncio.sleep(1)
      print('world')
  ```

  协程也叫异步函数，和普通函数相比，在def前面加了个关键字**async**。协程不能直接调用，没有效果，需要加入到事件循环里面。

- **协程的调用**

  ```python
  import asyncio
  import time
  
  async def say_after(delay, what):
      await asyncio.sleep(delay)
      print(what)
  
  async def main():
      print(f"started at {time.strftime('%X')}")
  
      await say_after(1, 'hello')
      await say_after(2, 'world')
  
      print(f"finished at {time.strftime('%X')}")
  
  asyncio.run(main())
  ```

  协程的调用使用 `asyncio.run(main())`,协程的使用一般会定义一个最高层级的入口协程`async def main()`,其他协程由其管理。这是一个串行的协程。

- **协程并行调用**

  ```python
  import asyncio
  import time
  async def main():
      task1 = asyncio.create_task(
          say_after(1, 'hello'))
  
      task2 = asyncio.create_task(
          say_after(2, 'world'))
  
      print(f"started at {time.strftime('%X')}")
  
      await task1
      await task2
  
      print(f"finished at {time.strftime('%X')}")
  ```

  

- 
