# Go Lang学习

看2020的课程表，做2023的lab

# 包、变量与函数

### 包

每个 Go 程序都由包构成。

程序从 `main` 包开始运行。

本程序通过导入路径 `"fmt"` 和 `"math/rand"` 来使用这两个包。

按照约定，**包名与导入路径的最后一个元素一致**。例如，`"math/rand"` 包中的源码均以 `package rand` 语句开始。操作也是rand.xxx

```go
package main

import (
	"fmt"
	"math/rand"
    //记得是圆括号，这是“分组”形式导入语句
)

func main() {
	fmt.Println("我最喜欢的数字是 ", rand.Intn(10))
    //Println中隔一个,就会加一个空格，故这里有两个空格
}
```

fmt.Println 等待所有参数的计算结果，然后再一次性打印输出，所以如果参数是一个会打印东西的函数，那么先打印出参数的结果，再打印别的，具体可以见if判断那一节。



### 导出名

在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的。例如，`Pizza` 就是个已导出名，`Pi` 也同样，它导出自 `math` 包。

`pizza` 和 `pi` 并未以大写字母开头，所以它们是未导出的。

**在导入一个包时，你只能引用其中已导出的名字，而这些名字都是大写开头的！** 任何「未导出」的名字在该包外均无法访问。

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.pi)
}
//错误，因为pi(π)是math里面的名字，要引用这个名字，要把pi改成Pi，像上一个代码块，就用了rand.Intn(),包不用大写，但包中的名字要大写
```



### 函数

注意类型在变量名的 **后面**。调用方法和cpp一样。

（参考这篇关于 [Go 声明语法](http://blog.go-zh.org/gos-declaration-syntax) 的文章，了解为何使用这种类型声明的形式。）

```go
func add(x int, y int) int {
	return x + y;
}
//其中，由于xy都是int，所以可以写成x,y int
```

函数可以返回任意数量的返回值。(与cpp不同)

```go
func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")//由于在此之前并没有声明变量如var a，b string,所以此处用:=有初始化并赋值的意思，具体见变量
	fmt.Println(a, b)
}
```



没有参数的 `return` 语句会直接返回已命名的返回值，也就是「裸」返回值。

裸返回语句应当仅用在下面这样的短函数中。在长的函数中它们会影响代码的可读性。

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
//7 10 
```



### 变量

`var` 语句用于声明一系列变量。和函数的参数列表一样，类型在最后。

如例中所示，`var` 语句可以出现在包或函数的层级。反正出现了类型就要配合var或者下面的const。

```go
var c, python, java bool

func main() {
    var h int
	var i int = 1
	fmt.Println(h, i, c, python, java)//0 1 false false false
}
```

变量声明可以包含初始值，每个变量对应一个。

如果提供了初始值，则类型可以省略；变量会从初始值中推断出类型。

```go
var c, python, java = true, false, "no!"//此时不用写bool或者string
```

在函数中，短赋值语句 `:=` 可在隐式确定类型的 `var` 声明中使用。

**函数外的每个语句都 必须 以关键字开始（`var`、`func` 等），因此 `:=` 结构不能在函数外使用**。

```go
package main

// 正确示例：在函数外部使用显式的var声明
var x = 10
var z int =20
//错误示例
//x := 10

func main() {
    // 在函数内部使用短变量声明
    y := 20
    i := y//当y的类型确定时，可以用:=传给新变量来确定新变量的类型
    println(x, y, i)
}
```



### 常量

常量的声明与变量类似，只不过使用 `const` 关键字。

常量可以是字符、字符串、布尔值或数值。

**常量不能用 `:=` 语法声明。**



### 基本类型

Go 的基本类型有（留意一下）

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
//int 类型可以存储最大 64 位的整数，根据平台不同有时会更小。
byte // uint8 的别名

rune // int32 的别名
     // 表示一个 Unicode 码位

float32 float64

complex64 complex128//复数
```

当你需要一个整数值时应使用 `int` 类型， 除非你有特殊的理由使用固定大小或无符号的整数类型。

和导入语句（import）一样，变量声明也可以「分组」成一个代码块。

```go
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1//此处不能省略uint64，因为超出了int的限制
	z      complex128 = cmplx.Sqrt(-5 + 12i)//此处如果想表达i，不能单独用i，否则会被判断成一个变量，要用1i
)

func main() {
	fmt.Printf("类型：%T 值：%v\n", ToBe, ToBe)
	fmt.Printf("类型：%T 值：%v\n", MaxInt, MaxInt)
	fmt.Printf("类型：%T 值：%v\n", z, z)
}
//类型：bool 值：false
//类型：uint64 值：18446744073709551615
//类型：complex128 值：(2+3i)
```

没有明确初始化的变量声明会被赋予对应类型的 **零值**。

零值是：

- 数值类型为 `0`，
- 布尔类型为 `false`，
- 字符串为 `""`（空字符串）。



表达式 `T(v)` 将值 `v` 转换为类型 `T`。%T和%v用于占位符，表示类型与值(最通用的一种)。

```go
package main

import (
    "fmt"
)

func main() {
    // 通用占位符
    fmt.Printf("默认格式：%v\n", 123)          // 输出：123
    fmt.Printf("Go 语法格式：%#v\n", 123)     // 输出：123
    fmt.Printf("类型：%T\n", 123)             // 输出：int

    // 布尔类型
    fmt.Printf("布尔值：%t\n", true)          // 输出：true

    // 整数类型
    fmt.Printf("二进制：%b\n", 123)           // 输出：1111011
    fmt.Printf("字符：%c\n", 123)             // 输出：{
    fmt.Printf("十进制：%d\n", 123)           // 输出：123
    fmt.Printf("八进制：%o\n", 123)           // 输出：173
    fmt.Printf("十六进制：%x\n", 123)         // 输出：7b
    fmt.Printf("十六进制（大写）：%X\n", 123) // 输出：7B

    // 浮点数
    fmt.Printf("科学计数法：%e\n", 123.456)   // 输出：1.234560e+02
    fmt.Printf("科学计数法（大写）：%E\n", 123.456) // 输出：1.234560E+02
    fmt.Printf("小数点：%f\n", 123.456)       // 输出：123.456000
    fmt.Printf("根据情况选择：%g\n", 123.456) // 输出：123.456
    fmt.Printf("根据情况选择（大写）：%G\n", 123.456) // 输出：123.456
//%g 会根据数值的大小和精度自动选择使用 %e（科学计数法）或者 %f（小数点表示法）来显示。
    
    // 字符串和字节切片
    fmt.Printf("字符串：%s\n", "Hello, Go!")   // 输出：Hello, Go!
    fmt.Printf("带引号字符串：%q\n", "Hello, Go!") // 输出："Hello, Go!"
    fmt.Printf("十六进制：%x\n", "Hello")      // 输出：48656c6c6f
    fmt.Printf("十六进制（大写）：%X\n", "Hello") // 输出：48656C6C6F

    // 指针
    fmt.Printf("指针：%p\n", &main)           // 输出：0x地址
}

```



一些数值类型的转换：

```
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

或者，更加简短的形式（当然这要在函数里面使用）：

```
i := 42
f := float64(i)
u := uint(f)
```

与 C 不同的是，Go 在不同类型的项之间赋值时需要显式转换。





# 流程控制语句:for、if、else、switch和 defer

### for循环

Go 只有一种循环结构：`for` 循环。

基本的 `for` 循环由三部分组成，它们用分号隔开：

- 初始化语句：在第一次迭代前执行
- 条件表达式：在每次迭代前求值
- 后置语句：在每次迭代的结尾执行

初始化语句通常为一句短变量声明，该变量声明仅在 `for` 语句的作用域中可见。

一旦条件表达式求值为 `false`，循环迭代就会终止。

**注意**：和 C、Java、JavaScript 之类的语言不同，Go 的 `for` 语句后面的三个构成部分外没有小括号， 大括号 `{ }` 则是必须的。

```go
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}//注意不能var i int =0，和cpp一样初始化语句和后置语句是可选的。此时就像while循环
for sum < 1000 {
		sum += sum
	}
```

如果省略循环条件，该循环就不会结束(即while(1))，因此无限循环可以写得很紧凑。



### if判断

Go 的 `if` 语句与 `for` 循环类似，表达式外无需小括号 `( )`，而大括号 `{ }` 则是必须的。

```go
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))//将参数格式化为字符串并返回。用途：用于将多个变量拼接成一个字符串，而不是直接输出到控制台,Println是输出到控制台
}
```

for与if一样，以这个代码为例子，x的作用域只存在于if语句内。

和 `for` 一样，**`if` 语句可以在条件表达式前执行一个简短语句**。

```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}//计算 x 的 n 次幂，并将结果赋值给 v
//如果 v 小于 lim，则返回 v
//如果 v 不小于 lim，则返回 lim

another example
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}/*注意输出为
27 >= 20
9 20
因为 fmt.Println 等待所有参数的计算结果，然后再一次性打印输出。所以在第二个函数计算过程中调用了 fmt.Printf，先打印了 27 >= 20。随后 fmt.Println 才打印 9 20
*/
```



### switch分支

