# Golang basic

- [variable](#variable)
  - [define, declare](#define-declare)
  - [basic type](#basic-type)
  - [constant](#constant)
- [loop](#loop)
- [condition](#condition)
- [defer](#defer)
- [function](#function)
- [pointer](#pointer)
- [struct](#struct)
- [array & slice](#array--slice)
  - [Slice literals](#slice-literals)
  - [Slice length and capacity](#slice-length-and-capacity)
  - [Creating a slice with make](#creating-a-slice-with-make)
- [map](#map)
  - [Map literals](#map-literals)
  - [map operations](#map-operations)
- [closure](#closure)

```go
package main

import "fmt"

func main() {
  fmt.Println("Hello Golang")
}
```

## variable

### define, declare
```go
var a int
var x, y float32

// Short variable declarations
m, n := 1, 2
```

### basic type
```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

### constant
```
Constants are declared like variables, but with the const keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the := syntax.
```

## loop

- for loop
```go
for i:=1; i <= 100; i++ {}
```

- while loop
```go
for i <= 100 {}
```

- infinite loop
```go
for {}
```
## condition

- if 
```go
if x {}

// do some computation before condition
if x := math.Sqrt(n); x < 10 { }

if cond1 {}
else if cond2 {}
else {}
```
- switch, case has implicit `break`
```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}

```

## defer

类似js的异步函数执行方式，在当前函数执行完毕之后才会执行。

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}

```


## function

```go
func add(x int, y int) int {

}

// simple form
func add(x, y int) int {

}

// return multiple
func swap(x, y string) (string, string) {
  return y, x
}

```


## pointer

Go has pointers. A pointer holds the memory address of a value. The type `*T` is a pointer to a `T` value. Its zero value is nil.

```go
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}

```

## struct

A struct is a collection of fields.
```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

## array & slice

array是不可变长的。The type `[n]T` is an array of n values of type T.

```go
var a [10]int
```
An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

The type []T is a slice with elements of type T.

A slice is formed by specifying two indices, a low and high bound, separated by a colon:

```go
a[low : high]
```
从数组中创建切片，切片只是对应位置的引用，对其值的改变会反映到原始数组中。
```go
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}
```

Slices are like references to arrays
A slice does not store any data, it just describes a section of an underlying array.

Changing the elements of a slice modifies the corresponding elements of its underlying array.

Other slices that share the same underlying array will see those changes.


### Slice literals
A slice literal is like an array literal without the length.

This is an array literal:
```go
[3]bool{true, true, false}
```
And this creates the same array as above, then builds a slice that references it:
```go
[]bool{true, true, false}
```

### Slice length and capacity
A slice has both a length and a capacity.
- The length of a slice is the number of elements it contains.
- The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.
- The length and capacity of a slice s can be obtained using the expressions len(s) and cap(s).

You can extend a slice's length by re-slicing it, provided it has sufficient capacity. 

### Creating a slice with make
Slices can be created with the built-in make function; this is how you create dynamically-sized arrays.

The make function allocates a zeroed array and returns a slice that refers to that array:
```go
a := make([]int, 5)  // len(a)=5
To specify a capacity, pass a third argument to make:

b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

```go
package main

import (
	"golang.org/x/tour/pic"
)

func Pic(dx, dy int) [][]uint8 {
	var res [][]uint8
	for i := 0; i < dy; i++ {
		var line []uint8
		for j := 0; j < dx; j++ {
			line = append(line, uint8((i - j)>>1))
		}
		res = append(res, line)
	}
	return res
}

func main() {
	pic.Show(Pic)
}
```

## map

A map maps keys to values. The zero value of a map is nil. A nil map has no keys, nor can keys be added.

The make function returns a map of the given type, initialized and ready for use.

有点像`defaultdict`
```go
m := make(map[string]int)

```

默认值为value类型的空值
```go
func WordCount(s string) map[string]int {
  words := strings.Split(s, " ")
  m := make(map[string]int)
  for _, word := range words {
    m[word] = m[word] + 1
  }
  return m
}
```

### Map literals
Map literals are like struct literals, but the keys are required.
```go
type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```

Omit type

```go
type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

### map operations

- exist
```go
elem, exist := m[key]
if exist { ... }
```
- insert or update
```go
m[key] = elem
```
- delete
```go
delete(m, key)
```


## closure
Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.

感觉closure持有的变量有点像类的私有变量一样，使它自己私有的一个状态。比如下面的例子中，`a, b`是函数自带的状态，每次调用会更新。感觉还是挺有用的，比如可以统计函数调用次数，或者维持一个持续状态。

- Fibonacci closure
```go
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
	
	a, b := 0, 1
	return func () int {
		res := a
		a, b = b, a + b
		return res
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}

```


