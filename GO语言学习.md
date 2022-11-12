# GO语言学习



## Hello，World

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



## 程序结构

### 命名

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

### 声明

Go语言主要有四种类型的声明语句：==var、const、type和func==，分别对应==变量、常量、类型和函数实体对象==的声明。

#### 变量

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

##### 简短变量声明

在函数内部声明和初始化局部变量，形式是==“名字 := 表达式”==，如：

```Go
anim := gif.GIF{LoopCount: nframes}
freq := rand.Float64() * 3.0
t := 0.0
```