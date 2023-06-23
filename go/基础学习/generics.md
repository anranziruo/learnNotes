### 泛型的使用

#### 泛型当做interface使用

示例代码如下:
```
package main

import "fmt"

func printSlice[T any](s []T) {
	for _, v := range s {
		fmt.Println(v)
	}
}

func main() {
	printSlice([]int{1, 2, 3})
	printSlice([]string{"a", "b", "c"})
	printSlice([]float64{1.1, 2.2, 3.3})
}
```

#### 泛型限制类型
比如说想做一个求和的函数，这个时候要对类型做限制,
示例代码如下:
```
type SumData interface {
	~int | ~int8 | ~float64 | ~float32 | ~int32 | ~int64
}

func SumSlice[T SumData](datas []T) T {
	var sum T
	for _, data := range datas {
		sum += data
	}
	return sum
}

func main() {
	println(SumSlice([]int{1, 2, 3}))
	println(SumSlice([]int8{1, 2, 3}))
	println(SumSlice([]float64{1.1, 2.2, 3.3}))
	println(SumSlice([]float32{1.1, 2.2, 3.3}))
	println(SumSlice([]int32{1, 2, 3}))
	println(SumSlice([]int64{1, 2, 3}))
}
```
当我们对用一个结构体,去调用会有啥问题,示例代码如下:
```
type User struct {
	Name string
}
func main() {
	println(SumSlice([]User{{"a"}, {"b"}, {"c"}}))
}
报错如下:
User does not satisfy SumData (User missing in ~int | ~int8 | ~float64 | ~float32 | ~int32 | ~int64)
```
当我们尝试,将结构体加入泛型的限制范围，示例代码如下:
```
type SumData interface {
	~int | ~int8 | ~float64 | ~float32 | ~int32 | ~int64 | ~User
}
还是会出现报错:
invalid use of ~ (underlying type of User is struct{Name string})
```
因此可以得出泛型在特定情况不是所有类型，而是有限制的泛型

####  可以自定义用户约束
可以从一段字符内容拼接为示例
示例代码如下:
```
package main

type Stringer interface {
	String() string
}

func concat[T Stringer](vals []T) string {
	var s string
	for _, val := range vals {
		s += val.String()
	}
	return s
}

type User struct {
	Name    string
	Address string
}

func (u User) String() string {
	return u.Name + " " + u.Address
}

func main() {
	users := []User{
		{"张三", "北京"},
		{"李四", "上海"},
		{"王五", "广州"},
	}
	println(concat(users))
}
```
示例代码的含义是只要实现了Stringer的方法，都可以使用concat方法进行拼接，Stringer的interface就相当于给concat方法建立了约束

#### map中使用泛型
```
package main

import (
	"fmt"
)

type CustomMap[k, v comparable] map[k]v

func main() {

	floatMap := CustomMap[float64, string]{}
	floatMap[1.1] = "one point one"
	floatMap[2.2] = "two point two"
	fmt.Println(floatMap)

	stringMap := CustomMap[string, string]{}
	stringMap["one"] = "one"
	stringMap["two"] = "two"
	fmt.Println(stringMap)

	/*
		mapMap := CustomMap[map[string]string, string]{}
			fmt.Println(mapMap)
		    error: invalid map key type map[string]string
	*/

	structMap := CustomMap[struct{ name string }, string]{}
	structMap[struct{ name string }{name: "one"}] = "one"
	structMap[struct{ name string }{name: "two"}] = "two"
	fmt.Println(structMap)

	/*
		structMap1 := CustomMap[struct{ name map[string]string }, string]{}
		structMap1[struct{ name map[string]string }{name: map[string]string{"name": "one"}}] = "one"
		structMap1[struct{ name map[string]string }{name: map[string]string{"name": "two"}}] = "two"
		fmt.Println(structMap1)
		error: invalid map key type struct { name map[string]string }
	*/

	/*
		funcMap := CustomMap[func() string, string]{}
		funcMap[func() string { return "one" }] = "one"
		funcMap[func() string { return "two" }] = "two"
		fmt.Println(funcMap)
		error: invalid map key type func() string
	*/

}
```
通过示例我们发现，comparable的类型对于map,结构体，函数这些不能对比的类型是不支持的

#### 切片中使用
```
package main

type Numeric interface {
	int | int32 | int64 | float32 | float64
}

func sumSlice[T Numeric](s []T) T {
	var sum T
	for _, v := range s {
		sum += v
	}
	return sum
}

func main() {
	s := []int{1, 2, 3, 4, 5}
	println(sumSlice(s))
	s1 := []float32{1.1, 2.2, 3.3, 4.4, 5.5}
	println(sumSlice(s1))
}
```

#### 结构体中使用
```
package main

type Value interface {
	string | int64 | float64
}

type User[T Value] struct {
	Name string
	Age  int
	Data T
}

func main() {
	var user User[int64]
	user.Name = "张三"
	user.Age = 18
	user.Data = 100

	var user2 User[string]
	user2.Name = "李四"
	user2.Age = 20
	user2.Data = "hello"

	println(user.Name, user.Age, user.Data)
	println(user2.Name, user2.Age, user2.Data)
}
```

#### 方法中使用
```
package main

type Value interface {
	string | int64 | float64
}

type User[T Value] struct {
	Name string
	Age  int
	Data T
}

func (u *User[T]) Print() {
	println(u.Name, u.Age, u.Data)
}

func (u *User[T]) SetData(data T) {
	u.Data = data
}

func (u *User[T]) GetData() T {
	return u.Data
}

func main() {
	u := User[string]{}
	u.Name = "John"
	u.Age = 20
	u.SetData("Hello")
	u.Print()
	println(u.GetData())

	u2 := User[int64]{}
	u2.Name = "John"
	u2.Age = 20
	u2.SetData(123)
	u2.Print()
	println(u2.GetData())

	u3 := User[float64]{}
	u3.Name = "John"
	u3.Age = 20
	u3.SetData(123.456)
	u3.Print()
	println(u3.GetData())
}
```





