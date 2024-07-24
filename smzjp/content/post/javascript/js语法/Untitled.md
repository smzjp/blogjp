1. js的运行环境
   1. 浏览器客户端
   2. node.js

## 作用域

### 局部作用域

1. 函数作用域

   var，let，const

   外部无法访问

2. 块作用域

   包括 {}，if{}，for(...){}

   ES6新增的，只有let，const有块作用域

   ```javascript
   //举例1
   for (let i = 1; i <= 3; i++) {
     console.log(i)
   }
   //报错
   console.log(i)
   ```

   如果把代码里的let换成var，就不会报错。

### 全局作用域

1. 没有放到任何{}里面的

2. <script> 标签和.js文件的最外层就是全局作用域  

### 作用域链

先从最内层找，然后一层一层往外找，知道全局作用域



### 垃圾回收

低版本的IE浏览器使用的是引用计数算法

引用计数算法会有循环引用的问题

目前现代浏览器使用的都是标记-清除算法

### 闭包

```javascript
function outer() {
    const a = 1
    function f() {
        console.log(a)
    }
    return f
}
let ff = outer()
ff()
ff()
```

闭包=内层函数+外层函数的变量

### 变量提升

代码运行的时候，会把函数的声明，创建提升到当前作用域的最开头

代码运行的时候，会把用var声明的变量的声明过程（没有赋值过程）提升到当前作用域的开头，在函数提升后面

let，const不存在变量提升，变量提升是旧的语法

```javascript
console.log(a)
fn()
var a = 10
function fn() {
    console.log(a)
}
```



```javascript
let a = 10
function fn() {
    console.log(a)
    let a = 20
}
fn()
```

这里要注意，这段代码会报错，let没有变量提升，但是fn里面的a也不会使用外层的a变量。

解释  const会提升创建，不会提升声明，相当于占位。但是不是声明，所以不能使用。

```javascript
console.log(a)
var a = 10
function a() {
    console.log(111)
}
console.log(a)

var a = 20
console.log(a)

var b = 30
var b
console.log("b =",b)


var c = 50
function c() {
    console.log("ccc")
}
console.log("c = ",c)

//打印结果
//[Function: a]
//10
//20
//b = 30
//c =  50
```

a函数会被提升到最前面，所以第一次打印的a是`[Function: a]`。

## 函数

### 动态参数

```javascript
function sum() {
    console.log(arguments)
    let s = 0
    for(let i = 0; i < arguments.length;i++) {
        s += arguments[i]
    }
    return s
}
console.log(sum(1,2,3))
console.log(sum(1,2))
```

`arguments`可以动态获取实参

### 剩余参数

ES5新增语法

```javascript
function fn(a, b, ...c) {
    console.log(a,b,c)
}
fn(1,2,3,4,5)
```

`...`可以指代多个实参，用数组接收。

### 箭头函数

1. `=>`中间不能有空格
2. `=>`和`{}`之间可以有回车
3. `()`和`=>`之间不能有回车
4. `()`里面放形参列表，`{}`里面放函数体

```javascript
//简写 如果形参只有一个，可以省略()
let fn = a => {
    console.log(a)
}
fn(33)

//如果 函数体只有一行，可以省略{},并且会return这一行的表达式
let fn = a => a+a
cc = fn(33)
console.log(cc)
```

```javascript
//注意，这种情况不能直接省略{},会有歧义
let fn = () => {
    return {uname:'zs',age:18}
}
console.log(fn())

//可以这样,再加一个()
let fn = () => ({uname:'zs',age:18})
console.log(fn())
```

**注意**

1. 箭头函数内部没有arguments，如果要获取动态参数，可以使用剩余参数
2. 箭头函数内部没有this，
3. 箭头函数属于表达式函数，因此不存在函数提升

### 数组解构

```javascript
//案例1
let [a,b,[c,d]] = [1,2,[5,8]]
console.log(a,b,c,d)

//案例2
let [,a,b,] = [1,2,3,4]
console.log(a,b)
```

### 对象的解构

```javascript
//不需要按顺序，变量名要等于键名
let {sex,uname} = {uname:'zs',age:18,sex:'男'}
//也可以，其他的可以用other接收
let {sex,uname,...other} = {uname:'zs',age:18,sex:'男'}

//案例2
let person = {
    uname:'zs',
    age:33,
    dog: {
        dname:'旺财',
        dage:2
    },
    cat: {
        cname: '花花',
        cage:1
    }
}
let {uname,dog:{dname}} = person
console.log(uname)
console.log(dname)

//使用案例3  
let obj = {
    uname: 'zs',
    age:22,
    sex:'男'
}
// 这里相当于 let { sex } = obj
function fn({sex}) {
    console.log(sex)
}
fn(obj)

//案例4
//变量有冲突时，或者想换一个变量
let {sex:xingbie,uname} = {uname:'zs',age:18,sex:'男'}
console.log(xingbie)

```





