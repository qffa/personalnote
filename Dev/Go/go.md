# go 基础


## 预备

预备知识

1. go语言以包作为管理单位
2. 每个文件必须先声明包
3. 程序必须有一个main包
4. 左大括号不能独占一行
5. 调用函数前，需要先导入包
6. 注释方式与C相同，行注释和块注释
7. 语句不用分号结尾
8. 自动格式化
9. 一个工程有且只有一个main函数


## 变量

数据类型的作用

- 数据内存空间大小
- 数据解释方式

声明、初始化、赋值

**说明**

1. 声明格式：var 变量名 类型
2. 没有初始化的变量，默认值为0
3. 可以同时声明多个变量
4. 初始化：声明时，同时赋值
5. 初始化时，可自动推导类型 `:=`
6. 赋值前，必须先声明变量
7. 多重赋值
8. 匿名变量
9. 同时声明不同类型的变量

**格式**

```
//声明格式：var  变量名  类型
var a int       //声明
var b, c int
var d int = 10  //初始化
var e int
e = 20          //赋值：先声明后赋值

f := 30         //自动推导
fmt.Printf("f type is %T\n", f)

x, y := 10, 20  //多重赋值
tmp, _ = x, y   //匿名变量，丢弃数据不处理，一般配合函数返回值使用
```

## 常量

**说明**

1. 使用`const`关键字
2. 也可以自动推导类型

**格式**

```
const a int = 10
const b = 20     //自动推导类型，注意没有冒号
```

**同时声明不同类型变量和常量**

```
package main
import "fmt"

func main() {
    //var a int = 1
    //var b float64 = 3.14

    var (
        a int       = 1
        b float64   = 3.14
    )

    //也支持自动推导
    var (
        c = 1
        d = 2.0
    )

    fmt.Println("a = ", a)
    fmt.Println("b = ", b)

    fmt.Println("c = ", c)
    fmt.Println("d = ", d)

    const (
        i int       = 10
        j float64   = 3.14
    )

    //自动推导
    const (
        p = 1
        q = 2.0
    )

    fmt.Println("i = ", i)
    fmt.Println("j = ", j)

    fmt.Println("p = ", p)
    fmt.Println("q = ", q)
}
```

## iota枚举

**说明**

1. 常量自动生成器
2. 每一行自动累加1
3. 遇到`const`，重置为0

**格式**

```
package main
import "fmt"


func main () {
    const (
        a = iota    // 0
        b = iota    // 1
        c = iota    // 2
    )
    fmt.Printf("a = %d, b = %d, c = %d\n", a, b, c)

    //只写一行iota也行
    const (         //将iota重置为0
        a1 = iota   // 0
        a2          // 1
        a3          // 2
    )
    fmt.Printf("a1 = %d, a2 = %d, a3 = %d\n", a1, a2, a3)

    const (
        i           = iota
        j1, j2, j3  = iota, iota, iota // 值都是1
        k           = iota
    )
    fmt.Printf("i = %d, j1 = %d, j2 = %d, j3 = %d, k = %d\n",
        i, j1, j2, j3, k
    )
}
```

## go数据类型分类

- 简单类型
- 符合类型

**简单类型**

| type          | Length   | Zero  | Description  |
|:------------- |:-------- | ----- | ------------ |
| bool          | 1        | false |              |
| byte          | 1        | 0     | uint8 alias  |
| rune          | 4        | 0     | for unicode  |
| int, uint     | 4 or 8   | 0     |              |
| int8, uint8   | 1        | 0     |              |
| int16, uint16 | 2        | 0     |              |
| int32, uint32 | 4        | 0     |              |
| int64, uint64 | 8        | 0     |              |
| float32       | 4        | 0.0   | 7            |
| float64       | 8        | 0.0   | 15           |
| complex64     | 8        |       |              |
| complex128    | 16       |       |              |
| uintptr       | 4 or 8   |       | pointor      |
| string        | variable | ""    | utf-8 string |



**bool**

```go
package main
import "fmt"

func main() {
    var a bool
    a = true
    fmt.Println("a = ", a)

    //自动推导类型
    var a2 = false
    fmt.Println("a2 = ", a2)

    a3 := false
    fmt.Println("a3 = ", a3)

    //默认值
    var a4 bool
    fmt.Println("a4 = ", a4)
}
```

