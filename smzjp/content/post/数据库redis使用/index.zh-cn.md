---
original: true
title: redis使用
description: redis常用方法及其在golang和python中的使用
date: 2022-10-20
slug: 
image:
tags:
    - redis
    - publish
    - subscribe
categories:
    - Golang
    - Python
---

redis：remote dictionary server 远程字典服务

### 安装

在mac下，通过homebrew安装，`brew install redis`

### 运行

redis也是c/s架构的，需要先启动redis服务器。

#### redis服务器

1. 启动方式
   1. 通过brew启动 `brew services start redis`,这是后台启动
   2. 直接在终端执行 `redis-server`，这个是前台启动
2. 关闭服务
   1. 如果是通过前台启动的，直接关闭终端就可以。
   2. 如果是通过brew启动的 `brew services stop redis`
   3. 也可以通过 客户端向服务端发送命令关闭 `redis-cli shutdown`
3. 检查redis相关进程是否在运行 可以通过 命令 `ps axu | grep redis`

#### redis客户端

必须先开启服务端服务，客户端才能正常工作。

##### 命令行终端

1. 启动方式
   1. `redis-cli`
   2. `redis-cli -h 127.0.0.1 -p 6379`
   3. `redis-cli -h 127.0.0.1 -p 6379 -a password` 
2. 退出方式
   1. 直接 输入`exit` 就可以。



##### 可视化工具

