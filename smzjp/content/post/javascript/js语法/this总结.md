全局中的`this`表示window

普通函数中 `this`表示 window

定时器中的`this`表示window

事件处理函数中的`this`表示事件源

构造函数，对象方法，原型对象方法中的`this`，表示实例对象

静态方法中的`this`表示构造函数

箭头函数中 `this` 需要按照作用域链去查找

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<script>

  var age = 10
let obj = {
    age:20,
  	//箭头函数中没有this，所以此this指的是外层作用域的也就是全局作用域
    say: () => {
        console.log(this.age)
    },
    eat: function () {
      	//箭头函数中没有this，所以这里的this指向eat方法中的this，既obj
        let fn = () => {
            console.log(this.age)
        }
        fn()
    },
    wash: () => {
        function abc() {
            console.log(this.age)
        }
        abc()
    }
  
}

obj.say()
obj.eat()
obj.wash()

</script>

</body>
</html>
```

打印结果是10和20和10



#### 修改this的指向



1. call方法。

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   
   <script>
   
     let obj = {age: 20}
     function fn() {
         console.log(this)
     }
   
     function fx(x, y) {
         console.log(this)
         console.log(x + y)
     }
   
     //正常调用函数中的this表示window
     fn()
     //fn.call(obj)可以代替fn(),并且改变了this。call方法的第一个参数表示新的this
     fn.call(obj)
     fx.call(obj,3,4)
     //apply和call类似,只是传的参数必须是数组
     fx.apply(obj,[3,6])
   	//bind 方法不会调用函数，而是创建了一个指定了this值的新函数
     //所以要加()调用
     fx.bind(obj,3,5)()
   </script>
   
   </body>
   </html>
   ```

2. apply方法

   和call方法类似，只是传参数是要数组

3. bind方法

   bind方法并不会调用函数，而是创建一个指定了this的新函数

   语法和call一样

   



