---
original: true
title: golang语法
description: 基本语法
date: 2022-12-20
slug: 
image: 
tags:
    -
categories:
    - Golang
---

### 基础语法

#### 注释

1. 单行注释

   ```go
   	//单行注释
   	fmt.Println("hello world")
   
   ```

2. 多行注释

   ```go
   	/*
   		多行注释
   		多行注释
   	*/
   	fmt.Println("hello world too")
   ```

#### 变量

变量赋值的三种方式

```go
//先声明
	var x int8
	//再赋值
	x = 100

//声明并且赋值
	var y = 100

//简写形式 相当于 var z = 300
	z := 300
```

1. 同时声明多个变量

   ```go
   //一行同时声明赋值多个变量
   	var name, age = "qu", 22
   ```

2. 值拷贝

   一个变量赋值给另一个变量时，会发生值拷贝。

   ```go
   	//值拷贝
   	var a = 100
   	//一个变量赋值给另一个变量时，会发生值拷贝
   	var b = a
   	//是两个独立的内存空间，不会相互影响
   	a = 200
   	fmt.Println(b)
   ```

3. 匿名变量

   如果不使用某个变量，可以用`_`下划线代替

   ```go
   //匿名变量
   	var _, b = 100, 200
   ```

4. 变量的命名

   1. 变量名由字母数字下划线组成，且不能是数字开头。
   2. 变量名不能是保留字和关键字。
   3. 变量名区分大小写。
   4. 变量名建议使用驼峰式命名。

   **关键字**

   | break    | default     | Func   | Interface | Select |
   | -------- | ----------- | ------ | --------- | ------ |
   | case     | defer       | go     | map       | struct |
   | chan     | else        | goto   | package   | switch |
   | const    | fallthrough | if     | range     | type   |
   | continue | for         | import | return    | var    |

   **预定义字**

   ```go
   	//内建常量
   	true false iota nil
   
   	//内建类型
   	int int8 int16 int32 int64
   	uint uint8 uint16 uint32 uint64 uintptr
   	float32 float64 complex128 complex64
   	bool byte rune string error
   
   	//内建函数
   	make len cap new append copy clone delete
   	complex real imag
   	panic recover
   
   ```

5. 语句分隔符

   有两个，一个是`;`分号，另一个是换行符。

   ```go
   	//不推荐
   	//var x = 100; var y = 200
   	//或者
   	var x = 100
   	var y = 200
   ```

### 基本数据类型

#### 字节

字节(Byte)：计算机中数据存储的基本单位，缩写是B。

位(bit)：也叫作”比特“，计算机中数据存储的最小单位，因为计算机中是以二进制的形式存储数据，所以一位就代表一个”0“或”1“，缩写是b。

字节和位的关系，1字节=8位。1Byte=8bit。1B=8b。



#### 整型

1. 整型类型

   | 类型   | 取值范围                                 |
   | ------ | ---------------------------------------- |
   | int8   | -128~127                                 |
   | uint8  | 0~255                                    |
   | int16  | -32768~32767                             |
   | uint16 | 0~65535                                  |
   | int32  | -2147483648~2147483647                   |
   | uint32 | 0~4294967295                             |
   | int64  | -9223372036854775808~9223372036854775807 |
   | uint64 | 0~18446744073709551615                   |
   | uint   | 与平台相关 32位操作系统上就是uint32      |
   | int    | 与平台相关 32位操作系统上就是int32       |

#### 浮点型

```go
  //浮点型， float32精度比较低
	var f1 float32
	f1 = 0.123456789
	fmt.Println(f1)
	//打印结果0.12345679

	//float64精度比较高
	var f2 float64
	f2 = 0.1234567890123456789
	fmt.Println(f2)
	//打印结果0.12345678901234568

	//科学计数法 都是浮点类型
	var f3 = 3.4e-2
	//打印结果0.034 float64
	fmt.Println(f3, reflect.TypeOf(f3))
	var f4 = 3.4e+2
	//340 float64
	fmt.Println(f4, reflect.TypeOf(f4))
```

#### 布尔类型

布尔类型只有两个值true和false，分别代表逻辑判断中的真和假，主要应用再条件判断中。

```go
  var b bool
	b = true
	b = false
	fmt.Println(b, reflect.TypeOf(b))

	//比较运算符的结果是一个布尔值
	fmt.Println(b == true)
```

