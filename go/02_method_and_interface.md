# method and interface

- [method](#method)
  - [Pointer receivers](#pointer-receivers)
- [interface](#interface)
  - [interface values](#interface-values)
  - [empty interface](#empty-interface)
  - [Type assertion](#type-assertion)
  - [type switches](#type-switches)
  - [Case study: Stringers interface](#case-study-stringers-interface)
- [Error](#error)
  - [Define own Error](#define-own-error)  

## method

Go没有class，但是可以在类型上定义方法；比如可以在`struct`上定义一些方法，其实也就是实现类的效果了。

**A method is a function with a special receiver argument.** 

如下面所示，我们在Vertex上定义了Abs方法，在func和Abs之间的东西就是我们的接收器，表示这个函数所作用的类型，这样以来，一个函数就变成了一个方法。


```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

不过函数与方法没有本质的区别，方法不过是带有接收器的函数。所以我们完全可以不这样定义，而是直接把类型作为函数的参数传递。

```go
type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

比如这里实现了和上面例子中几乎一样的函数，只不过这里是把Vertex作为函数参数，而上面的例子是把它作为接收器receiver，所以它就变成了方法。

### Pointer receivers

接收器可以使用指针类型，比如：
```go
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

这样主要有两个好处：
- 不需要复制参数，而只是传递一个地址
- 可以在方法中修改原始的值，这通常是需要的



同理，函数的参数也可以是指针，比如
```go
func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Scale(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

但是这两种情况还是有一些区别的；
- 如果函数的参数声明为指针类型，那么它只能接受指针类型的数组，比如调用时必须要用`&x`的方式传入指针；
- 但是对于指针类型的接收器，既可以传值，也可以传指针；在传值的情况下，go会自动进行翻译: `(&x).Abs()`

反过来也是成立的：
- 如果函数的参数声明为接收值，那么就只能传递值，而不能传递引用；
- 但是对于接收值的receiver，既可以传值，也可以传引用；在传引用时，go也会自动解引用: `(*p).Abs()`



## interface

An interface type is defined as a set of method signatures. A value of interface type can hold any value that implements those methods.

**Interfaces are implemented implicitly** A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.

Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.


比如我们定义图片接口为：

```go
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
```

那么任何实现了接口中这几个方法的，就是实现了这个接口，而不需要额外使用implement之类的关键字说明，比如我们自定义一个图片类：

```go
package main

import (
	"image"
	"image/color"

	"golang.org/x/tour/pic"
)

type Image struct {
	W, H int
}

func (this Image) Bounds() image.Rectangle {
	return image.Rect(0, 0, this.W, this.H)
}

func (this Image) ColorModel() color.Model {
	return color.RGBAModel
}

func (this Image) At(x, y int) color.Color {
	return color.RGBA{uint8(x) / 2, uint8(y) / 2, 50, 200}
}

func main() {
	m := Image{256, 256}
	pic.ShowImage(m)
}
```

正如在main中所看到的一样，我们自己定义的Image类符合了`image`的标准，所以适用于`image`的`ShowImage`方法同样也适用于我们自定义的`Image`。

### interface values

interface是一种type，既然是一个类型，它就有值，interface的值可以想象为一个元组：`(value, concrete_type)`，也就是说interface类型的一个value，关联到一个具体的类型，比如说：

```go
package main

import "fmt"

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

output为：

```
(<nil>, *main.T)
<nil>
(&{hello}, *main.T)
hello
```



`i`是一个接口类型的变量，它可以持有任何实现该接口的具体类型；比如在一开始，它持有一个空的`*T`，所以输出的时候，它持有的值为nil但是它自己本身不为空；

这里怎么理解呢？我们上面提到了interface的value实际上可以理解为一个tuple，那么在这里`(value, concrete_type)`里面的具体类型是一个空值；

然后它持有一个非空的`&T{"hello"}`，也就是一个T类型的引用。Note that an interface value that holds a nil concrete value is itself non-nil.



但是对于`(value, concret_type)`，里面的value不可以是空值，比如下面的例子会报错：

```go
package main

import "fmt"

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

A nil interface value holds neither value nor concrete type.

**Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which *concrete* method to call.**



### empty interface

The interface type that specifies zero methods is known as the *empty interface*:

```
interface{}
```

An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. For example, `fmt.Print` takes any number of arguments of type `interface{}`.

比如下面例子中的describe函数，为了让它能够接收任意参数，我们声明它的参数为空接口类型：

```go
package main

import "fmt"

func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```



### Type assertion

简而言之就是找到`(value, concrete_type)`里面的`contrete_type `. A *type assertion* provides access to an interface value's underlying concrete value.

```
t := i.(T)
```

This statement asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t`.

If `i` does not hold a `T`, the statement will trigger a panic.

To *test* whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

```
t, ok := i.(T)
```

If `i` holds a `T`, then `t` will be the underlying value and `ok` will be true.

If not, `ok` will be false and `t` will be the zero value of type `T`, and no panic occurs.

Note the similarity between this syntax and that of reading from a map.



```go
package main

import "fmt"

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



### type switches

简而言之，就是允许我们对一个未知的类型进行判断，根据不同的结果，执行不同的逻辑，比如一个函数，接收任意参数，但是对接收数字和字符串时，输出的格式需要有不同。所以我们需要能够进行ifelse的逻辑，在数字和字符串时，进行判断；

```go
package main

import "fmt"

func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}

func main() {
	do(21)
	do("hello")
	do(true)
}
```





### Case study: Stringers interface

类似于Java的`toString()` 以及python的`__str__()`方法，目的就是为了序列化某个数据类型，从而可以有效地输出；

```go
type Stringer interface {
    String() string
}
```



```go
package main

import "fmt"

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
	fmt.Println(a, z)
}
```



再比如我们可以自定义IP地址的输出类型：

```go
package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.
func (ip IPAddr) String() string {
	res := ""
	for i := 0; i < 4; i++ {
		res += fmt.Sprint(ip[i])
		if i != 3 {
			res += "."
		}
	}
	return res
}
func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}
```



## Error

error是一个built-in的interface，用于错误处理，它的定义如下：

```go
type error interface {
    Error() string
}
```

也就是说自定义错误类型，只要实现`Error() string`即可；

而且一般而言，Go的模式在返回时，通常会返回两个值`(value, error)`，这样有助于错误处理；

### define own error
比如我们写一个自定的Sqrt函数，并在输入为负数时返回一个error；

首先定义一个新的错误类型，为了简单起见，我们直接定一个float64的别名：

```go
type ErrNegativeSqrt float64
```

然后让它实现error接口，也就是实现Error函数：

```go
func (e ErrNegativeSqrt) Error() string {
	return "cannot Sqrt negative number:" + fmt.Sprint(float64(e))
}
```

所以Sqrt函数可以这样写：

```go
func Sqrt(x float64) (float64, error) {
	if x < 0 {
		return 0, ErrNegativeSqrt(-2)
	}
	z := x / 2
	old := x
	for {
		z = z + (x-z*z)/(2*z)
		if math.Abs(old-z) < 1e-4 {
			return z, nil
		}
		old = z
	}
	return z, nil

}
```

**Note:** A call to `fmt.Sprint(e)` inside the `Error` method will send the program into an infinite loop. You can avoid this by converting `e` first: `fmt.Sprint(float64(e))`. Why?

因为为了输出e为字符串，我们需要Error函数，而我们现在又在Error函数中，这就出现了无限递归。
