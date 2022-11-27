# Hello，World

> ```go
> package main
> 
> import "fmt"
> 
> func main() {
>     fmt.Println("Hello, World")
> }
> ```

一个Go语言编写的程序对应一个或多个以.go为文件后缀名的源文件。

* 每个源文件中以包的声明语句开始，说明该源文件是属于哪个包
* 包声明语句之后是import语句导入依赖的其它包
* 然后是包一级的类型、变量、常量、函数的声明语句



# 第二章　程序结构



## 2.1.命名

1. 名字以字母或下划线开始
2. 区分大小写
3. 关键字

> ```
> break      default       func     interface   select
> case       defer         go       map         struct
> chan       else          goto     package     switch
> const      fallthrough   if       range       type
> continue   for           import   return      var
> ```

4. 名字的开头字母的大小写决定了名字在包外的可见性

* 如果一个名字是大写字母开头,那么它将是导出的，也就是说可以被外部的包访问
* 包本身的名字一般总是用小写字母

5. 驼峰式命名



## 2.2.声明

Go语言主要有四种类型的声明语句：==var、const、type和func==，分别对应==变量、常量、类型和函数实体对象==的声明。



## 2.3.变量

1. 变量声明的一般语法：

```Go
var 变量名字 类型 = 表达式
```

​	其中类型和表达式可以省略其中一个。

* 省略类型时，根据表达式初始化推导类型、
* 省略表达式时，初始化为变量对应的零值

2. 可以在一个声明语句中同时声明一组变量

```Go
var i, j, k int                 // int, int, int
var b, f, s = true, 2.3, "four" // bool, float64, string
```

### 2.3.1简短变量声明

在函数内部声明和初始化局部变量，形式是==“名字 := 表达式”==，如：

```Go
anim := gif.GIF{LoopCount: nframes}
freq := rand.Float64() * 3.0
t := 0.0
```

也可以用来声明和初始化一组变量：

> ```Go
> i, j := 0, 1
> ```

* 简短变量声明语句中必须至少要声明一个新的变量

### 2.3.2. 指针

如果用“var x int”声明语句声明一个x变量，

那么==&x表达式==（取x变量的内存地址）将产生一个指向该整数变量的指针，

指针对应的数据类型是`*int`，即“指向int类型的指针”。

* 在Go语言中，返回函数中局部变量的地址是安全的，如

```Go
var p = f()

func f() *int {
    v := 1
    return &v
}
```

* 每次调用f函数都将返回不同的结果：

```Go
fmt.Println(f() == f()) // "false"
```

### 2.3.3.new函数

表达式==new(T)==将创建一个T类型的匿名变量，初始化为T类型的零值，然后返回变量地址，返回的指针类型为`*T`。

```Go
p := new(int)   // p, *int 类型, 指向匿名的 int 变量
fmt.Println(*p) // "0"
*p = 2          // 设置 int 匿名变量的值为 2
fmt.Println(*p) // "2"
```

* 每次调用new函数都是返回一个新的变量的地址
* new不是关键字，是预定义函数，可以重新定义为别的类型，此时new函数不可用

### 2.3.4. 变量的生命周期

1. Go语言的**自动垃圾收集器**如何知道一个变量是何时可以被回收？从每个包级的变量和每个当前运行函数的每一个局部变量开始，**通过指针或引用的访问路径遍历**，是否可以找到该变量
2. 编译器会**自动选择**在栈上还是在堆上分配局部变量的存储空间,==不由var和new决定==
3. **不需**为了编写正确的代码而要**考虑变量的逃逸行为**，只不过逃逸的变量需要额外分配内存，同时**对性能的优化**可能会产生细微的影响
4. 将指向短生命周期对象的指针保存到具有长生命周期的对象中，会阻止对短生命周期对象的**垃圾回收**



## 2.4. 赋值

简单的赋值语句：

```go
x = 1                       // 命名变量的赋值
*p = true                   // 通过指针间接赋值
person.name = "bob"         // 结构体字段赋值
count[x] = count[x] * scale // 数组、slice或map的元素赋值
count[x] *= scale			//效果等同第4行
```