#### 字符串

字符串是 UTF-8 字符的一个序列（当字符为 ASCII 码时则占用 1 个字节，其它字符根据需要占用 2-4 个字节）。UTF-8 是被广泛使用的编码格式，是文本文件的标准编码，其它包括 XML 和 JSON 在内，也都使用该编码。由于该编码对占用字节长度的不定性，Go 中的字符串里面的字符也可能根据需要占用 1 至 4 个字节。

字符串是一种值类型，且值不可变，即创建某个文本后你无法再次修改这个文本的内容；更深入地讲，字符串是字节的定长数组，字符串再内存中是一串连续存储空间。

##### 转义符

- `\n`：换行符
- `\r`：回车符
- `\t`：tab 键
- `\u` 或 `\U`：Unicode 字符
- `\\`：反斜杠自身

```go
s := "\u0041 么么腭面 \U0001f642"
	fmt.Println(s)
```

`\u0041`和`\U0001f642`会被转义成`A`和` 🙂`。如果不想被转义可以使用 `\`，或者把字符串用反引号括起来,字符串里面的转义符就不会生效。反引号也支持字符串多行书写

```go
s := `\u0041 么么腭面 \U0001f642`
	fmt.Println(s)
```

##### 基本用法

```go
  //使用双引号
	s := "hello world"
	fmt.Println(s)

	//索引操作,获取指定下标的字符，然后把字符转化为字符串
	fmt.Println(string(s[1]))

	//切片 获取指定范围的字符串值
	fmt.Println(string(s[6:11]))
	fmt.Println(string(s[:6]))
	fmt.Println(string(s[6:]))

	//字符串拼接
	var s1 = "hello"
	var s2 = "world"
	s3 := s1 + s2
	fmt.Println(s3)

	//转义符  \n换行符  \ 使普通符号 n 特殊化
	s4 := "aaaa\nbbbb\ncccc"
	fmt.Println(s4)

	// 使用 \ 使特殊符号" 普通化
	s5 := "my name is \"zhangsan\""
	fmt.Println(s5)

	// 打印路径
	s6 := "work\\next"
	fmt.Println(s6)

	//打印多行字符串 使用反引号
	s7 := `数额哦嗯嗯额么
	么们嗯嗯
	恩`
	fmt.Println(s7)
```

##### 常用方法

```go
  s := "Hello World"

	//转成大写字符串
	sUpper := strings.ToUpper(s)
	fmt.Println(sUpper)
	//转成小写字符串
	fmt.Println(strings.ToLower(sUpper))

	//判断是否以某个字符串开头和结尾
	fmt.Println(strings.HasPrefix(s, "H"))
	fmt.Println(strings.HasSuffix(s, "World"))

	//判断是否包含某个字符串
	fmt.Println(strings.Contains(s, "llo"))

	s1 := "   lisi "
	//去除字符串前后两端的空格，也可以去除其他的字符串，根据你输入的
	//s2 := strings.TrimSpace(s1)
	s2 := strings.Trim(s1, " ")
	fmt.Println(s2)

	//索引函数
	// 找到某个子字符串在字符串中的起始位置 如果不存在返回-1
	s3 := "abc ddd eemem"
	fmt.Println(strings.Index(s3, "ddd"))

	//分隔字符串
	//按空格分隔
	//strings.Fields(s)
	s4 := "beijing,shanghai,guangzhou,shenzhen"
	//按指定字符串分隔
	datas := strings.Split(s4, ",")
	fmt.Println(datas)

	//拼接字符串
	fmt.Println(strings.Join(datas, "-"))