Go 的 `switch` 语句类似于 C、C++、Java、JavaScript 和 PHP 中的，不过 Go 只会运行选定的 `case`，而非之后所有的 `case`。 在效果上，**Go 的做法相当于这些语言中为每个 `case` 后面自动添加了所需的 `break` 语句**。在 Go 中，除非以 `fallthrough` 语句结束，否则分支会自动终止。 Go 的另一点重要的不同在于 `switch` 的 `case` 无需为常量，且取值不限于整数。

```c
switch (day) {
	case 3:
		printf("今天是周三\n");
            // 没有 break，继续执行下一个 case
    case 4:
        printf("今天是周四\n");
        break; // 
}/*如果 day 的值为 3，上述代码的输出将是：
今天是周三
今天是周四*/
```

上面是C语言风格的switch，下面是go语言

```go
func main() {
	fmt.Print("Go 运行的系统环境：")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("macOS.")//会跳出这个switch语句
	case "linux":
		fmt.Println("Linux.")
		fallthrough//还会走到下面的default
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", "windows")
	}
}
```



无条件的 `switch` 同 `switch true` 一样。

这种形式能将一长串 `if-then-else` 写得更加清晰。

```go
func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("早上好！")
	case t.Hour() < 17:
		fmt.Println("下午好！")
	default:
		fmt.Println("晚上好！")
	}
}
```



### defer推迟

**defer 语句会将函数推迟到外层函数返回之后执行。**

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。推迟调用的函数调用会被压入一个栈中。 当外层函数返回时，被推迟的调用会按照后进先出的顺序调用。

更多关于 defer 语句的信息，请阅读[此博文](http://blog.go-zh.org/defer-panic-and-recover)。

```go
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
/*
counting
done
9
8
7...
*/
```



# 结构体、切片和映射

###　指针

Go 拥有指针。指针保存了值的内存地址。

类型 `*T` 是指向 `T` 类型值的指针，其零值为 `nil`。

```
var p *int
```

`&` 操作符会生成一个指向其操作数的指针。

```
i := 42
p = &i
```

`*` 操作符表示指针指向的底层值。

```
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```

这也就是通常所说的「解引用」或「间接引用」。

与 C 不同，Go 没有指针运算。



### 结构体

一个 结构体（`struct`）就是一组字段(field)。

结构体字段可通过点号 `.` 来访问。

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}//赋值方式实例化一个对象
    a := Vertex{}//只是实例化一个对象
    v2 = Vertex{X: 1}  // Y:0 被隐式地赋予零值
    p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针），完全不需要像c语言那样
    //struct Vertex *p = (struct Vertex *)malloc(sizeof(struct Vertex))
    //btw：为什么不用&，而int a =10；int *p = &a;中要用&，因为malloc已经返回了一个地址，所以并不需要&来获取地址
	v.X = 4//实例化一个对象的方式修改其中的X值
	fmt.Println(v.X)//4
    fmt.Println(a.X)//0
    fmt.Println(p)//&{1 2}  记得有个&符号！！！
}

s := struct {
		i int
		b bool
}//也可以内联(匿名)结构体，然后用s.i这样子来引用
```

结构体字段可通过结构体指针来访问。

**结构体里面可以包含接口**

```go
type UppercaseReader struct {
    reader io.Reader
}
```

- 该结构体包含一个字段 `reader`，类型是 `io.Reader` 接口。
- 这个字段用于保存被包装的 `Reader`。

如果我们有一个指向结构体的指针 `p` 那么可以通过 `(*p).X` 来访问其字段 `X`。 不过这么写太啰嗦了，所以语言也允许我们使用隐式解引用，直接写 `p.X` 就可以。

```go
p :=&v//获取其指针，通过指针的解引用的方式去修改其值，相当于c中的->
p.X = 10
```



###　数组与切片

类型 `[n]T` 表示一个数组，它拥有 `n` 个类型为 `T` 的值。

表达式

```
var a [10]int
```

会将变量 `a` 声明为拥有 10 个整数的数组。是长度！

数组的长度是其类型的一部分，因此数组不能改变大小。 这看起来是个限制，不过没关系，Go 拥有更加方便的使用数组的方式。

类型 `[]T` 表示一个元素类型为 `T` 的切片。.

切片通过两个下标来界定，一个下界和一个上界，二者以冒号分隔：

```go
a[low : high]//a为数组名
```

它会选出一个半闭半开区间，包括第一个元素，但排除最后一个元素。

```go
func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}  //另一种初始化的方式，即声明并初始化
	fmt.Println(primes)
}/*Hello World
[Hello World]
[2 3 5 7 11 13]打印数组名会自动带个[]
*/

var s []int = primes[1:4]
fmt.Println(s)//[3 5 7]
```

切片就像数组的引用 切片并不存储任何数据，它只是描述了底层数组中的一段。

**更改切片的元素会修改其底层数组中对应的元素**(**要看好下标！**切片的元素下标和底层数组下标可能是不一样的!)。

和它共享底层数组的切片都会观测到这些修改。

```go
func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}/*
[John Paul George Ringo]
[John Paul] [Paul George]
[John XXX] [XXX George]
[John XXX George Ringo]
这是易错点！！！别忘了会修改原始的还有别的切片
*/
```



切片字面量类似于没有长度的数组字面量。

这是一个数组字面量：

```go
[3]bool{true, true, false}
```

下面这样则会创建一个和上面相同的数组，然后再构建一个引用了它的切片：

```go
[]bool{true, true, false}
```



```go
s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
/*[{2 true} {3 false} {5 true} {7 true} {11 false} {13 true}]*/数组打印自动带上[]
```

定义并初始化了一个结构体切片，可以通过s[index].field来修改元素



在进行切片时，你可以利用它的默认行为来忽略上下界。

切片下界的默认值为 0，上界则是该切片的长度。

对于数组

```
var a [10]int
```

来说，以下切片表达式和它是等价的：

```
a[0:10]
a[:10]
a[0:]
a[:]
```

```go
func main() {
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)

	s = s[:2]
	fmt.Println(s)

	s = s[1:]
	fmt.Println(s)
}/*[3 5 7]
[3 5]
[5]
这是易错点，每次切片都直接对s进行了修改*/
```

切片拥有 **长度** 和 **容量**。

切片的长度就是它所包含的元素个数。

切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

切片 `s` 的长度和容量可通过表达式 `len(s)` 和 `cap(s)` 来获取。

你可以通过重新切片(比如说从`s = s[:2]`变成`s = s[:4]`)来扩展一个切片，给它提供足够的容量。 

切片的零值是 `nil`。

nil 切片的长度和容量为 0 且没有底层数组。



切片可以用内置函数 `make` 来创建，这也是你创建**动态数组**的方式。即不用先定义一个数组，再通过数组名[low,high]来定义一个切片了，记得用某个东西去接收如`var s []T`，函数内部就如下方如此操作即可。

`make` 函数会分配一个元素为零值的数组并返回一个引用了它的切片：

```
a := make([]int, 5)  // len(a)=5
```

要指定它的容量，需向 `make` 传入第三个参数：

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```



切片可以包含任何类型，当然也包括其他切片。下面就是一个切片嵌套切片的例子。

```go
matrix := [][]uint8{
        {1, 0, 0, 0},
        {0, 2, 0, 0},
        {0, 0, 3, 0},
    }
```

**当说到外层切片的元素时，我们讨论的是每一行的整个切片 (例如 `[1, 0, 0, 0]`)；当说到内层切片的元素时，我们讨论的是每一行中的单个值 (例如 `1` 或 `0`)。外层切片长度为3，内层切片长度为4。**

```go
func main() {
	// 创建一个井字棋（经典游戏）
	board := [][]string{
		[]string{"_", "_", "_"},//这是[][]string切片中的其中一个元素，而不是_这个为一个元素
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}
    //可以打印单行数组的，就Println(board[0])即可
	// 两个玩家轮流打上 X 和 O
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
```



