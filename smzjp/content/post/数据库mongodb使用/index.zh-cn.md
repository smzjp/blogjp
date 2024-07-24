---
original: true
title: mongodb的使用
description: 常用命令和在golang，paython中的使用
date: 2022-11-05
slug: 
image: 
tags:
   - mongodb
categories:
   - Golang
   - Python
---

### 安装

1. 在mac下通过homebrew安装。
2. 可以通过 `brew search ` 搜索 mongodb。看有哪些版本。
3. `brew install mongodb-community@5.0` 这里安装**mongodb-community@5.0**版本

### 启动

#### mongodb服务器

1. 通过brew在后台启动服务器，`brew services start mongodb-community@5.0`

#### mongodb客户端

1. `mongosh` 启动客户端
   1.   `mongosh` 或者  `mongosh -u root -p root123 admin` 

### 命令

[官方文档](https://docs.mongoing.com/mongodb-crud-operations)

 collection对应mysql的table，文档对应mysql的一条记录。

#### 数据库相关操作

1. `show dbs` 或者 `show databases` 显示所有数据库
2. `use xxx`  使用指定数据库，如果不存在会新建此数据库
3. `db` 显示当前正在使用的数据库
4. `db.dropDatabase()` 删除当前使用的数据库

#### collect(表)相关操作

运行命令前需要先执行进入到具体的某一个数据库才可以 `use xxx`

1. `show collections` 或者`show tables` 显示当前数据库的所有集合(表)

#### 文档(记录)相关操作

假设插入了一个文档

```
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );
```

##### 新增

1. 新增一个文档

   ```
   db.inventory.insertOne({ item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" })
   ```

2. 新增多个文档 

   ```
   db.inventory.insertMany([{ item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },{ item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }])
   ```

##### 查询

1. 查询所有文档  `db.inventory.find({})` 或者 `db.inventory.find()` 或者`db.goods.find().pretty()` 按结构化展示所有记录。
2. 等值查询 `db.inventory.find({"item":"sketchbook"})`  查找item等于sketchbook的所有文档
3. 查找单个文档 `db.inventory.findOne({"item":"sketchbook"})` 只返回第一个找到的文档
4. 取指定字段显示`db.inventory.find({"item":"sketchbook"},{"_id":0,item:1})` 只显示item字段，0表示隐藏，1表示显示。
5. 排序 默认是按照_id 升序排列的 , 1代表升序，-1代表降序`db.inventory.find( { tags:{$all:["blank","red"]}} ).sort({qty:1,item:-1})`
6. 获取数量`db.inventory.count({"item":"paper"})` 
7. 分页
   1. `db.inventory.find().limit(2)` 取两个记录
   2. `db.inventory.find().limit(2).skip(1)`  跳过第一条记录，再开始取两条记录

###### 条件查询

1. 比较运算符查找

   | $ne  | $gt  | $lt  | $get | $lte |
   | :--: | :--: | :--: | :--: | :--: |
   |  !=  |  >   |  <   |  >=  |  <=  |

   `db.inventory.find({qty:{$gt:50}})` 查找qty大于50的所有文档

2. and 逻辑运算符查找 `db.inventory.find({qty:{$gt:50},qty:{$lt:80}})` 查找qty大于50且小于80的所有文档

3. or逻辑运算符查找 `db.inventory.find({$or:[{item:"postcard"},{item:"planner"}]})` 查找item等于postcard或者item等于planner的所有文档

4. 模运算 `db.inventory.find({qty:{$mod:[2,1]}})` 查找 qty 除 2，余数是1的记录。也就是所有qty是奇数的记录

5. not 逻辑运算 `db.inventory.find({qty:{$not:{$mod:[2,1]}}})`, 查找所有qty是偶数的记录

6. 正则匹配 `/正则表达式/i`

   1. `db.inventory.find({item:/^s.*/i})`, `^`表示与张开头，`.`是匹配任意字符，`*`表示任意数量的字符。查找所有item值是s开头的文档，也可以写成 `db.inventory.find({item:{$regex:"^s.*"}})`

7. 成员运算 `db.inventory.find({qty:{$in:[80,95]}})`

8. | $in  |  $nin  |
   | :--: | :----: |
   |  in  | not in |

   

###### 数组查询

插入如下文档

```
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

1. `db.inventory.find( { tags:["red", "blank"]})` 查询tags值等于["red", "blank"]的文档，注意数组需要顺序也一致。
2. `db.inventory.find( { tags:"blank"} )` 查看tags元素包含blank的记录。
3. `db.inventory.find( { tags:{$all:["blank","red"]}} )`查看tags元素既包含blank又包含red的记录。

##### 更新

1. 更新单个文档的指定字段

   ```mongodb
   db.inventory.updateOne(
       { item: "paper" },
       {
           $set: { "size.uom": "cm", status: "P" }
       }
   )
   ```

   `{ item: "paper" }` 是条件，找到第一个符合这个的文档。`{$set: { "size.uom": "cm", status: "P" }}`是修改的内容，

2. 更新单个文档的指定字段，如果没找到文档会执行插入操作

   ```mongodb
   db.inventory.updateOne(
       { item: "paper" },
       {
           $set: { "size.uom": "cm", status: "P" }
       },
       {upsert:true}
   )
   ```

3. 更新多个文档

   ```mongodb
   db.inventory.updateMany( 
         { "qty": { $lt: 50 } },
         {  
             $set: { "size.uom": "in", status: "P" }
         }
     )
   ```

   `{ "qty": { $lt: 50 } }` 是条件，找到所有符合条件的文档，把这些文档按照`{$set: { "size.uom": "in", status: "P" }}`修改内容。

4. 替换文档(覆盖)

   ```mongodb
   db.inventory.replaceOne(
      { item: "paper" },
      { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
   )
   ```

   `{ item: "paper" }` 是条件，找到符合条件的第一个文档，把这个文档的内容替换为新内容。旧内容除了"_id"不变，其他内容都没了。

5. 自增或者自减

   1. `db.inventory.update({}, {$inc:{qty:1}})` ,找到符合条件的第一个文档，使这个文档的qty值增加1
   2. `db.inventory.update({}, {$inc:{qty:-1}})` 找到符合条件的第一个文档，使这个文档的qty值减少1

##### 删除

1. 删除所有文档 `db.inventory.deleteMany({})`
2. 删除所有符合条件的文档 `db.inventory.deleteMany({status: "A"})`
3. 删除符合条件的一个文档 `db.inventory.deleteOne({ status: "A" })`

#### 权限设置

##### 创建超级管理员用户

1. `use admin` 进入admin数据库
2. `db.createUser({user:"root",pwd:"root123",roles:[{role: "root",db:"admin"}]})`
3. 然后关闭服务，重启服务，可以使用用户，密码链接登陆 `mongosh -u root -p root123 admin` admin表示数据库，这样就可以直接登陆到admin数据库，不用使用use命令切换。
4. 如果登陆的时候没有使用用户登陆，也可以登陆后再进行验证使用命令 `db.auth("root","root123")`,注意要先切换到 `use admin`再执行。

##### 创建普通用户

1. `db.createUser({user:"app01",pwd:"app01",roles:[{role:"read",db:"app"},{role:"readWrite",db:"test"}]})` ,user表示用户名称，pwd用户密码，role是在指定的数据库的权限角色，官方有定义很多，可以看官方文档，db表示对应的数据库名称

##### 查看用户

1. `use admin`
2. `db.system.users.find()`

##### 删除用户

1. 需要root用户登录
2. `db.dropUser("app01")` app01是用户名称

##### 注意

mongodb默认是不要权限就可以操作的，设置权限后，需要在配置文件 **mongod.conf**里面添加配置

```
security:
    authorization: enabled
```

配置文件的路径在 `/opt/homebrew/etc/mongod.conf`

### python

#### 链接

```python
import pymongo


# 链接数据库 database是数据库名称
def get_db(database):
    conn = pymongo.MongoClient(
        host='127.0.0.1',
        port=27017,
        username="root",
        password="root123"
    )
    db = conn[database]
    return db
```

#### 执行

```python
# 查询数据
def find(table):
    db = get_db("sanqi")
    return db[table].find()


# 插入一条数据 table是集合(表)名称，
def insert_one(table, data):
    db = get_db('sanqi')
    return db[table].insert_one(data)


# 根据condition条件在table中查询，然后将查询到的数据修改为data
def update(table, condition, data):
    db = get_db('sanqi')
    return db[table].update_one(condition, {"$set": data})


# 根据condition条件删除数据
def delete(table, condition):
    db = get_db('sanqi')
    return db[table].delete_one(condition)


# 插入
r = insert_one('stu', {"school": "qinghua", "age": 33, "name": "alex"})
# 修改数据，把age是33的doucments(行)的name修改为"hahahahaha"
r = update("stu",{'age':33},{"name":"hahahahaha"})
# 删除
r = delete('stu',{"school": 'qinghua'})
# 查找
r = find("stu")
print(list(r))
```



### golang

首先下载 `go get -u go.mongodb.org/mongo-driver/mongo` 驱动

#### 链接

```go
package main

import (
	"context"
	"fmt"
	"time"

	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
	"go.mongodb.org/mongo-driver/mongo/readpref"
)

func main() {

	// uri := "mongodb://localhost:27017"
	uri := "mongodb://root:root123@localhost:27017/admin"
	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()
  // ctx := context.TODO()
	client, err := mongo.Connect(ctx, options.Client().ApplyURI(uri))
	if err != nil {
		panic(err)
	}

	defer func() {
		if err = client.Disconnect(ctx); err != nil {
			panic(err)
		}
	}()
	if err := client.Ping(ctx, readpref.Primary()); err != nil {
		panic(err)
	}
	fmt.Println("mongodb 链接成功")

}

```

#### 执行

方式1

```go
//集合(表)
	collection := client.Database("sanqi").Collection("stu")
	//设置空的查询条件
	cursor, err := collection.Find(context.Background(), bson.D{})
	// 定义bson.M类型的文档数组，bson.M是一个map类型的键值数据结构
	var results []bson.M
	// var results []map[string]interface{}
	// 使用All函数获取所有查询结果，并将结果保存至results变量。
	if err = cursor.All(context.TODO(), &results); err != nil {
		log.Fatal(err)
	}
	// 遍历结果数组
	for _, result := range results {
		fmt.Println(result)
	}
```

方式2 

```go
var results map[string]interface{}
	//集合(表)
	collection := client.Database("sanqi").Collection("stu")
	//设置空的查询条件
	err = collection.FindOne(context.Background(), bson.D{}).Decode(&results)
	// 定义bson.M类型的文档数组，bson.M是一个map类型的键值数据结构

	fmt.Println(results)
```

方式2只针对结果只有一个的





**参考文档:** [https://www.tizi365.com/topic/92.html](https://www.tizi365.com/topic/92.html)