```

1. `strings.NewReader(str)`通过string生成reader对象。

   - `Read()` 从 `[]byte` 中读取内容。
   - `ReadByte()` 和 `ReadRune()` 从字符串中读取下一个 `byte` 或者 `rune`。

   

#### 指针

##### 注意事项

1. 不能获取字面量或者常量的地址

   ```go
   const i = 5
   ptr := &i //error: cannot take the address of i
   ptr2 := &10 //error: cannot take the address of 10
   ```

2. 对一个空指针的反向引用是不合法的，并且会使程序崩溃

   ```go
   package main
   func main() {
   	var p *int = nil
   	*p = 0
   }
   // in Windows: stops only with: <exit code="-1073741819" msg="process crashed"/>
   // runtime error: invalid memory address or nil pointer dereference
   ```

   

#### 常量

常量使用关键字`const`定义，用于存储不会改变的数据，存储在常量中的数据类型只可以是布尔型，数字型（整数型，浮点型，复数）和字符串型。

##### 定义格式

`const identifier [type] = value`

##### 举例

- 显式类型定义： `const a string = "abcde"`
- 隐式类型定义：`const a = "abcde"`

##### 注意

1. 常量的值必须是能够在编译时就能够确定的，常量赋值表达式中可以涉及计算过程，但是所有用于计算的值必须在编译期间就能获得。

   - 正确的做法：`const c1 = 2/3`
   - 错误的做法：`const c2 = customFunc()`

   赋值语句不能有自定义函数，但可以使用内置函数，比如`len()`。

#### 值类型和引用类型

##### 值类型

所有像 `int`、`float`、`bool` 和 `string` 这些基本类型都属于值类型，使用这些类型的变量直接指向存在内存中的值。另外，像`数组`和`结构体`这些复合类型也是值类型。

当使用等号 `=` 将一个变量的值赋值给另一个变量时，如：`j = i`，实际上是在内存中将 `i` 的值进行了拷贝，所以如果后面`i`的值即使修改了，也不会影响`j`值。

值类型的变量的值存储在栈中。

##### 引用类型

指针属性引用类型，其他的引用类型还包括`slice`，`map`，`channel`。被引用的变量会存储在堆中，以便进行垃圾回收，且比栈拥有更大的内存空间。

一个引用类型的变量`r1`存储的是`r1`的值所在的内存地址，当使用赋值语句`r2 = r1`时，只有内存地址被复制，这时`r2`存的值也是内存地址(这个内存地址指向`r1`的值)。如果`r1`的值被改变了，`r2`也会受到影响。

#### 随机数

```go
  //方法1
	for i := 0; i < 10; i++ {
		a := rand.Int()
		fmt.Printf("%d / ", a)
	}
	fmt.Println()
	//方法2
	for i := 0; i < 5; i++ {
		r := rand.Intn(8)
		fmt.Printf("%d / ", r)
	}
	fmt.Println()
	//方法3
	timer := int64(time.Now().Nanosecond())
	r := rand.New(rand.NewSource(timer))
	for i := 0; i < 10; i++ {
		fmt.Printf("%f / ", 100*r.Float32())
	}
	//方法4
	timens := int64(time.Now().Nanosecond())
	rand.Seed(timens)
	for i := 0; i < 10; i++ {
		fmt.Printf("%2.2f / ", 100*rand.Float32())
	}
```

函数 `rand.Intn` 返回介于 [0,n) 之间的伪随机数。

函数 `rand.Float32` 和 `rand.Float64` 返回介于 [0.0,1.0) 之间的伪随机数。

你可以使用 `rand.Seed(value)` 函数来提供伪随机数的生成种子，一般情况下都会使用当前时间的纳秒级数字。

方法4中的`rand.Seed`已经弃用了，可以使用方法3代替。

#### 字符类型(byte和rune)

严格来说，`byte`和`rune`并不是Go语言的一个类型，字符只是整数的特殊用例。`byte` 类型是 `uint8` 的别名，`rune`是`int32`的别名。

##### byte类型

`byte`代表一个ASCII编码的字符。ASCII字符集总的只有128个字符，所以可以用`byte`表示，

`byte`有多种写法

```go
  //十进制写法
	var ch byte = 65
	//16进制写法
	var ch02 byte = '\x41'
	//8进制写法
	var ch03 byte = '\101'
	//打印对应的字符 都代表 字符 'A'
	fmt.Printf("%c---%c----%c\n", ch, ch02, ch03)
```



##### rune类型

`rune`代表一个UTF-8编码的字符，UFT-8是unicode字符集的一种编码方式，当需要处理中文，日文等其他复合字符时，需要用到`rune`类型。

```go
  var ch rune = 65
	var ch02 rune = '\x41'
	var ch03 rune = '\u0041'
	var ch04 rune = '\U00101234'
	fmt.Println(ch04)

	//输出表示该字符的十进制整数
	fmt.Printf("%d----%d----%d \n", ch, ch02, ch03)
	//输出对应的字符
	fmt.Printf("%c----%c----%c \n", ch, ch02, ch03)
	//输出表示该字符的16进制整数
	fmt.Printf("%X----%X----%X \n", ch, ch02, ch03)
	//输出格式为 U+hhhh 的字符串，表示对应的unicode码点
	fmt.Printf("%U----%U----%U \n", ch, ch02, ch03)
	dd := fmt.Sprintf("%U", ch02)
	fmt.Println("dd", dd)