为切片追加新的元素是种常见的操作，为此 Go 提供了内置的 `append` 函数。内置函数的[文档](https://tour.go-zh.org/pkg/builtin/#append)对该函数有详细的介绍。

```
func append(s []T, vs ...T) []T
```

`append` 的第一个参数 `s` 是一个元素类型为 `T` 的切片，其余类型为 `T` 的值将会追加到该切片的末尾。

`append` 的结果是一个包含原切片所有元素加上新添加元素的切片。

当 `s` 的底层数组太小，不足以容纳所有给定的值时，它就会分配一个更大的数组。 返回的切片会指向这个新分配的数组。

（要了解关于切片的更多内容，请阅读文章 [Go 切片：用法和本质](https://tour.go-zh.org/blog/go-slices-usage-and-internals)。）

```go
func main() {
	var s []int
	printSlice(s)

	// 可在空切片上追加
	s = append(s, 0)
	printSlice(s)

	// 这个切片会按需增长
	s = append(s, 1)
	printSlice(s)

	// 可以一次性添加多个元素
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
/*
len=0 cap=0 []
len=1 cap=1 [0]
len=2 cap=2 [0 1]
len=5 cap=6 [0 1 2 3 4]
*/
```



**`for` 循环的 `range` 形式可遍历切片或映射。**

当使用 `for` 循环遍历切片时，**每次迭代都会返回两个值**(所以要用两个参数去接受)。 第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

可以将下标或值赋予 `_` 来忽略它。

```
for i, _ := range pow
for _, value := range pow
```

**若你只需要索引，忽略第二个变量即可。**

```go
for i := range pow
```

```go
func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```



**练习**

实现 `Pic`。它应当返回一个长度为 `dy` 的切片（即有dy行），其中每个元素是一个长度为 `dx`（即有dx列）元素类型为 `uint8` 的切片。当你运行此程序时，它会将每个整数解释为灰度值 （好吧，其实是蓝度值）并显示它所对应的图像。

图像的解析式由你来定。几个有趣的函数包括 `(x+y)/2`、`x*y`、`x^y`、`x*log(y)` 和 `x%(y+1)`。

（提示：需要使用循环来分配 `[][]uint8` 中的每个 `[]uint8`。）

（请使用 `uint8(intValue)` 在类型之间转换；你可能会用到 `math` 包中的函数。）

```go
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	pow := make([][]uint8,dy)
	for y:=0; y < dy; y++{
		row :=make([]uint8,dx)
		for x:=0; x < dx; x++{
			row[x] = uint8(x^y)
		}
		pow[y] = row//就像上面井字棋游戏一样，把内层的列当做一个整体，那就是一个元素，即每一行都是一个元素（以外层视角来看）
	}
	return pow
}

func main() {
	pic.Show(Pic)
}
```

解析：返回一个长度为dy的切片，说明切片有dy行；其中每个元素是一个长度为dx，说明每一行中有dx列，这里的每个元素是以二维数组的视角去看的，每个元素就是每一行的意思。所以要先动态分配dy长度给外层切片，然后对每一行做遍历，现在视角转到了内层切片即一维数组，那么动态分配dx长度(列数)给内层切片，赋值给内层切片后再赋值给外层切片那就用元素充满了外层切片的某一行了。最后返回外层切片即可。



### map映射

可以把映射与数组归在一起想，它是一种类型，而不是某个函数的意思。

`map` 映射将键映射到值。**大致是map [T]T，里面是键的type，外面是值的type。**

映射的零值为 `nil` 。`nil` 映射既没有键，也不能添加键。

`make` 函数会返回给定类型的映射，并将其初始化备用，通俗来说就是创建了一个新的map实例。

```go
type Vertex struct {
	Lat, Long float64
}

//var m map[string]Vertex,要这行的话下面main中的:就不需要了
//还可以这么初始化： 记得是KV形式
/*var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}此处的Vertex可以省略
*/

func main() {
	m := make(map[string]Vertex)//具体来说，m 是一个 map[string]Vertex，即将字符串（如地名）映射到 Vertex 结构体（地理坐标）
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
    }//向map中添加一个键值对，键是"Bell Labs"(当然这里面的内容可以替换),值是上面的数字。Vertex关键字不要省略
	fmt.Println(m["Bell Labs"])
    fmt.Println(m["Bell Labs"].Lat) //只打印Lat的信息
}
```

注意不能只对Lat或者Long做映射，要两个一起做映射；如果要只对一个做映射，只能分开写，如下方

```go
var m map[string]float64

func main() {
    // 初始化 map，仅存储纬度
    m = make(map[string]float64)
    // 给 "Bell" 做映射
    m["Bell"] = 40.68433
    // 打印 "Bell" 的纬度
    fmt.Println("Latitude of Bell:", m["Bell"])
}
```

想要实例化然后赋值，就`实例化出来的名字，如m[T's name] = value`,比如上面的`m["Bell"] = 40.68433`



在映射 `m` 中插入或修改元素，即修改就是重新插入：

```go
m[key] = elem//(如果elem是结构体，记得得 结构体名{val1 val2})
```

获取元素：

```
elem = m[key]
```

删除元素：

```
delete(m, key)
```

通过双赋值检测某个键是否存在：

```
elem, ok = m[key]
```

若 `key` 在 `m` 中，`ok` 为 `true` ；否则，`ok` 为 `false`。

若 `key` 不在映射中，则 `elem` 是该映射元素类型的零值。

**注**：若 `elem` 或 `ok` 还未声明，你可以使用短变量声明：

```go
elem, ok := m[key]
```



### 函数值

函数也是值。它们可以像其他值一样传递。

函数值可以用作函数的参数或返回值。如下示例，**如果函数要作为参数，那么在参数列表里面要加个fn，之后func(别的函数参数类型，...) 别的函数返回值类型**。return fn就是应用fn函数，例子中的3和4作为参数传到fn函数里面。

```go
func compute(fn func(float64, float64) float64) float64 {  第一个float64是 作为compute函数的参数 的函数 的参数，第二个同理，第三个是 的返回值，第四个是compute函数的返回值
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}//匿名函数
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))//传函数到compute函数里面，比如这个计算幂次方的函数传到compute，它计算的是3的4次方，上面计算的是三角形的斜边
}
```



Go 函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。 该函数可以访问并赋予其引用的变量值，换句话说，该函数被“绑定”到了这些变量。在 Go 语言中，闭包通过匿名函数来实现，匿名函数可以引用在其外部定义的变量。

```go
示例1：基本闭包
package main

import "fmt"

func main() {
    // 创建一个匿名函数，并将其赋值给变量 add
    add := func(a, b int) int {
        return a + b
    }

    // 调用闭包
    fmt.Println(add(3, 4)) // 输出：7
}
示例2：带状态的闭包
package main

import "fmt"

// adder 函数返回一个闭包(把func (int) int看作是一个整体就好)，该闭包保持了 sum 变量的状态
func adder() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}
/*sum 变量在 adder 函数中定义，但在闭包函数中使用。
每个调用 adder 返回的闭包函数 pos 和 neg 各自维护了独立的 sum 变量。*/
func main() {
    // 创建两个独立的闭包
    pos, neg := adder(), adder()
    for i := 0; i < 10; i++ {
        fmt.Println(
            pos(i),     // 每次调用时，pos 的 sum 变量累加 i
            neg(-2*i),  // 每次调用时，neg 的 sum 变量累加 -2*i
        )
    }
}
```

**大致形式如** 

```
func 函数名(参数) func(闭包参数的T) 闭包返回值的T{
	...   如func(x int) int
	return func(闭包参数) 闭包返回值的T{
		...
		return 闭包返回值}
}

记得要创建闭包，如pos, neg := adder(), adder()一样
```

实现一个 `fibonacci` 函数，它返回一个函数（闭包），该闭包返回一个[斐波纳契数列](https://zh.wikipedia.org/wiki/斐波那契数列) (0, 1, 1, 2, 3, 5, ...)。

```go
// fibonacci 是返回一个「返回一个 int 的函数」的函数
func fibonacci() func() int {
	m,n := 0,1
	return func() int{
		current := m
		m,n = n,m+n
		return current
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
/*m,n是会变化的，第一次循环开始的时候mn为01，第一次结束mn就是11了，以11进入到新的闭包，但可见current是全新的，每次循环都有一个新的current，但是与上面sum一样闭包函数执行完毕后都会影响到外面变量的值(如sum在每次循环结束后都会变化)，m/n/sum这些都拿来复用。*/
```





# 方法和接口

### 方法

**理解不了就转换成等价形式**

方法就是一类带特殊的 **接收者** 参数的函数。**是接收者而不是返回者！**

方法接收者在它自己的参数列表内，位于 `func` 关键字和方法名之间。

在此例中，`Abs` 方法拥有一个名字为 `v`，类型为 `Vertex` 的接收者，即Abs方法是属于Vertex类型的。看清哪个是方法名哪个是接收者哪个是返回值类型。

- `Abs` 方法接收者 `v` 在这里作为方法的一个参数，就像它在方法的参数列表内。

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {//v可以用来取值
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
/*本质上等价于func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}*/

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

**记住：方法只是个带接收者参数的函数。也可以为非结构体类型声明方法（如为type MyFloat float64这种类型声明方法，用的时候和这个一样声明一个接受者如：aa MyFloat），接收者的类型定义和方法声明必须在同一包内**

你可以为指针类型的接收者声明方法。

这意味着对于某类型 `T`，接收者的类型可以用 `*T` 的文法。 （此外，`T` 本身不能是指针，比如不能是 `*int`。）

例如，这里为 `*Vertex` 定义了 `Scale` 方法。

指针接收者的方法可以修改接收者指向的值（如这里的 `Scale` 所示）。 由于方法经常需要修改它的接收者，**指针接收者比值接收者更常用。**通常来说，所有给定类型的方法都应该有值或指针接收者，但并不应该二者混用，直接全用指针接收者就行了。

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs()) //50，若把v *Vertex中的*去掉，那么结果就是5，因为值接收者是对副本做出改变
}
```

接收者为指针的的方法被调用时，接收者既能是值又能是指针

```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}   ******既能是值
	v.Scale(2)
	ScaleFunc(&v, 10)

	p := &Vertex{4, 3}   *****也能是指针
	p.Scale(3)
	ScaleFunc(p, 8)

	fmt.Println(v, p)
}
```



### 接口

方法多和接口一起来使用。

1.接口类型是一组方法签名的集合

接口类型的定义包括一组方法签名，这些方法没有实现（即方法体为空）。例如：

```go
package main

import "fmt"