**byte**

```
var ch byte
ch = 97
fmt.Println("ch = ", ch)
fmt.Printf("%d is %c\n", ch, ch)

ch = 'a'
fmt.Printf("%d is %c\n", ch, ch)
```

*大写小写字符相差32，小写大*

**string**

字符串以`\0`结尾

```
package main
import "fmt"

func main() {
    var str1 string
    str1 = "abc"
    fmt.Println("str1: ", str1)

    str2 := "xyz"
    fmt.Printf("str2 type: %T", str2)

    fmt.Println("str2 len: ", len(str2))
    fmt.Printf("str2[0] = %c, str2[1] = %c, str2[2] = %c\n",
    str2[0], str2[1], str2[2])
}
```

**complex**

```
package main
import "fmt"

func main() {
    var t complex128
    t = 2.1 + 3.14i
    fmt.Println("t = ", t)

    t2 := 2.2 + 3.3i
    fmt.Printf("t2 type: %T\n", t2)

    fmt.Println("real(t2) = ", real(t2), "imag(t2) = ", imag(t2))
}
```

## 格式化输出输入

**常用格式**

```go
package main
import "fmt"

func main () {
    a := 10
    b := "abc"
    c := 'a'
    d := 3.14

    fmt.Printf("%T, %T, %T, %T\n", a, b, c, d)
    fmt.Printf("a = %d, b = %s, c = %c, d = %f\n", a, b, c, d)
    //自动匹配格式%v
    fmt.Printf("a = %v, b = %v, c = %v, d = %v\n", a, b, c, d)
}
```

**输入**

```go
package main
import "fmt"

func main() {
    var a int
    fmt.Printf("请输入变量a: ")

    //阻塞等待用户输入
    fmt.Scanf("%d", &a)
    //fmt.Scan(&a) 也可以
    fmt.Printf("a = %d\n", a)
}
```

## 类型转换

*兼容类型才可以转换*

- bool类型与整形之间不可以互转
- byte可以转成整形, `var ch byte` `var t int = int(ch)`

## 类型别名

```go
func main() {
  type bigint int64
  var a bigint
  fmt.Printf("a type: %T\n", a)

  type (
    long int64
    char byte
  )
}
```

## 运算符

| Type       | Operators                           |
|:---------- |:----------------------------------- |
| arithmetic | `+`, `-`, `*`, `/`, `%`, `++`, `--` |
| relational | `==`, `!=`, `<`, `>`, `<=`, `>=`    |
| logical    | `!`, `&&`, `||`                     |
| bit        | `&`, `|`, `^`, `<<`, `>>`           |
| assignment | `=`, `+=`, `&=` ...                 |
| ptr        | `*`, `&`                            |

*非零为真*

运算符优先级：7级

## 流程控制

**分支**


if else

```go
package main
import "fmt"

func main() {
    s := "boss"
    if s == "boss" {
        fmt.Println("no need to code")
    }

    //if支持一个初始化语句
    if a := 10; a == 10 {
        fmt.Println("a == 10")
    }

    //多分枝
    a := 20
    if a == 10 {
        fmt.Println("a == 10")
    } else {
        fmt.Println("a != 10")
    }

    if a := 10; a == 10 {
        fmt.Println("a == 10")
    } else if a > 10 {
        fmt.Println("a > 10")
    } else if a < 10 {
        fmt.Println("a < 10")
    } else {
        fmt.Println("not possible")
    }
}
```

switch

```go
package main
import "fmt"

func main() {
    num := 1
    fmt.Printf("select: ")
    fmt.Scanf("%d", &num)

    switch num {
    case 1:
        fmt.Println("select 1")
        break   //break是默认的，不用写
        //关闭默认的break行为
        //fallthrough
    case 2:
        fmt.Println("select 2")
    case 3:
        fmt.Println("select 3")
    case 4:
        fmt.Println("select 4")
    default:
        fmt.Println("select 1")
    }
}
```

**循环**

*go只有for循环*

语句

- for
- range

跳转

- break
- continue
- goto