```



### 类型转换

```go
  //数字转换
	var x int8
	x = 100
	var y = 200
	//注意转换用精度低的转成精度高的，这样可以避免精度损失
	fmt.Println(int(x) + y)

	//字符串和数字转换
	var score = "99"
	//字符串转换成数字
	value, _ := strconv.Atoi(score)
	fmt.Println(value, reflect.TypeOf(value))
	var height = 180
	//整型数字转字符串
	s := strconv.Itoa(height)
	fmt.Println(s, reflect.TypeOf(s))
	//转成浮点类型
	value2, _ := strconv.ParseFloat("3.14", 32)
	fmt.Println(value2, reflect.TypeOf(value2))
```

### 常量

### 控制语句

#### 条件语句

1. 判断一个字符串是否为空

   ```go
   if str == "" {...}
   if len(str) == 0 {...}
   ```

2. 判断运行go程序的操作系统类型，这可以通过常量 `runtime.GOOS` 来判断

   ```go
   if runtime.GOOS == "windows"	 {
   	.	..
   } else { // Unix-like
   	.	..
   }
   ```

3. `if` 可以包含一个初始化语句（如：给一个变量赋值）。这种写法具有固定的格式（在初始化语句后方必须加上分号）。

   ```go
   val := 10
   if val > max {
   	// do something
   }
   //也可以这样写
   if val := 10; val > max {
   	// do something
   }
   ```

   但要注意的是，使用简短方式 `:=` 声明的变量的作用域只存在于 `if` 结构中。

4. `switch`语句特殊写法，不提供判断值，用来代替`if-else`。

   ```go
   package main
   
   import "fmt"
   
   func main() {
   	var num1 int = 7
   
   	switch {
   	    case num1 < 0:
   		    fmt.Println("Number is negative")
   	    case num1 > 0 && num1 < 10:
   		    fmt.Println("Number is between 0 and 10")
   	    default:
   		    fmt.Println("Number is 10 or greater")
   	}
   }
   ```

5. `switch`语句特殊写法，包含初始化语句。

   ```go
   switch a, b := x[i], y[j]; {
   	case a < b: t = -1
   	case a == b: t = 0
   	case a > b: t = 1
   }
   ```

#### 循环语句

循环语句的基本形式为

```
for 初始化语句; 条件语句; 修饰语句 {}
```

这三部分组成的循环的头部，它们之间使用分号 `;` 相隔，但并不需要括号 `()` 将它们括起来。例如：`for (i = 0; i < 10; i++) { }`，这是无效的代码！

可以在循环种同时使用多个计数器

```
for i, j := 0, 10; i < j; i, j = i+1, j-1 {}
```

注意每个语句都只能有一条语句，比如不可以

```
for i:=0, j := 0; i < j; i, j = i+1, j-1 {}
```

#### 标签与goto语句

`for`、`switch` 或 `select` 语句都可以配合标签 (label) 形式的标识符使用，即某一行第一个以冒号 (`:`) 结尾的单词（gofmt 会将后续代码自动移至下一行）。

##### continue用法

```go
package main

import "fmt"

func main() {

LABEL1:
	for i := 0; i <= 5; i++ {
		for j := 0; j <= 5; j++ {
			if j == 4 {
				continue LABEL1
			}
			fmt.Printf("i is: %d, and j is: %d\n", i, j)
		}
	}

}
```

从打印结果可以看出，这个加了标签的`continue`相当于外层的`for`的`continue`。

##### goto用法

```go
package main

func main() {
	i:=0
	HERE:
		print(i)
		i++
		if i==5 {
			return
		}
		goto HERE
}
```

使用`goto`和标签模拟循环。

**特别注意**

使用标签和 `goto` 语句是不被鼓励的：它们会很快导致非常糟糕的程序设计，而且总有更加可读的替代方案来实现相同的需求。

如果您必须使用 `goto`，应当只使用正序的标签（标签位于 `goto` 语句之后），但注意标签和 `goto` 语句之间不能出现定义新变量的语句，否则会导致编译失败。















