#### 参数

##### 参数传递
* go的参数传递只有传值
* go的参数只有传值，但是按照参数分为是引用类型还是非引用类型
```
指针、map、slice、chan这些属于引用类型
int、string、struct属于非引用类型
引用类型和传引用是两个概念
```
示例代码如下:
```
package main

import "fmt"

type User struct {
	Name string `json:"name"`
	Age  int32  `json:"age"`
}

func main() {
	var x int32 = 1
	Add(x)
	fmt.Println(x, &x)
	y := &x
	AddPointer(y)
	fmt.Println(&y, x)
	var info User
	info.Name = "xiaoheiwa"
	ModifyS(info)
	fmt.Println(info)
	z := make(map[string]string)
	z["1"] = "1"
	mp := &z
	fmt.Printf("%p\n", mp)
	AddMap(z)
	fmt.Println(mp)
	var c = []int32{1, 2, 3, 4}
	ModifySlice(c)
	fmt.Println(c)
}

func Add(x int32) {
	x = x + 10
}

func AddPointer(y *int32) {
	*y = *y + 10
}

func AddMap(data map[string]string) {
	fmt.Printf("%p\n", &data)
	data["2"] = "2"
}

func ModifyS(datas User) {
	datas.Name = "张超"
}

func ModifySlice(datas []int32) {
	datas[2] = 6
	datas = append(datas, 14)
}
```
通过对于上面的源码我们可以看出区别
#### 可变长参数
可变参数就是一个占位符，你可以将1个或者多个参数赋值给这个占位符，这样不管实际参数的数量是多少，都能交给可变参数来处理，我们看一下可变参数的声明
实例源码:
```
func main() {
    s := []int{1, 2, 3}
	sums();//不传参数可以调用
	sum(1, 2, 3)
	sum(s...)
    //这两种调用方式都是可以的
}
func sum(nums ...int) {
	total := 0
	for _, num := range nums {
		total += num
	}
	fmt.Println(total)
}
```
固定参数搭配可变参数使用时，可变参数要放在固定参数的后面
```
func intS(x int,y...int){
	sum:=0+x
	for _,v:=range y{
		sum+=v
	}
	fmt.Println(sum)
}
intS(12) //输出12
intS(12，1，2，3) //输出18
```

