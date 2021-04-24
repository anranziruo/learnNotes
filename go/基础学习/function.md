#### 函数

##### 函数类型
```
声明函数类型
type cal func(int, int,int) int

func add(x, y ,z int) int {
	return x + y
}

凡是满足这个条件的函数都是cal类型的函数

var a calculation
a= add
```
##### 函数作为参数
```
func params(x, y int) int {
	return x + y
}
func calc(x, y int, par1 func(int, int) int) int {
	return par1(x, y)
}
```
##### 函数作为返回值
```
func returnF(x, y int) (func(int)(int)) {
	return func(z int)(int){
		return x+y+z
	}
}
func main() {
	t1:=returnF(12,48)
	fv:=t1(35)
	fmt.Println(fv)
}
```
##### 匿名函数
匿名函数就是没有函数名的函数，用于回调和闭包 格式如下:
```
func(参数)(返回值){
    函数体
}

a:= func(x,y,z int)(int) {
		return x+y+z
	}
fmt.Println(a(20,30,60))
```
##### 闭包
闭包指的是一个函数和与其相关的引用环境组合而成的实体
```
func cal(baseV int) (func(int) int, func(int) int){
	inc := func(i int) int {
		baseV += i
		return baseV
	}
	dec := func(i int) int {
		baseV -= i
		return baseV
	}
	return inc,dec
}

func sub(x int) func(int) int {
	return func(y int) int {
		x += y
		return x
	}
}

func main() {
	s1:=sub(10)
	fmt.Println(s1(20)) //在变量S1被销毁之前，整个闭包环境x变量都是有效的
	fmt.Println(s1(30)) //输出结果是70

	in,de:=cal(20)
	fmt.Println(in(15),de(10))
	fmt.Println(in(8),de(4)) //in,de其实都在类似同一个闭包内.所以base的值都会叠加
}
```