// 定义一个接口
type Speaker interface {
    Speak() string
}
```

在这个例子中，`Speaker` 接口定义了一个方法 `Speak()`，方法返回一个 `string` 类型。

2. 实现了这些方法的类型自动满足接口

任何定义了 `Speak()` 方法的类型都被认为实现了 `Speaker` 接口。例如：

```go
// 定义一个结构体
type Dog struct {
    Name string
}

// 为 Dog 定义 Speak 方法
func (d Dog) Speak() string {
    return "Woof!"
}

// 定义另一个结构体
type Cat struct {
    Name string
}

// 为 Cat 定义 Speak 方法
func (c Cat) Speak() string {
    return "Meow!"
}
```

在这个例子中，`Dog` 和 `Cat` 都实现了 `Speaker` 接口，因为它们都定义了 `Speak()` 方法。

3. 接口类型的变量可以持有任何实现了这些方法的值

接口类型的变量可以保存任何实现了该接口的类型的值。例如：

```go
func main() {
    var speaker Speaker //接口类型的变量

    dog := Dog{Name: "Buddy"}
    cat := Cat{Name: "Kitty"}

    speaker = dog  // 接口变量可以持有 Dog 类型的值
    fmt.Println(speaker.Speak()) // 输出: Woof!

    speaker = cat  // 接口变量可以持有 Cat 类型的值
    fmt.Println(speaker.Speak()) // 输出: Meow!
}
```

在这个例子中，接口变量 `speaker` 可以持有 `Dog` 类型和 `Cat` 类型的值，因为它们都实现了 `Speaker` 接口。

举个综合例子

下面是一个更加完整的例子：

```go
package main

import "fmt"

// 定义接口
type Speaker interface {
    Speak() string
}

// 实现接口的结构体之一
type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return "Woof!"
}

// 实现接口的结构体之二
type Cat struct {
    Name string
}

func (c Cat) Speak() string {
    return "Meow!"
}

// 使用接口类型的函数
func makeAnimalSpeak(speaker Speaker) {
    fmt.Println(speaker.Speak())
}

func main() {
    dog := Dog{Name: "Buddy"}
    cat := Cat{Name: "Kitty"}

    makeAnimalSpeak(dog) // 输出: Woof!
    makeAnimalSpeak(cat) // 输出: Meow!
}
```

在这个综合例子中，`makeAnimalSpeak` 函数接受一个 `Speaker` 接口类型的参数，因此可以接收任何实现了 `Speak()` 方法的类型。



接口的隐式实现

```go
type I interface {
	M()
}

type T struct {
	S string
}

// 此方法表示类型 T 实现了接口 I，不过我们并不需要显式声明这一点。
func (t T) M() {
	fmt.Println(t.S)
}
```



即便接口内的具体值为 nil，方法仍然会被 nil 接收者调用。

在一些语言中，这会触发一个空指针异常，但在 Go 中通常会写一些方法来优雅地处理它（如本例中的 `M` 方法）。

**注意:** 保存了 nil 具体值的接口其自身并不为 nil。

```go
type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

func main() {
	var i I

	var t *T
	i = t
	describe(i)
	i.M()

	i = &T{"hello"}
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

nil 接口值既不保存值也不保存具体类型。

为 nil 接口调用方法会产生运行时错误，因为接口的元组内并未包含能够指明该调用哪个 **具体** 方法的类型。如下是错误的

```go
type I interface {
	M()
}

func main() {
	var i I
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

即空接口值是错误的，但可以先为接口赋值如上个例子的i=t，那么就算t是空的也是允许。



指定了零个方法的接口值被称为 *空接口：*

```
interface{}
```

空接口可保存任何类型的值。（因为每个类型都至少实现了零个方法。）

空接口被用来处理未知类型的值。例如，`fmt.Print` 可接受类型为 `interface{}` 的任意数量的参数。

```go
func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}
```



### 类型

**类型断言** 提供了访问接口值底层具体值的方式。

```
t := i.(T)
```

该语句断言接口值 `i` 保存了具体类型 `T`，并将其底层类型为 `T` 的值赋予变量 `t`。

若 `i` 并未保存 `T` 类型的值，该语句就会触发一个 panic。

为了 **判断** 一个接口值是否保存了一个特定的类型，类型断言可返回两个值：其底层值以及一个报告断言是否成功的布尔值。

```
t, ok := i.(T)
```

若 `i` 保存了一个 `T`，那么 `t` 将会是其底层值，而 `ok` 为 `true`。

否则，`ok` 将为 `false` 而 `t` 将为 `T` 类型的零值，程序并不会产生 panic。**就是有无第二个变量所决定会不会panic**。有点类似于map映射，可以跳转观察一下。

```go
func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // panic
	fmt.Println(f)
}
```

**类型选择** 是一种按顺序从几个类型断言中选择分支的结构。

类型选择与一般的 switch 语句相似，不过类型选择中的 case 为类型（而非值）， 它们针对给定接口值所存储的值的类型进行比较。

```
switch v := i.(type) {
case T:
    // v 的类型为 T
case S:
    // v 的类型为 S
default:
    // 没有匹配，v 与 i 的类型相同
}
```

类型选择中的声明与类型断言 `i.(T)` 的语法相同，只是具体类型 `T` 被替换成了关键字 `type`。

此选择语句判断接口值 `i` 保存的值类型是 `T` 还是 `S`。在 `T` 或 `S` 的情况下，变量 `v` 会分别按 `T` 或 `S` 类型保存 `i` 拥有的值。在默认（即没有匹配）的情况下，变量 `v` 与 `i` 的接口类型和值相同。



[`fmt`](https://go-zh.org/pkg/fmt/) 包中定义的 [`Stringer`](https://go-zh.org/pkg/fmt/#Stringer) 是最普遍的接口之一。

```
type Stringer interface {
    String() string
}
```

`Stringer` 是一个可以用字符串描述自己的类型。`fmt` 包（还有很多包）都通过此接口来打印值。如

```go
type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
    fmt.Println(a, z)   //a和z会调用自己实现的String()方法
}
```

Go 程序使用 `error` 值来表示错误状态。

与 `fmt.Stringer` 类似，`error` 类型是一个内建接口：

```
type error interface {
    Error() string
}
```

（与 `fmt.Stringer` 类似，`fmt` 包也会根据对 `error` 的实现来打印值。）

通常函数会返回一个 `error` 值，调用它的代码应当判断这个错误是否等于 `nil` 来进行错误处理。

```
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)
```

`error` 为 nil 时表示成功；非 nil 的 `error` 表示失败。

```go
type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```

练习;`Sqrt` 接受到一个负数时，应当返回一个非 nil 的错误值。复数同样也不被支持。

创建一个新的类型

```
type ErrNegativeSqrt float64
```

并为其实现

```
func (e ErrNegativeSqrt) Error() string
```

方法使其拥有 `error` 值，通过 `ErrNegativeSqrt(-2).Error()` 调用该方法应返回 `"cannot Sqrt negative number: -2"`。

```go
// 定义新的错误类型（基于 float64）
type ErrNegativeSqrt float64

// 实现 error 接口的 Error 方法
func (e ErrNegativeSqrt) Error() string {
    // 返回格式化后的错误信息
    return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
}

