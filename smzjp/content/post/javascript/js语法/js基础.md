

#### 存放位置

1. 内嵌式
   1. 可以放在html文件的任何位置。推荐放在head或者body标签里面，需要用script标签标记。
2. 外联式
   1. 把js代码统一放在一个.js结尾的js文件中。然后在html里面通过script标签导入。
3. 行内式
   1. 内嵌在其他比如button标签里面

**注意**

内嵌式和外联式不能共用同一个script标签

#### 注释

1. 单行注释 `//`
2. 多行注释`/* */`

#### 结束符

js的结束符是`;` 也是可以省略的。

#### 输出语法

```javascript
//1.将信息在网页中以弹窗的形式输出
alert("abc")
//2.将信息在控制台以文本的形式输出
console.log("abc")
//3.将信息在网页的body标签中输出
document.write("abc")
```

#### 输入语法

```js
//1. 会弹窗。有输入框和确定，取消按钮
prompt("请输入用户名:")
//2.会弹窗，有确定和取消两个按钮
confirm("确定删除吗？")
```

#### 字面量

在计算机科学中，字面量(literal)是在计算机中描述 事/物 的

#### 获取用户输入

 ```js
 
 let name = prompt('请输入用户名:')
 document.write(name)
 
 ```

#### let和var的区别

var一般不使用，是旧版的。

let是为解决var的问题而出现的

**不同点**

1. let 定义的变量，必须先定义再使用。var定义的变量，可以先使用后定义
2. let定义的变量名，不能重复定义。var定义的变量名可以重复。

#### 常量

使用关键字`const`定义变量

#### 隐式数据类型转换

1. 如果是加法运算，变量当中有字符串和数字类型，会把非字符串的变量转换成字符串类型，然后拼接。

   ```js
   let a = '1'
   let b = 2
   let c = a + b
   console.log(c)
   console.log(typeof c)
   //打印结果
   //11
   //string
   
   //如果不希望返回字符串，想要执行加法运算 可以在前面加个 + 号
   let c = +a + b
   
   ```

2. 非加法运算中，会把字符串类型转换成数字类型，然后再计算

   ```js
   let a = '2'
   let b = '3'
   let c = a * b
   console.log(c)
   console.log(typeof c)
   //打印结果
   //6
   //number
   ```

#### 字符串拼接

```js
let username = '老段';
let age = 30;
//方法1 加号拼接
console.log('姓名:'+ username + '\n' + '年龄:'+age)
//方法2 反引号模版字符串拼接
console.log(`姓名：${username}`)
```



#### undefined类型

如果变量的值是undefined或者变量没有赋值，那么变量的数据类型就是undefined类型

```js
let a = undefined;
let b;
console.log(typeof a)
console.log(typeof b)
//打印结果
//undefined
//undefined
```

#### null空类型（空对象类型）

如果变量的值是null，那么变量的类型叫空类型

```js
let a = null
console.log(typeof(a))

let b = {}
console.log(typeof(b))
//注意a和b的区别。b是一个不完全空对象，原型链上有Object。null是完全空对象，原型链也没有
console.log(Object.getPrototypeOf(b))
// 不能执行，会报错
// console.log(Object.getPrototypeOf(a))

console.log(Object.prototype.toString.call(a))
console.log(Object.prototype.toString.call(b))
//打印结果
//object
//object
//[Object: null prototype] {}
//[object Null]
//[object Object]

```

#### 强制类型转换

1. 强制转换为数字类型

   ```js
   //强制转换为数字类型
   let a = Number("2.12")
   console.log(a)
   //只保留整数部分
   let b = parseInt('2.12')
   console.log(b)
   let c = parseFloat('2.12')
   console.log(c)
   console.log(a + b + c)
   //打印结果
   //2.12
   //2
   //2.12
   //6.24
   
   //特殊
   //parseInt会保留转换成功的部分 并且只会转换整数部分
   let d = parseInt('2.1abc')
   console.log(d)
   //parseFloat会保留转换成功的部分
   let e = parseFloat('2.1abc')
   console.log(e)
   //Number转换的时候有一个转换不成功，就会返回NaN（not a number）
   let f = Number('2.1abc')
   console.log(f)
   //打印结果
   //2
   //2.1
   //NaN
   ```

2. 强制转换为字符串类型

   ```js
   //方法1
   let a = String(2.3)
   console.log(a)
   //方法2 toString(进制)，不填默认是10进制
   let b = 66
   console.log(b.toString(8))
   //打印结果
   //2.3
   //102
   ```

3. 语法糖

   ```js
   let a = '1'
   let b = '2'
   //在字符串变量前面添加一个 + 号，会变为数字类型
   let sum = +a + +b
   console.log(sum)
   
   
   ```

   



#### 比较运算符

```js
let a = 1;
let b = '1';

//等于 == 只比较值是否相等 true
console.log(a == b)
//全等于 === 比较值和数据类型是否都相等 false
console.log(a === b)

```

#### 语句和表达式

**语句:**是一段可以执行的代码，重点是用来执行，不是用来计算某个具体结果。比如`prompt()`,`alert()`

**表达式:**在程序中能够得出，计算出某一个具体的结果。比如`3+4`,`num++`, 因为表达是可以被求值，所以可以写在赋值语句的右侧。`x = 3+4`

#### 逻辑运算符短路

```js
console.log(3 || 4)
console.log(0 || '' || 2 || 3)
console.log(3 && 4)
//打印结果
//3
//2
//4
```

#### switch语句

必须加break，不然会穿透

```js
let num = 1;
switch (num) {
    case 1:
        console.log("1111")
        break;
    case 2:
        console.log("222222")
        break;
    default:
        console.log("default")
}

```

#### 数组操作

```js
let arr = []
//1.增 向数组后面添加
arr.push(2)
arr.push(1)
//向数组前面添加
arr.unshift(4,3)
arr.unshift(5)
console.log(arr)
//排序
arr.sort(function (a,b) {
    //升序
    return a - b;
    //降序
    // return b - a;
})
console.log(arr)
//2.删 删除数组最后一个值
arr.pop()
console.log(arr)
//删除数组第一个值
arr.shift()
console.log(arr)
//指定删除开始位置的下标及个数
arr.splice(0,2)
console.log(arr)
```

#### Promise函数

是为了解决回调地狱出现的。

```js
//假设有一段代码，需要嵌套很多层。
setTimeout(function () {
     console.log("111111")
     setTimeout(function () {
         console.log('2222222')
         setTimeout(function () {
             console.log('333333')
         },3000)
     },2000)
 },1000)

//上面代码用promise的写法
new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log('1111111')
        resolve()
    },1000)
}).then(function () {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log('2222222')
            resolve()
        },2000)
    })
}).then(function () {
    setTimeout(function () {
        console.log('33333333')
    },3000)
})
```

















