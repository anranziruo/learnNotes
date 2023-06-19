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