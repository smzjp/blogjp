---
original: true
title: mysql数据库
description: 常用方法和在golang，python的链接
date: 2022-09-19
slug: 
image: 
tags:
    - mysql
categories:
    - Golang
    - Python
---

#### 安装

##### 服务端

[下载链接](https://dev.mysql.com/downloads/mysql/)

##### 客户端

可以直接用vscode或者Pycharm提供的插件，也很方便。

#### 常用命令

##### 链接操作

1. 链接数据库 `mysql -h主机地址 -u用户名 -p用户密码`
   1. 注意用户名和主机地址前可以有空格， 密码前不能有空格
   2. 本地一般是 `mysql -u root -p`  分两步，第二步输密码
   3. 指定ip和端口号 `mysql -h 127.0.0.1 -u root -p -P 3306`
   4. mysql的默认端口是3306

##### 数据库操作

在数据库里面操作末尾记得加上`;`分号表示命令结束。

1. `show databases;` 显示所有数据库
   1. `show create database database_name;` 查看某个数据库的创建信息，可以看到数据库的编码格式等信息。
2. `create database database_name;` 创建数据库
   1. `create database database_name character set gbk;` 可以指定编码格式
3. `alter database database_name character set gbk;` 修改数据库
   1. 只能修改编码格式，不能修改数据库名
4. `drop database [if exists] spiders10;` 删除数据库
5. `use database_name;` 进入某个数据库
6. `select database();` 显示当前使用的是哪个数据库

##### 表操作

需要先执行 `use database_name;` 进入到某个具体的数据库。

1. `show tables;` 显示当前数据库所有的表

2. 创建表

   ```sql
   create table worker (
       name varchar(32) unique ,
       age int not null ,
       salary float(6, 2),
       job char(6)
   )
   ```

   `unique`和`not null` 是约束，表示字段值唯一，和字段值不能为空。

   `varchar(32)` 表示变长字符串类型，最大不能超过23字符。

   `char(6)` 表示定长字符串，固定6个字符。

   `float(6, 2)` 表示浮点类型，最大六位数，其中有两位是小数点xxxx.xx

3. `desc table_name;` 显示某个表的字段信息

4. `show create table table_name;` 查看某个表的建表时候所使用的语句

5. `drop table table_name;` 删除某个表

6. `rename table table_name to table_name_new;` 修改表名

7. 

##### 表记录操作

1. `INSERT [INTO] <表名> [<列名1>[,...<列名n>]] VALUES (值1) [,...(值n)];`插入表记录。

   1. 指定需要插入数据的列名，若向表中的所有列插入数据，则列名可以省略，注意列名和值要一一对应。

   2. `[]`里面的表示可选。

   3. 举例

      ```sql
      insert into workers (name, age, salary, job) values ("张三", 25, 5555.32, "为");
      # 或者
      insert into workers values ("李四", 25, 5555.32, "为");
      # 添加多个
      insert into workers values
                              ("王五", 25, 5555.32, "为"),
                              ("王五2", 25, 5555.32, "为"),
                              ("王五3", 25, 5555.32, "为"),
                              ("王五4", 25, 5555.32, "为");
      ```

2. 查询表记录

   ```
   SELECT *|字段名1[,地段名2,...] FROM table_name	
   										[WHERE 条件
   										GROUP BY 字段名
   										HAVING 刷选
   										ORDER BY 字段名
   										LIMIT 限制条件]
   ```

   `where 条件`中可以使用

   1. 比较运算符:
      1. `> < >= <= <> =`
      2. `between 80 and 100` 值在80到100之间
      3. `in(80,90,100)` 值是80，90或100
      4. `like '张%'` 
         1. `%` 表示0到任意多字符，可以匹配张三，张小凡等
         2. `_` 表示一个字符，只能匹配张三，`_` 可以使用多个。
   2. 逻辑运算符:
      1.  多个条件可以使用`and` ` or`  ` not`
   3. 正则:
      1. `SELECT * FROM table_name WHERE 字段名 REGEXP '正则语句'; `

   `LIMIT 限制条件`

   1. 限制记录的个数

      `limit 3`表示取前三条数据

   2. 指定位置取记录

      `limit 3,4` 取三条数据，从第三条开始取往后数三条

   

   举例

   ```sql
   # 查询workers表的所有字段数据,\G表示数据垂直显示，如果是在终端操作，会比较清楚记录看起来。
   select * from workers\G;
   # 查询 指定字段的数据
   select  name,age from workers;
   # 查询 所有age大于30的记录
   select  name,age from workers where age > 30;
   # 查询 所有名称包含王的记录
   select  name,age from  workers where name like "%王%";
   # 降序desc 升序esc 先按age排序，如果age一样再按salary排序
   select name,age from workers order by age desc,salary desc ;
   
   ```

   

3. 表记录删除 `DELETE FROM <表名> [WHERE 语句] [ORDER BY 语句] [LIMIT 语句]`

   1. `delete from table_name;` 删除表的所有记录
   2. `delete from workers where name = "张三";` 删除字段名name是张三的记录

4. 表记录更新 `UPDATE <表名> SET 字段1=新值1 [字段2=新值2...] [WHERE 语句]`

   1. `update workers set age = 30 where name = "李四";` 把李四的年龄改为30

#### python

##### 链接

```python
import pymysql

try:
    # 1.创建链接对象
    conn = pymysql.Connect(
        # mysql数据库服务器地址
        # host='127.0.0.1',
        host='localhost',
        # mysql端口号
        port=3306,
        # mysql的用户名
        user= 'root',
        # mysql的密码,如果没有密码为空''就可以
        password='xxxx',
        # 数据仓库的名字
        db='spiders',
      	# 设置返回结果以字典，数组形式，默认是元组
        cursorclass=pymysql.cursors.DictCursor,
        # 编码格式
        charset='utf8'
    )
except Exception as e:
    print(e)
else:
    print('成功链接数据库')
```

##### 执行

```python
# 2.创建游标对象，用来执行sql语句的
cursor = conn.cursor()

def createDataBase():
    # 创建数据库，数据库名称一般用小写
    return 'CREATE DATABASE spiders'

def changeDataBase():
    # 切换数据库,创建链接对象conn的时候也有指定db，后面如果需要切换的话，可以用这个语句
    return 'USE spiders'

def deleteTable():
    # 删除表
    return 'DROP TABLE IF EXISTS urls'

def createTable():
    # 创建表
    # return 'CREATE TABLE EMPLOYEE ( FIRST_NAME CHAR(20) NOT NULL, LAST_NAME CHAR(20), AGE INT, SEX CHAR(1), INCOME FLOAT)'
    return 'CREATE TABLE urls ( id INT PRIMARY KEY AUTO_INCREMENT, url VARCHAR(1000) NOT NULL , content VARCHAR(4000) NOT NULL , create_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP)'


def insertData():
    # 插入数据
    # return 'insert into EMPLOYEE values ("三","李",29,"男",123.0)'
    return 'insert into urls (url, content) values ("www.baidu.com","这是内容呃呃嗯嗯嗯")'


def deleteData():
    # 删除数据
    return 'delete from EMPLOYEE where FIRST_NAME = "三"'

def updateData():
    # 更新数据
    return 'update EMPLOYEE set FIRST_NAME = "%s" where LAST_NAME = "张"'%'三dd三'
    # return 'update EMPLOYEE set AGE = %d where LAST_NAME = "张"'%100

def searchData():
    # 查询数据
    # return  'select * from EMPLOYEE'
    # *表示返回所有的列
    # return  'select * from urls where url = "www.baidu.com"'
    # 返回指定的列
    return  'select url,content from urls where url = "www.baidu.com"'

def zipSearchTable():
    # 表合并查找，webs和urls是表名，链接条件是webs.web_url = urls.url
    # 内链接，必须web_url和url相等的行才会合并显示，类似并集
    return 'select * from webs inner join urls on webs.web_url = urls.url'
    # 左链接,如果左边的表(webs)的数据有多(存在web_url，找不到url与其相等)， 会保留此数据
    # return 'select * from webs left join urls on webs.web_url = urls.url'
    # 右链接，如果右边的表的数据有多(存在url，找不到web_url与其相等)， 会保留此数据
    # return 'select * from webs right join urls on webs.web_url = urls.url'

try:
    # 3.执行sql语句
    # cursor.execute(deleteTable())
    # cursor.execute(createTable())
    # cursor.execute(insertData())
    # cursor.execute(deleteData())
    # cursor.execute(updateData())
    cursor.execute(searchData())
    # 查询数据，需要多执行一个fetch操作,才可以拿到数据
    result = cursor.fetchall()
    # 获取一条数据
    # result = cursor.fetchone()
    print("查询结果:",result)

    # 4. 提交数据，如果不提交，mysql数据库将不会生效
    conn.commit()
except Exception as e:
    print("错误: " + e)
    # 4. #撤销数据,如果发生错误则回滚
    conn.rollback()

```

##### 关闭

```python
#5.关闭资源
cursor.close()
conn.close()
```

#### golang

golang中使用mysql，首先需要安装驱动 `go get -u github.com/go-sql-driver/mysql`

##### 链接

```go
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

func main() {
  db, _ := sql.Open("mysql", "root:123456@(127.0.0.1:3306)/spiders")
  //设置打开数据库连接的最大数量，也就是并发数。
	//默认是没限制
	db.SetMaxOpenConns(25)
	// 用于设置连接池中连接的数量。
  //默认是2
	db.SetMaxIdleConns(5)
	//链接池中的链接的最大存活时间
	db.SetConnMaxLifetime(5 * time.Minute)
	defer db.Close()

	if err := db.Ping(); err != nil {
		fmt.Println("数据库连接失败")
		return
	}
	fmt.Println("数据库连接成功")
}

```

`SetMaxOpenConns`是设置数据库的并发能力，如果设置为25，那么假设都26个协程同一时间点访问数据库，那么有一个有一个协程需要等待。

`SetMaxIdleConns`  是设置链接池的数量，如果设置为5，那么假设有10个协程同一时间访问数据库，那么有5个协程可以直接去链接池里面去链接，不用新创建链接。其他5个协程就需要新创建链接。如果是链接池没有链接，那么会创建10个链接，数据库访问完毕后，其中5个会进入链接池，供后面复用。其他剩下的5个链接会直接关闭。

`SetConnMaxLifetime` 是用来控制链接池中的链接的存活时间的，如果长时间没有访问数据库的请求，链接池中的链接一直开着出浴空闲状态也是浪费。所以需要设置一个最大存活时间，超过这个时间就主动关闭链接。再有，mysql的服务器也有一个类似的控制时间`wait_timeout` 默认是8小时, 链接如果一直空闲超过这个时间，服务器会关闭链接，这时候如果go程序去调用这个链接会panic。所以不如go程序主动设置个时间主动断开链接，这样至少不会报错。设置的时间比  `wait_timeout`短就可以。

##### 查询

```go
  var id int
	var url, content, date string
	//多行查询
	rows, _ := db.Query("select * from urls")
	for rows.Next() {
		rows.Scan(&id, &url, &content, &date)
		fmt.Println(id, url, content, date)
	}

	//查询单行
	row := db.QueryRow("select url,content from urls where url = 'www.baidu.com'")
	row.Scan(&url, &content)

```

##### 插入操作

```go
	//插入操作
	sql := "insert into urls values (3,'www.cc.com','enenen','2023-02-04 19:42:22')"
	result, _ := db.Exec(sql)
	n, _ := result.RowsAffected()
	fmt.Println("受影响的记录是", n)

  //占位符插入操作 推荐使用占位符 防止注入
	datas := [][]string{{"4", "www.xx.com", "enenen", "2023-02-04 19:42:22"}, {"5", "www.kk.com", "enenen", "2023-02-04 19:42:22"}}
	pre, _ := db.Prepare("insert into urls values (?,?,?,?)")
	for _, d := range datas {
		pre.Exec(d[0], d[1], d[2], d[3])
	}
```

##### 事物

```go
//开启事务
	tx, err := db.Begin()
	if err != nil {
		fmt.Println("tx fail")
	}
	// 准备sql语句,
	pre, _ := tx.Prepare("delete from urls WHERE id = ?")
	res, err := pre.Exec(2)
	if err != nil {
		fmt.Println("err", err)
    if err = tx.Rollback(); err != nil {
			fmt.Println("rollbackerr", err)
		}
		return
	}
	n, _ := res.RowsAffected()
	fmt.Println("受影响的记录是", n)

	res, err = pre.Exec("eee")
	if err != nil {
		fmt.Println("err", err)
		if err = tx.Rollback(); err != nil {
			fmt.Println("rollbackerr", err)
		}
		return
	}
	n, _ = res.RowsAffected()
	fmt.Println("受影响的记录是", n)
	fmt.Println("提交事物")
	// 提交事务
	tx.Commit()
```

注意事物里面的操作要使用`tx`的，`tx`对象也拥有数据库交互的`Query`、`Exec`、`Prepare`方法，用法和`db`对象类似。事物最终一定要调用`Commit`或者`Rollback`方法，不然链接不会释放。