[RedisDesktopManager](https://github.com/RedisInsight/RedisDesktopManager)



### 语法

#### set操作

是字符串相关的操作

1. `set key value` 设置key的值为value
   1. `get key` 获取指定key的值
2. `setex key value second` 设置key的值为value,并在second秒后过期，在second秒后，`get key` 会为空
   1. `psetex key milliseconds value` 设置key的值为value,并在milliseconds秒后过期，单位时毫秒
3. `getset key value` 设置key的值为value，并返回key的旧值
4. `mset key1 value1 [key2 value2 ...]` 一次设置多个key，value值
   1. `mget key1 [key2 ...]` 一次获取多个key的值
5. `setnx key value` 只有在key不存在时，才会给 key设置value

#### list操作

是列表相关的操作，key对应存的是list。

1. `lpush key value1 [value2 ...]` 把vlue值添加到key对应的列表中，添加到列表的头部
   1. `rpush key value1 [value2 ...]` 把vlue值添加到key对应的列表中，添加到列表的尾部
   2. `lpushx key value1 [value2 ...]` 为已存在的key添加值，如果key不存在添加不成功，可以防止不小心重写创建一个key。
2. `lrange key start stop` 查看 key 对应的list的里面的值，必须要指定查看的list的范围
   1. 如果要查看所有的list数据 可以用 `lrange key 0 -1`
3. `llen key ` 查看list的长度
4. `linsert key before|after pivot value` 在key对应的列表里面的pivot值的前面或者后面插入值value。如果列表里面有多个pivot值，只会对列表最前面的生效。
5. `lpop key` 移除列表的第一个元素，并且返回被移除的值
   1. `blpop key timeout` 移除列表的第一个元素，并且返回被移除的值, 如果列表没有元素会阻塞timeout秒直到等待超时或发现可弹出的元素为止。
6. `lindex key index` 通过索引获取列表中的元素

#### hash操作

是字典相关的操作，key对应存的是字典，然后对字典进行相关操作。

1. `hset key dicKey dicValue` 将key对应的字典中的dicKey设置为dicValue
2. `hget key  dicKey` 查看字典中dicKey对应的值
   1. `hgetall  key` 查看字典的所有键值对
3. `hdel key dicKey1 [dicKey2 ...]` 删除字典键值对
4. `hvals key` 查看字典的所有值
5. `hexists key dicKey` 查看字典中是否存在dicKey键值对



#### 查找操作

1. `keys *` 查找当前所有设置的key。
2. `exists key` 判断是否存在key键。



#### 其他操作

1. 自增 `incr key` 将key中存储的数字值加1.
   1. 即使key对应的value数字是字符串类型，也可以。
   2. `incrby key increment` 自增指定的increment值。
   3. 自减 `decr key`





### python

#### 链接

```python
import redis
# 方式1 链接数据库
r = redis.Redis(host='127.0.0.1', port=6379)

# 方式2 
pool = redis.ConnectionPool(host='127.0.0.1', port=6379)
r = redis.Redis(connection_pool=pool)

```

#### 命令

```python
# set操作
r.set("aaa", "bbb")
# 如果 key存在，设置不成功
r.setnx("aaa", "dxdc")
print(r.get("aaa").decode())
# setex   设置10s后过期
r.setex("a", 10, "好的")
print(r.get("aaa").decode())

# hash操作，赋值
r.hset("info", mapping={"name": "zhangsan", "age": 30})
# hash操作 取值
print(r.hget("info", "name"))
print(r.hgetall("info"))

# list操作 赋值
r.lpush("scores", 310)
# list操作  取所有值
print(r.lrange("scores", 0, -1))
# # list操作 插入值
r.linsert("scores", "AFTER", 140, 150)
# list操作 删除第一个元素
r.lpop("scores")

# 集合(无序，去重)操作 添加
r.sadd("name_set", "zs", "lisi", "wangwu", "zs")
# 集合操作 删除
r.srem("name_set", "zs")
# 集合操作 获取值
print(r.smembers("name_set"))
# 随机取两个值
print(r.srandmember("name_set", 2))
# 有序集合操作 按照权重排序
r.zadd("bangdan", {"a": 78, "b": 20, "c": 10, "d": 90})
# 有序集合操作， 取值
print(r.zrange("bangdan", 0, -1))
# 有序集合操作， 取值 带权重
print(r.zrange("bangdan", 0, -1, withscores=True))
# 有序集合操作， 取值 带权重 倒叙
print(r.zrevrange("bangdan", 0, -1, withscores=True))

# 其他操作 获取所有key
print(r.keys("*"))
# 获取所有包含b的key
print(r.keys("*b*"))
# 设置过期时间
r.expire("name", 10)
```



### golang

[文档链接](https://redis.uptrace.dev/zh/guide/go-redis.html)

#### 链接

```go
import (
	"fmt"
	"github.com/go-redis/redis"
)

var redisClient *redis.Client

func initRedis() (err error) {
	redisClient = redis.NewClient(&redis.Options{
		Addr:     "127.0.0.1:6379",
		Password: "",
		DB:       0,
	})
	_, err = redisClient.Ping().Result()
	return
}

func main() {

	err := initRedis()
	if err != nil {
		fmt.Printf("redis链接失败: %v\n", err)
		return
	}
	fmt.Println("redis链接成功")

	// 设置key值， 0代表不会过期
	err = redisClient.Set("name", "zhangsan", 0).Err()
	if err != nil {
		panic(err)
  }
}
```

#### 命令

```go
// 设置key值， 0代表不会过期
	err = redisClient.Set("name", "zhangsan", 0).Err()
	if err != nil {
		panic(err)
	}

	//获取key值
	val, err := redisClient.Get("name").Result()
	if err != nil {
		panic(err)
	}
	fmt.Println("name:", val)

	//批量设置key的值
	err = redisClient.MSet("name1", "zs1", "name2", "zs2", "name3", "zs3").Err()
	if err != nil {
		panic(err)
	}
	//批量获取key的值
	vals, err := redisClient.MGet("name1", "name2", "name3").Result()
	if err != nil {
		panic(err)
	}
	fmt.Println(vals...)
```

#### 发布订阅模式

```go
pubsub := redisClient.Subscribe("mychannel1")

	defer pubsub.Close()

	err = redisClient.Publish("mychannel1", "hello world").Err()

	if err != nil {
		panic(err)
	}

	for {
		msg, err := pubsub.ReceiveMessage()
		if err != nil {
			panic(err)
		}
		fmt.Println("enene", msg.Channel, msg.Payload)

	}
```

或者

```go
pubsub := redisClient.Subscribe("mychannel1")

	defer pubsub.Close()

	err = redisClient.Publish("mychannel1", "hello world").Err()

	if err != nil {
		panic(err)
	}
	time.AfterFunc(time.Second*3, func() {
		// When pubsub is closed channel is closed too.
		pubsub.Close()
	})
	ch := pubsub.Channel()

	for msg := range ch {
		fmt.Println("enene", msg.Channel, msg.Payload)
	}
```

也可以启动多个终端，一个执行发布，其他的执行订阅。要先启动订阅，在执行发布。

```go
//订阅
package main

import (
	"fmt"

	"github.com/go-redis/redis"
)

func main() {

	redisClient1 := redis.NewClient(&redis.Options{
		Addr:     "127.0.0.1:6379",
		Password: "",
		DB:       0,
	})

	publish := redisClient1.Subscribe("chat")
	defer publish.Close()

	for msg := range publish.Channel() {
		fmt.Printf("channel=%s message=%s\n", msg.Channel, msg.Payload)
	}

}
```

```go
//发布
package main

import (
	"fmt"

	"github.com/go-redis/redis"
)

func main() {

	redisClient2 := redis.NewClient(&redis.Options{
		Addr:     "127.0.0.1:6379",
		Password: "",
		DB:       0,
	})

	n, err := redisClient2.Publish("chat", "helllllo").Result()
	if err != nil {
		fmt.Println(err)
	}

	fmt.Println(n, "received message")

}

```