++是语句

* 支持v++
* 不支持++v
* 不支持  x = i++

### 2.4.1. 元组赋值

即允许同时更新多个变量的值，例如交换两个变量的值：
```go
x, y = y, x
a[i], a[j] = a[j], a[i]
```

**可以用下划线空白标识符`_`来丢弃不需要的值**

### 2.4.2. 可赋值性

隐式的赋值行为,例如:

```Go
medals := []string{"gold", "silver", "bronze"}
```

隐式地对slice的每个元素进行赋值操作，类似这样写的行为：

```Go
medals[0] = "gold"
medals[1] = "silver"
medals[2] = "bronze"
```

只有右边的值对于左边的变量是可赋值的，赋值语句才是允许的



## 2.5. 类型

```Go
type 类型名字 底层类型
```

类型转换：

* 对于每一个类型T，都有一个对应的类型转换操作T(x)，用于将x转为T类型
* 有当两个类型的底层基础类型相同时，才允许这种转型操作



## 2.6. 包和文件

没看懂，回头再学



# 第三章　基础数据类型

## 3.1. 整型

四种有符号整数类型：`int8、int16、int32和int64`

四种无符号整数类型：`uint8、uint16、uint32和uint64`

* Unicode字符`rune`类型是和`int32`等价的类型
* `byte`也是`uint8`类型的等价类型
* 无符号整数类型`unitptr`，没有具体bit大小，容纳指针
* `int、uint和uintptr`是不同类型的兄弟类型



算术运算、逻辑运算和比较运算的二元运算符（优先级递减）

```
*      /      %      <<       >>     &       &^
+      -      |      ^
==     !=     <      <=       >      >=
&&
||
```

* 取模运算符%仅用于整数间的运算

* 除法运算符`/`的行为则依赖于操作数是否全为整数，比如`5.0/4.0`的结果是1.25，但是5/4的结果是1



## 3.2. 浮点数

两种精度的浮点数，`float32和float64`



## 3.3. 复数

Go语言提供了两种精度的复数类型：`complex64和complex128`，分别对应`float32和float64`两种浮点数精度。

内置的complex函数用于构建复数，内建的real和imag函数分别返回复数的实部和虚部：

```Go
var x complex128 = complex(1, 2) // 1+2i
var y complex128 = complex(3, 4) // 3+4i
fmt.Println(x*y)                 // "(-5+10i)"
fmt.Println(real(x*y))           // "-5"
fmt.Println(imag(x*y))           // "10"
```

等效于

```Go
x := 1 + 2i
y := 3 + 4i
```



## 3.4. 布尔型

一个布尔类型的值只有两种：true和false



## 3.5. 字符串

内置的`len`函数可以返回一个字符串中的字节数目

```Go
s := "hello, world"
fmt.Println(len(s))     // "12"
fmt.Println(s[0], s[7]) // "104 119" ('h' and 'w')
```

### 3.5.1. 字符串面值

### 3.5.2. Unicode

### 3.5.3. UTF-8

### 3.5.4. 字符串和Byte切片

标准库中有四个包对字符串处理尤为重要：bytes、strings、strconv和unicode包。

* strings包提供了许多如字符串的查询、替换、比较、截断、拆分和合并等功能
* 提供了很多类似功能的函数bytes包针对和字符串有着相同结构的[]byte类型
* strconv包提供了布尔型、整型数、浮点数和对应字符串的相互转换，还提供了双引号转义相关的转换
* unicode包提供了IsDigit、IsLetter、IsUpper和IsLower等类似功能，它们用于给字符分类

### 3.5.5. 字符串和数字的转换

* 用`fmt.Sprintf`返回一个格式化的字符串
* 用`strconv.Itoa`(“整数到ASCII”)

```Go
x := 123
y := fmt.Sprintf("%d", x)
fmt.Println(y, strconv.Itoa(x)) // "123 123"
```