// 实现 Sqrt 函数
func Sqrt(x float64) (float64, error) {
    if x < 0 {
        return 0, ErrNegativeSqrt(x) //想想上面那个函数的等价形式，就能理解为什么可以直接传个x了！
    }
    // 简单的牛顿迭代法求平方根
    z := x
    for i := 0; i < 10; i++ {
        z = z - (z*z - x) / (2*z)
    }
    return z, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
```



`io` 包指定了 `io.Reader` 接口，它表示数据流的读取端。

Go 标准库包含了该接口的[许多实现](https://cs.opensource.google/search?q=Read\(\w%2B\s\[\]byte\)&ss=go%2Fgo)，包括文件、网络连接、压缩和加密等等。

`io.Reader` 接口有一个 `Read` 方法：

```
func (T) Read(b []byte) (n int, err error)
```

`Read` 用数据填充给定的字节切片并返回填充的字节数和错误值。在遇到数据流的结尾时，它会返回一个 `io.EOF` 错误。

示例代码创建了一个 [`strings.Reader`](https://go-zh.org/pkg/strings/#Reader) 并以每次 8 字节的速度读取它的输出。

```go
/*strings.NewReader 函数
strings.NewReader 是 Go 的标准库 strings 包中提供的一个函数，用于创建一个新的 Reader，该 Reader 从给定的字符串中读取数据。

原型
func NewReader(s string) *strings.Reader
参数和返回值
参数：接受一个字符串 s。
返回值：返回一个指向 strings.Reader 的指针，*strings.Reader 实现了 io.Reader、io.ReaderAt、io.Seeker、io.WriterTo 等接口。*/

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}
/*n = 8 err = <nil> b = [72 101 108 108 111 44 32 82]
b[:n] = "Hello, R"
n = 5 err = <nil> b = [101 97 100 101 114 44 32 82] 读取到 5 个字节 ("eader")，因为字符串已经到末尾了，没有 8 个字节。
												缓冲区的前 5 个字节更新为 "eader"，后 3 个字节保持不变 [44 32 													82]。
b[:n] = "eader"
n = 0 err = EOF b = [101 97 100 101 114 44 32 82]
b[:n] = ""
```

实现一个 `Reader` 类型，它产生一个 ASCII 字符 `'A'` 的无限流。

```go
type MyReader struct{}

// TODO: 为 MyReader 添加一个 Read([]byte) (int, error) 方法。
func (mr MyReader) Read(b []byte) (int,error){
	for i:= range b {
		b[i] = 'A'
	}
	return len(b), nil
}

func main() {
	reader.Validate(MyReader{})
}
```

有种常见的模式是一个 io.Reader 包装另一个 io.Reader，然后通过某种方式修改其数据流。 例如，gzip.NewReader 函数接受一个 io.Reader（已压缩的数据流）并返回一个同样实现了 io.Reader 的 *gzip.Reader（解压后的数据流）。**理解下面的例子！**

1. **定义 `UppercaseReader` 结构**：

   ```go
   type UppercaseReader struct {
       reader io.Reader
   }
   ```

2. **实现 `Read` 方法**：

   ```go
   func (ur *UppercaseReader) Read(p []byte) (int, error) {
       n, err := ur.reader.Read(p)
       if err != nil {
           return n, err
       }
   
       // 将读取的字节转换为大写
       for i := 0; i < n; i++ {
           p[i] = byte(unicode.ToUpper(rune(p[i])))
       }
   
       return n, nil
   }
   ```

   - 该方法首先从原始 `reader` 中读取数据。
   - 然后将读取到的字节转换为大写。
   - 最后返回修改后的字节数以及可能的错误。

3. **在 `main` 函数中使用**：

   ```go
   func main() {
       input := "Hello, World!"
       reader := strings.NewReader(input)
   
       uppercaseReader := &UppercaseReader{reader: reader} //该结构体包含一个字段 reader，类型是 io.Reader 接口。
   
       if _, err := io.Copy(os.Stdout, uppercaseReader); err != nil {
           fmt.Fprintln(os.Stderr, "Error:", err)
           os.Exit(1)
       }
       fmt.Println()
   }
   ```

   - 先创建一个字符串读取器。
   - 再用 `UppercaseReader` 包装它，实现对数据流的转换。
     - **第一个 `reader`**：
       - **`UppercaseReader` 结构体中的字段名**。
       - 表示 `UppercaseReader` 结构体中的一个字段，该字段的名字叫做 `reader`，并且它的类型是 `io.Reader`。
     - **第二个 `reader`**：
       - **之前定义的变量名**。
       - 它是一个变量名，表示已经存在并且用来初始化结构体字段的具体 `io.Reader` 对象
   - 使用 `io.Copy` 将转换后的数据流写入标准输出。
   - 运行上述代码得到`HELLO WORLD`



[`image`](https://go-zh.org/pkg/image/#Image) 包定义了 `Image` 接口：

```
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
```

**注意:** `Bounds` 方法的返回值 `Rectangle` 实际上是一个 [`image.Rectangle`](https://go-zh.org/pkg/image/#Rectangle)，它在 `image` 包中声明。

（请参阅[文档](https://go-zh.org/pkg/image/#Image)了解全部信息。）

`color.Color` 和 `color.Model` 类型也是接口，但是通常因为直接使用预定义的实现 `image.RGBA` 和 `image.RGBAModel` 而被忽视了。这些接口和类型由 [`image/color`](https://go-zh.org/pkg/image/color/) 包定义。

```go
package main

import (
    "fmt"
    "image"
    "image/color"
)

func main() {
    // 创建一个 100x100 像素的图像，所有像素初始为透明的黑色 (0, 0, 0, 0)
    m := image.NewRGBA(image.Rect(0, 0, 100, 100))

    // 打印图像的边界矩形，应该输出 (0,0)-(100,100)
    fmt.Println(m.Bounds()) 
    
    // 获取并打印图像 (0, 0) 处像素的 RGBA 值，应该默认输出 0 0 0 0
    fmt.Println(m.At(0, 0).RGBA())

    // 对 (0, 0) 处像素进行操作，例如设置为半透明红色
    m.Set(0, 0, color.RGBA{255, 0, 0, 128})
    // 再次获取并打印图像 (0, 0) 处像素的 RGBA 值
    fmt.Println(m.At(0, 0).RGBA()) // 应该输出 65535 0 0 32768，注意值已经放大到 16 位。
}
```

**练习**：图片生成器将会返回一个 image.Image 的实现。

定义你自己的 Image 类型，实现必要的方法并调用 pic.ShowImage。

Bounds 应当返回一个 image.Rectangle ，例如 image.Rect(0, 0, w, h)。

ColorModel 应当返回 color.RGBAModel。

At 应当返回一个颜色。上一个图片生成器的值 v 对应于此次的 color.RGBA{v, v, 255, 255}。

```go
package main

import (
    "image"
    "image/color"
    "golang.org/x/tour/pic" // 假设 pic.ShowImage 在此包中定义
)

// 自定义的 Image 类型
type Image struct {
    width, height int
}

// Bounds 方法实现
func (img *Image) Bounds() image.Rectangle {
    return image.Rect(0, 0, img.width, img.height)
}

// ColorModel 方法实现
func (img *Image) ColorModel() color.Model {
    return color.RGBAModel
}

// At 方法实现
func (img *Image) At(x, y int) color.Color {
    // 生成颜色值，简单地使用 x 和 y 的某些操作
    v := uint8((x + y) % 256)
    return color.RGBA{v, v, 255, 255}
}

// 主函数
func main() {
    m := Image{width: 256, height: 256}
    pic.ShowImage(&m)
}
```

btw:

```go
m := Image{256, 256}
pic.ShowImage(&m)
```

1. 创建 `Image` 值

   ```
   m := Image{256, 256}
   ```

   - 这里我们创建了一个 `Image` 结构体的值 `m`，它在栈上分配内存。

```go
m := &Image{256, 256}
pic.ShowImage(m)
```

1. 创建 `Image` 指针

   ```
   m := &Image{256, 256}
   ```

   - 这里直接创建了一个指向 `Image` 结构体的指针 `m`，它在堆上分配内存（在栈上存储指针）。





# 泛型

让我们通过具体例子来展示有无类型参数的区别。

### 没有类型参数的情况

假设我们想要实现两个函数，分别用于查找整数切片和字符串切片中的元素。

#### 查找整数元素的函数

```go
func IndexInt(s []int, x int) int {
    for i, v := range s {
        if v == x {
            return i
        }
    }
    return -1
}
```

#### 查找字符串元素的函数

```go
func IndexString(s []string, x string) int {
    for i, v := range s {
        if v == x {
            return i
        }
    }
    return -1
}
```

#### 主函数调用

```go
func main() {
    // 使用 IndexInt 查找整数切片中的元素
    si := []int{10, 20, 15, -10}
    fmt.Println(IndexInt(si, 15))

    // 使用 IndexString 查找字符串切片中的元素
    ss := []string{"foo", "bar", "baz"}
    fmt.Println(IndexString(ss, "hello"))
}
```

### 使用类型参数的情况

通过使用类型参数，我们可以将上述两个函数合并为一个通用的函数。

#### 定义通用的 Index 函数

```go
func Index[T comparable](s []T, x T) int {
    for i, v := range s {
        if v == x {
            return i
        }
    }
    return -1
}
```

- `T`: 这是一个类型参数，占位符，表示可以是任何类型。
- `comparable`: 这是一个约束，表示类型 `T` 必须是可比较的（可以使用 `==` 和 `!=` 操作符）。

#### 主函数调用

```go
func main() {
    // 使用通用的 Index 函数查找整数切片中的元素
    si := []int{10, 20, 15, -10}
    fmt.Println(Index(si, 15))

    // 使用通用的 Index 函数查找字符串切片中的元素
    ss := []string{"foo", "bar", "baz"}
    fmt.Println(Index(ss, "hello"))
}
```

**由此看来，可以省去很多重复的代码，关键就是泛型编程，把真正的数据类型换成T，有点像java的List<T>等等**

泛型单链表 `List[T]`，并为其添加一些基本操作。

```go
package main

import (
    "fmt"
)

// List 表示一个可以保存任何类型的值的单链表。any 是 Go 中的新类型别名，实际上是 interface{} 的别名。使用 any 可以让代码更具可读性。
//interface{}（以及因此 any）表示任何类型，这是因为所有类型都实现了空接口。T any 意味着 T 可以是任何类型。这种声明允许类型 T 在调用泛型函数或创建泛型类型实例时被具体化为任何有效的 Go 类型。
type List[T any] struct {
    next *List[T]
    val  T
}

// 添加元素到链表的尾部
func (list *List[T]) Append(value T) {
    current := list
    for current.next != nil {
        current = current.next
    }
    current.next = &List[T]{val: value}
}

// 查找元素在链表中的位置，返回下标，如果未找到返回 -1
func (list *List[T]) Index(value T) int {
    current := list
    index := 0
    for current != nil {
        if current.val == value {
            return index
        }
        current = current.next
        index++
    }
    return -1
}

// 删除链表中第一个找到的指定值
func (list *List[T]) Remove(value T) {
    current := list
    var previous *List[T]
    for current != nil {
        if current.val == value {
            if previous != nil {
                previous.next = current.next
            }
            return
        }
        previous = current
        current = current.next
    }
}

// 打印链表中的所有元素
func (list *List[T]) Print() {
    current := list
    for current != nil {
        fmt.Printf("%v -> ", current.val)
        current = current.next
    }
    fmt.Println("nil")
}

func main() {
    // 创建一个存储整数的链表
    intList := &List[int]{val: 1}
    intList.Append(2)
    intList.Append(3)
    intList.Append(4)
    intList.Print() // 输出: 1 -> 2 -> 3 -> 4 -> nil

    // 查找元素
    fmt.Println(intList.Index(3))  // 输出: 2
    fmt.Println(intList.Index(10)) // 输出: -1

    // 删除元素
    intList.Remove(3)
    intList.Print() // 输出: 1 -> 2 -> 4 -> nil

    // 创建一个存储字符串的链表
    strList := &List[string]{val: "foo"}
    strList.Append("bar")
    strList.Append("baz")
    strList.Print() // 输出: foo -> bar -> baz -> nil
}
```





# 并发

Go 程（goroutine）是由 Go 运行时管理的轻量级线程。

```
go f(x, y, z)
```

会启动一个新的 Go 协程并执行

```
f(x, y, z)
```

`f`, `x`, `y` 和 `z` 的求值发生在当前的 Go 协程中，而 `f` 的执行发生在新的 Go 协程中。



信道是带有类型的管道，你可以通过它用**信道操作符 `<-` 来发送或者接收值**，而不是ch。

```
ch <- v    // 将 v 发送至信道 ch。
v := <-ch  // 从 ch 接收值并赋予 v。
```

（“箭头”就是数据流的方向。）

和映射与切片一样，信道在使用前必须创建：

```
ch := make(chan int)
```

默认情况下，发送和接收操作在另一端准备好之前都会阻塞。这使得 Go 程可以在没有显式的锁或竞态变量的情况下进行同步。

以下示例对切片中的数进行求和，将任务分配给两个 Go 程。一旦两个 Go 程完成了它们的计算，它就能算出最终的结果。

```go
func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // 发送 sum 到通道 c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int)
    go sum(s[:len(s)/2], c)  // 传递切片的前半部分到 goroutine
    go sum(s[len(s)/2:], c)  // 传递切片的后半部分到 goroutine
    x, y := <-c, <-c         // 从通道 c 接收两个结果

    fmt.Println(x, y, x+y)   // 打印两个结果以及它们的和   需要注意的是，**goroutine 的运行顺序是不确定的**，两个 goroutine 的结果可能以任何顺序发送到通道 c。这意味着 x, y := <-c, <-c 可能先接收到 17，也可能先接收到 -5。
}
```

信道可以是 **带缓冲的**。将缓冲长度作为第二个参数提供给 `make` 来初始化一个带缓冲的信道：

```
ch := make(chan int, 100)
```

仅当信道的缓冲区填满后，向其发送数据时才会阻塞。当缓冲区为空时，接受方会阻塞。填满缓冲区后再尝试去填缓冲区会导致死锁。

1. **发送操作**：使用 `chan <-` 语法，将数据发送到通道。左边是通道，右边是要发送的值。**发是指向通道发**

   ```go
   c <- value
   ```

   解释：将 `value` 发送到通道 `c`。

2. **接收操作**：使用 `<- chan` 语法，从通道接收数据。可能会在一个变量或直接在操作中使用。**收是指从通道收**

   ```go
   value := <- c
   ```

   解释：从通道 `c` 接收一个值，并将该值赋给 `value`。

3. **关键就是看通道变量在哪一边，如果在左边，那就是发送操作。**



发送者可通过 `close` 关闭一个信道来表示没有需要发送的值了。

```go
func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i := range c {  //记住这种遍历方式！！！这才是打印通道内所有值的方法！！
		fmt.Println(i)
	}
}
```



接收者可以通过为接收表达式分配第二个参数来测试信道是否被关闭：若没有值可以接收且信道已被关闭，那么在执行完

```
v, ok := <-ch
```

此时 `ok` 会被设置为 `false`。

循环 `for i := range c` 会不断从信道接收值，直到它被关闭。

**注意**： 只应由发送者关闭信道，而不应油接收者关闭。向一个已经关闭的信道发送数据会引发程序 panic。

**还要注意**： 信道与文件不同，通常情况下无需关闭它们。只有在必须告诉接收者不再有需要发送的值时才有必要关闭，例如终止一个 `range` 循环。



`go func() { ... }()`

`go func() { ... }()` 是一个匿名函数，定义后立即被调用并作为一个新的 goroutine 执行。这个模式在 Go 语言中很常见，用于并发启动某段代码执行。

这里的 `go func() { ... }()` 具体做了以下事情：

1. `go func() { ... }` 定义一个匿名函数，并指示要以新的 goroutine 方式并发执行这个函数。
2. 后面的 `()` 表示立即调用这个匿名函数。



`select` 语句使一个 Go 程可以等待多个通信操作。

`select` 会阻塞到某个分支可以继续执行为止，这时就会执行该分支。当多个分支都准备好时会随机选择一个执行。

```go
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)   //看清楚这是接收操作！
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

1. `main` 函数开始执行。
2. 创建通道 `c` 和 `quit`。
3. 启动新的 goroutine 执行匿名函数。
   - 匿名函数启动后，会立即阻塞在 `<-c` 上，等待从 `c` 通道接收值。
4. `main` 函数继续执行并调用 `fibonacci(c, quit)`。
5. `fibonacci` 函数开始生成 Fibonacci 数列，并通过 `c` 通道发送值。
6. 匿名函数在 `c` 通道中收到值时打印出来，并继续循环接收后续值。
7. 当接收到 10 个值后，匿名函数向 `quit` 通道发送 `0`。
8. `fibonacci` 函数在 `quit` 通道接收到信号后，打印 `quit` 并返回，结束执行。



`select` 语句的特点和用法

1. **选择一个可以立即执行的通道操作**：
   - 如果有多个通道操作都可以执行，Go 运行时会随机选择一个。
   - 如果没有任何通道操作可以执行，`select` 会阻塞，直到某个操作可以执行为止。
2. **每个分支中必须包含一个通道操作**。
3. **可以有一个 `default` 分支**：
   - 如果提供了 `default` 分支，并且没有其他通道操作可以执行，`default` 分支会立即执行。

基本语法

```go
select {
case <-chan1:
    // chan1 有数据可读
case chan2 <- value:
    // 可以向 chan2 发送数据
default:
    // 如果没有任何通道操作可以进行，执行这里的代码
}假设没有default，那么case1和case2都是可以执行，Go运行时会随机选择一个
```

示例代码

下面是一个使用 `select` 语句的示例，它展示了如何在多个通道操作之间进行选择：

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        c1 <- "hello from c1"
    }()

    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "hello from c2"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("Received", msg1)
        case msg2 := <-c2:
            fmt.Println("Received", msg2)
        }
    }
}
```

解释

在这个示例中，有两个 goroutine，它们会分别在不同的时间间隔后向不同的通道发送消息。`main` 函数中的 `select` 语句会监听这两个通道，在其中之一有数据可读时进行操作。

1. 两个 goroutines 会在不同的时间后向 `c1` 和 `c2` 通道发送字符串。
2. `select` 语句会阻塞，直到 `c1` 或 `c2` 有数据发送过来。
3. select 语句的每个case分支检查各自的通道操作：
   - 如果通过 `<-c1` 接收到数据，就执行第一个 `case`。
   - 如果通过 `<-c2` 接收到数据，就执行第二个 `case`。
4. 当所有通道操作都无法进行时，如果有 `default` 分支，则它会被执行。

使用 `select` 解决实际问题

`select` 语句非常适用于处理多个通道的并发通信问题，可以用来实现超时控制、多路复用通道、响应系统信号等。

实现超时控制

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c := make(chan string)

    go func() {
        time.Sleep(2 * time.Second)
        c <- "result"
    }()

    select {
    case res := <-c:
        fmt.Println(res)
    case <-time.After(1 * time.Second):
        fmt.Println("timeout")
    }
}
```

在这个示例中，`time.After(1 * time.Second)` 创建一个定时器通道，在 1 秒后发送数据。如果主程序超过 1 秒仍没有收到 `c` 通道的消息，将会进入 `case <-time.After(1 * time.Second)` 分支，打印 "timeout"。



 `tree` 包，它定义了类型：`Tree` 的文档可在[这里](https://godoc.org/golang.org/x/tour/tree#Tree)找到。

```go
type Tree struct {
    Left  *Tree
    Value int //大写不是小写
    Right *Tree
}
```

**练习：等价二叉查找树**

**1.** 实现 `Walk` 函数。

**2.** 测试 `Walk` 函数。

函数 `tree.New(k)` 用于构造一个随机结构的已排序二叉查找树，它保存了值 `k`, `2k`, `3k`, ..., `10k`。

创建一个新的信道 `ch` 并且对其进行步进：

```
go Walk(tree.New(1), ch)
```

然后从信道中读取并打印 10 个值。应当是数字 1, 2, 3, ..., 10.

**3.** 用 `Walk` 实现 `Same` 函数来检测 `t1` 和 `t2` 是否存储了相同的值。

**4.** 测试 `Same` 函数。

`Same(tree.New(1), tree.New(1))` 应当返回 `true`，而 `Same(tree.New(1), tree.New(2))` 应当返回 `false`。

```go
package main

import "golang.org/x/tour/tree"
import "fmt"

// Walk 遍历树 t，并树中所有的值发送到信道 ch。
func Walk(t *tree.Tree, ch chan int){
	if t == nil {
		return
	}
	Walk(t.Left,ch)
	ch <- t.Value
	Walk(t.Right,ch)
}

// Same 判断 t1 和 t2 是否包含相同的值。
func Same(t1, t2 *tree.Tree) bool {
    ch1 := make(chan int)
    ch2 := make(chan int)

    // 启动两个 goroutine 分别遍历两棵树
    go func() {
        Walk(t1, ch1)
        close(ch1)
    }()
    go func() {
        Walk(t2, ch2)
        close(ch2)
    }()

    // 比较两个通道中的值
    for {
        v1, ok1 := <-ch1
        v2, ok2 := <-ch2

        if ok1 != ok2 || v1 != v2 {
            return false
        }
        if !ok1 {
            return true
        }
    }
}

func main() {
    // 测试 Same
    t1 := tree.New(1)
    t2 := tree.New(1)
    t3 := tree.New(2)

    fmt.Println(Same(t1, t2)) // 应该返回 true
    fmt.Println(Same(t1, t3)) // 应该返回 false
}
```



信道非常适合在各个 Go 程间进行通信。

但是如果我们并不需要通信呢？比如说，若我们只是想保证每次只有一个 Go 程能够访问一个共享的变量，从而避免冲突？

这里涉及的概念叫做 *互斥（mutual*exclusion）* ，我们通常使用 *互斥锁（Mutex）* 这一数据结构来提供这种机制。

Go 标准库中提供了 [`sync.Mutex`](https://go-zh.org/pkg/sync/#Mutex) 互斥锁类型及其两个方法：

- `Lock`
- `Unlock`

我们可以通过在代码前调用 `Lock` 方法，在代码后调用 `Unlock` 方法来保证一段代码的互斥执行。参见 `Inc` 方法。

我们也可以用 `defer` 语句来保证互斥锁一定会被解锁。参见 `Value` 方法。

```go
import (
	"fmt"
	"sync"
	"time"
)

// SafeCounter 是并发安全的
type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

// Inc 对给定键的计数加一
func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	// 锁定使得一次只有一个 Go 协程可以访问映射 c.v。
	c.v[key]++
	c.mu.Unlock()
}

// Value 返回给定键的计数的当前值。
func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	// 锁定使得一次只有一个 Go 协程可以访问映射 c.v。
	defer c.mu.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}
```



在这个练习中，我们将会使用 Go 的并发特性来并行化一个 Web 爬虫。

修改 `Crawl` 函数来并行地抓取 URL，并且保证不重复。

*提示：* 你可以用一个 map 来缓存已经获取的 URL，但是要注意 map 本身并不是并发安全的！

```go
package main

import (
	"fmt"
	"sync"
)

//
// Several solutions to the crawler exercise from the Go tutorial
// https://tour.golang.org/concurrency/10
//

//
// Serial crawler
//

func Serial(url string, fetcher Fetcher, fetched map[string]bool) {
	if fetched[url] {
		return
	}
	fetched[url] = true
	urls, err := fetcher.Fetch(url)
	if err != nil {
		return
	}
	for _, u := range urls {
		Serial(u, fetcher, fetched)
	}
	return
}

//
// Concurrent crawler with shared state and Mutex
//

type fetchState struct {
	mu      sync.Mutex
	fetched map[string]bool
}

func ConcurrentMutex(url string, fetcher Fetcher, f *fetchState) {
	f.mu.Lock()
	already := f.fetched[url]
	f.fetched[url] = true
	f.mu.Unlock()

	if already {
		return
	}

	urls, err := fetcher.Fetch(url)
	if err != nil {
		return
	}
	var done sync.WaitGroup
	for _, u := range urls {
		done.Add(1)
        u2 := u
		go func() {
			defer done.Done()
			ConcurrentMutex(u2, fetcher, f)
		}()
		//go func(u string) {
		//	defer done.Done()
		//	ConcurrentMutex(u, fetcher, f)
		//}(u)
	}
	done.Wait()
	return
}

func makeState() *fetchState {
	f := &fetchState{}
	f.fetched = make(map[string]bool)
	return f
}

//
// Concurrent crawler with channels
//

func worker(url string, ch chan []string, fetcher Fetcher) {
	urls, err := fetcher.Fetch(url)
	if err != nil {
		ch <- []string{}
	} else {
		ch <- urls
	}
}

func master(ch chan []string, fetcher Fetcher) {
	n := 1
	fetched := make(map[string]bool)
	for urls := range ch {
		for _, u := range urls {
			if fetched[u] == false {
				fetched[u] = true
				n += 1
				go worker(u, ch, fetcher)
			}
		}
		n -= 1
		if n == 0 {
			break
		}
	}
}

func ConcurrentChannel(url string, fetcher Fetcher) {
	ch := make(chan []string)
	go func() {
		ch <- []string{url}
	}()
	master(ch, fetcher)
}

//
// main
//

func main() {
	fmt.Printf("=== Serial===\n")
	Serial("http://golang.org/", fetcher, make(map[string]bool))

	fmt.Printf("=== ConcurrentMutex ===\n")
	ConcurrentMutex("http://golang.org/", fetcher, makeState())

	fmt.Printf("=== ConcurrentChannel ===\n")
	ConcurrentChannel("http://golang.org/", fetcher)
}

//
// Fetcher
//

type Fetcher interface {
	// Fetch returns a slice of URLs found on the page.
	Fetch(url string) (urls []string, err error)
}

// fakeFetcher is Fetcher that returns canned results.
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) ([]string, error) {
	if res, ok := f[url]; ok {
		fmt.Printf("found:   %s\n", url)
		return res.urls, nil
	}
	fmt.Printf("missing: %s\n", url)
	return nil, fmt.Errorf("not found: %s", url)
}

// fetcher is a populated fakeFetcher.
var fetcher = fakeFetcher{
	"http://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"http://golang.org/pkg/",
			"http://golang.org/cmd/",
		},
	},
	"http://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"http://golang.org/",
			"http://golang.org/cmd/",
			"http://golang.org/pkg/fmt/",
			"http://golang.org/pkg/os/",
		},
	},
	"http://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"http://golang.org/",
			"http://golang.org/pkg/",
		},
	},
	"http://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"http://golang.org/",
			"http://golang.org/pkg/",
		},
	},
}
```





`sync.WaitGroup` 是 Go 语言标准库中的一个用于并发编程的工具。它主要用于在主程序或其他 Goroutine 中等待一组 Goroutine 完成执行。通过 `WaitGroup`，你可以确保主程序在所有相关 Goroutine 完成之后再退出，避免因主程序提前退出导致 Goroutine 没有完成任务。

WaitGroup 的基本用法

属性和方法

`sync.WaitGroup` 本身没有公开的属性，但它有以下几个重要的方法：

1. **Add(delta int)**：
   - 增加或减少 `WaitGroup` 的计数器。`delta` 应该是一个整数。增加的 Goroutine 数量应该匹配实际启动的 Goroutine 数量。如果 `delta` 为负值，则减少计数器。
   - 例如，`wg.Add(1)` 增加计数器1，`wg.Add(-1)` 减少计数器1。
2. **Done()**：
   - 减少 `WaitGroup` 的计数器，和 `Add(-1)` 功能相同。通常在 Goroutine 完成其工作时调用。
   - 例如，在一个 Goroutine 的结束部分调用 `wg.Done()`。
3. **Wait()**：
   - 阻塞并等待 `WaitGroup` 的计数器归零。当计数器为零时，`Wait` 解除阻塞。通常由调用 `wg.Wait()` 的 Goroutine 执行。

使用示例

下面是一个简单的示例代码，展示了如何使用 `sync.WaitGroup` 等待一组 Goroutine 完成：

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done() // 在函数结束时调用 Done，确保无论函数如何结束都会减少 WaitGroup 的计数器
    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second) // 模拟工作
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1) // 每启动一个 Goroutine，就增加 WaitGroup 的计数
        go worker(i, &wg)
    }

    wg.Wait() // 等待所有 Goroutine 完成
    fmt.Println("All workers done")
}
```

解释

1. **例子中的 Goroutine**：
   - `worker` 函数是一个简单的工作函数，它模拟了一些工作。`defer wg.Done()` 确保在函数返回时，无论是正常返回还是异常返回，`Done` 方法都会被调用，减少 `WaitGroup` 的计数器。
2. **启动 Goroutine**：
   - 在主函数中，for 循环启动了 5 个 Goroutine，每次启动之前调用 `wg.Add(1)` 增加计数器。
3. **等待所有 Goroutine 完成**：
   - `wg.Wait()` 会阻塞当前 Goroutine（在这个例子中是 `main` 函数的 Goroutine），直到 `WaitGroup` 的计数器为零。
   - 不能知道当前计数器值为多少，但可以用Wait方法去等待计数器归零再进行下一步操作。

使用建议

- **配对调用**：确保每次调用 `Add` 增加计数器时，都有一个对应的 `Done` 调用减少计数器。
- **避免减少计数器到负值**：不正确的 `Add` 或 `Done` 调用可能导致计数器减少到负值，`Wait` 方法在这种情况下会立即返回并且进一步行为变得未定义。
- **线程安全**：`WaitGroup` 是线程安全的，可以在多个 Goroutine 之间安全地共享使用。

# 接下来去哪？

你可以从[安装 Go](https://go-zh.org/doc/install/) 开始。

一旦安装了 Go，Go [文档](https://go-zh.org/doc/)是一个极好的 应当继续阅读的内容。 它包含了参考、指南、视频等等更多资料。

了解如何组织 Go 代码并在其上工作，参阅[此视频](https://www.youtube.com/watch?v=XCsL89YtqCs)，或者阅读[如何编写 Go 代码](https://tour.go-zh.org/doc/code.html)。

如果你需要标准库方面的帮助，请参考[包手册](https://go-zh.org/pkg/)。如果是语言本身的帮助，阅读[语言规范](https://tour.go-zh.org/ref/spec)是件令人愉快的事情。

进一步探索 Go 的并发模型，参阅 [Go 并发模型](https://www.youtube.com/watch?v=f6kdp27TYZs)([幻灯片](https://talks.go-zh.org/2012/concurrency.slide))以及[深入 Go 并发模型](https://www.youtube.com/watch?v=QDDwwePbDtw)([幻灯片](https://talks.go-zh.org/2013/advconc.slide))并阅读[通过通信共享内存](https://tour.go-zh.org/doc/codewalk/sharemem/)的代码之旅。

[Go 博客](https://blog.go-zh.org/)有着众多关于 Go 的文章和信息。

[Go 技术论坛](https://learnku.com/go)有大量关于 Go 的中文文档和 Go 官方博客的翻译。

开源电子书 [Go Web 编程](https://github.com/astaxie/build-web-application-with-golang)和 [Go 入门指南](https://github.com/Unknwon/the-way-to-go_ZH_CN)能够帮助你更加深入的了解和学习 Go 语言。

访问 [go-zh.org](https://go-zh.org/) 了解更多内容。



# 一些知识补充



**使用 GOPATH 进行项目管理**

如果你决定继续使用 GOPATH，你可以按照以下步骤进行项目管理：

目录结构

```plaintext
D:\GoWorkspace\
├── bin
├── pkg
└── src
    ├── projectA
    │   ├── main.go
    │   ├── otherfile.go
    │   └── ...
    └── projectB
        ├── main.go
        ├── otherfile.go
        └── ...
```





**使用GOMODULE进行项目管理**

假设你的 GitHub 用户名为 `yourusername`，那么目录结构可能如下：

```plaintext
D:\
├── github.com
    └── yourusername
        ├── projectA
        │   ├── moduleA1
        │   │   ├── go.mod
        │   │   ├── hh.go
        │   │   └── ...
        │   └── moduleA2
        │       ├── go.mod
        │       ├── main.go
        │       └── ...
        └── projectB
            ├── moduleB1
            │   ├── go.mod
            │   ├── main.go
            │   └── ...
            └── moduleB2
                ├── go.mod
                ├── aa.go
                └── ...
```

在这样的结构中，每个子模块 (`moduleA1`, `moduleA2`, `moduleB1`, `moduleB2`) 都会有自己的 `go.mod` 文件，可以独立的管理依赖和版本。

初始化 Go Modules

1. **进入每个模块目录并初始化 Go Modules**

例如，对于 `moduleA1` 和 `moduleA2`：

```shell
cd D:/github.com/yourusername/projectA/moduleA1
go mod init github.com/yourusername/projectA/moduleA1

cd D:/github.com/yourusername/projectA/moduleA2
go mod init github.com/yourusername/projectA/moduleA2
```

对于 `moduleB1` 和 `moduleB2`：

```shell
cd D:/github.com/yourusername/projectB/moduleB1
go mod init github.com/yourusername/projectB/moduleB1

cd D:/github.com/yourusername/projectB/moduleB2
go mod init github.com/yourusername/projectB/moduleB2
```

初始化 Git 仓库

1. **在顶层项目目录下初始化 Git 仓库**

例如，对于 `projectA` 和 `projectB`：

```shell
cd D:/github.com/yourusername/projectA
git init
git remote add origin https://github.com/yourusername/projectA.git
git add .
git commit -m "Initial commit"
git push -u origin master

cd D:/github.com/yourusername/projectB
git init
git remote add origin https://github.com/yourusername/projectB.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

Go Modules 的开发和构建

1. **在各自模块目录下进行构建和开发**

例如，对于 `moduleA1` 和 `moduleA2`：

```shell
cd D:/github.com/yourusername/projectA/moduleA1
go build
./moduleA1

cd D:/github.com/yourusername/projectA/moduleA2
go build
./moduleA2
```

对于 `moduleB1` 和 `moduleB2`：

```shell
cd D:/github.com/yourusername/projectB/moduleB1
go build
./moduleB1

cd D:/github.com/yourusername/projectB/moduleB2
go build
./moduleB2
```





在 Go 中，如果你的项目具有多个模块，并且这些模块之间有相互依赖关系（例如，某些模块是其他模块的依赖），你可以选择一个模块作为应用的主模块，该模块包含启动代码（即 `main` 函数）。其他模块可以独立存在，并作为库模块被主模块引用。

以下是一个示例项目结构，展示了如何组织管理系统的多模块设计，并在 Go 中实现一个启动类（`main` 函数）。

### 示例项目结构

假设你的 GitHub 用户名是 `yourusername`，项目名是 `management-system`，模块包括 `management`, `common` 和 `frontend`。

目录结构如下：

```plaintext
D:/github.com/yourusername/management-system/
├── common
│   ├── go.mod
│   ├── common.go
│   └── ...
├── management
│   ├── go.mod
│   ├── management.go
│   └── ...
└── frontend
    ├── go.mod
    ├── frontend.go
    ├── main.go  # 启动类
    └── ...
```

项目初始化

1. **初始化各个模块**

```shell
cd D:/github.com/yourusername/management-system/common
go mod init github.com/yourusername/management-system/common

cd D:/github.com/yourusername/management-system/management
go mod init github.com/yourusername/management-system/management

cd D:/github.com/yourusername/management-system/frontend
go mod init github.com/yourusername/management-system/frontend
```

1. **在 `frontend` 模块中添加对 `common` 和 `management` 模块的依赖**

编辑 `frontend/go.mod` 文件，添加对其他两个模块的依赖：

```go
module github.com/yourusername/management-system/frontend

go 1.17

require (
    github.com/yourusername/management-system/common v0.0.0
    github.com/yourusername/management-system/management v0.0.0
)
```

运行以下命令使依赖生效：

```shell
cd D:/github.com/yourusername/management-system/frontend
go get github.com/yourusername/management-system/common
go get github.com/yourusername/management-system/management
```

代码示例

`common/common.go`

```go
package common

import "fmt"

func CommonFunction() {
    fmt.Println("This is a common function")
}
```

`management/management.go`

```go
package management

import (
    "fmt"
    "github.com/yourusername/management-system/common"
)

func ManagementFunction() {
    fmt.Println("This is a management function")
    common.CommonFunction()
}
```

`frontend/frontend.go`

```go
package frontend

import (
    "fmt"
    "github.com/yourusername/management-system/management"
)

func FrontendFunction() {
    fmt.Println("This is a frontend function")
    management.ManagementFunction()
}
```

`frontend/main.go`

```go
package main

import "github.com/yourusername/management-system/frontend"

func main() {
    frontend.FrontendFunction()
}
```

构建和运行

1. 构建和运行项目：

```shell
cd D:/github.com/yourusername/management-system/frontend
go build
./frontend
```

这个过程将编译 `frontend` 模块，并执行 `main` 函数，从而启动整个应用。





使用 `go get` 命令来拉取别人的项目

假设 `yourproject/moduleA` 需要引用 `someproject` 中的某些代码，编辑 `yourproject/moduleA/go.mod` 文件：**是.mod文件！！！**

```go
module github.com/yourusername/yourproject/moduleA

go 1.17

require (
    github.com/usera/someproject v0.0.0-00010101000000-000000000000
)
```

*备注: `v0.0.0-00010101000000-000000000000` 为默认占位符版本号，当实际拉取后这里会更新为实际的版本号或最新提交的哈希。*

`yourproject/moduleA`中编辑 `moduleA.go` 文件来引用 `someproject`：

```go
package moduleA

import (
    "fmt"
    "github.com/usera/someproject"  ！！！！！！！！！
)

func UseSomeProject() {
    fmt.Println("Using some project functionality")
    someproject.SomeFunction()   ！！！！！！！
}
```



[1 Go语言开发环境搭建详细教程+go常见bug合集【Go语言教程】_go开发环境搭建-CSDN博客](https://blog.csdn.net/weixin_45565886/article/details/130175889?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-130175889-blog-127686657.235^v38^pc_relevant_sort_base2&spm=1001.2101.3001.4242.1&utm_relevant_index=3)



可以利用gofmt来格式化代码的缩进，如在命令行输入`gofmt -w main.go`即可，-w是必须要加的参数，还有-l，-s等等，[Go代码格式化——gofmt的使用-CSDN博客](https://blog.csdn.net/weixin_52000204/article/details/126638903#:~:text=Golang的开发团)