* `FormatInt`和`FormatUint`函数可以用不同的进制来格式化数字：

```Go
fmt.Println(strconv.FormatInt(int64(x), 2)) // "1111011"
```

* `fmt.Printf`函数的%b、%d、%o和%x等参数

```Go
s := fmt.Sprintf("x=%b", x) // "x=1111011"
```

* 如果要将一个字符串解析为整数，可以使用`strconv`包的`Atoi`或`ParseInt`函数，还有用于解析无符号整数的`ParseUint`函数：

```Go
x, err := strconv.Atoi("123")             // x is an int
y, err := strconv.ParseInt("123", 10, 64) // base 10, up to 64 bits
```



## 3.6. 常量

```Go
const pi = 3.14159 // approximately; math.Pi is a better approximation
```

* 可以批量声明多个常量

```Go
const (
    a = 1
    b
    c = 2
    d
)

fmt.Println(a, b, c, d) // "1 1 2 2"
```

### 3.6.1. iota 常量生成器

```Go
const (
    _ = 1 << (10 * iota)
    KiB // 1024
    MiB // 1048576
    GiB // 1073741824
    TiB // 1099511627776             (exceeds 1 << 32)
    PiB // 1125899906842624
    EiB // 1152921504606846976
    ZiB // 1180591620717411303424    (exceeds 1 << 64)
    YiB // 1208925819614629174706176
)
```

### 3.6.2. 无类型常量





# 第四章　复合数据类型

## 4.1. 数组

初始化数组

```Go
var q [3]int = [3]int{1, 2, 3}
var r [3]int = [3]int{1, 2}
fmt.Println(r[2]) // "0"
```

```Go
q := [...]int{1, 2, 3}
fmt.Printf("%T\n", q) // "[3]int"
```



## 4.2. Slice

一个slice类型一般写作[]T，其中T代表slice中元素的类型；slice的语法和数组很像，只是没有固定长度而已。

一个slice由三个部分构成：==指针、长度和容量==。

* 指针指向第一个slice元素对应的底层数组元素的地址
* 长度对应slice中元素的数目
* 容量一般是从slice的开始位置到底层数据的结尾位置

内置的len和cap函数分别返回slice的长度和容量。

如果切片操作超出cap(s)的上限将导致一个panic异常，但是超出len(s)则是意味着扩展了slice，因为新slice的长度会变大。

和数组不同的是，slice之间不能比较。bytes.Equal函数来判断两个字节型slice是否相等。

### 4.2.1. append函数

内置的append函数用于向slice追加元素。



## 4.3. Map

一个map就是一个哈希表的引用

好多看不懂，做个标记，统计到“回头再学”



## 4.4. 结构体

结构体是一种聚合的数据类型，是由零个或多个任意类型的值聚合成的实体。

```Go
type Employee struct {
    ID        int
    Name      string
    Address   string
    DoB       time.Time
    Position  string
    Salary    int
    ManagerID int
}

var dilbert Employee
```

### 4.4.1. 结构体字面值

结构体值也可以用结构体字面值表示，结构体字面值可以指定每个成员的值。

* 第一种写法，要求以结构体成员定义的顺序为每个结构体成员指定一个字面值

```Go
type Point struct{ X, Y int }

p := Point{1, 2}
```

* 第二种写法，以成员名字和相应的值来初始化，可以包含部分或全部的成员

```Go
anim := gif.GIF{LoopCount: nframes}
```

### 4.4.2. 结构体比较

下面两个比较的表达式是等价的

```Go
type Point struct{ X, Y int }

p := Point{1, 2}
q := Point{2, 1}
fmt.Println(p.X == q.X && p.Y == q.Y) // "false"
fmt.Println(p == q)                   // "false"
```

### 4.4.3. 结构体嵌入和匿名成员



## 4.5. JSON

## 4.6. 文本和HTML模板





# 第五章　函数

## 5.1. 函数声明

函数声明包括函数名、形式参数列表、返回值列表（可省略）以及函数体。

```Go
func name(parameter-list) (result-list) {
    body
}
```
