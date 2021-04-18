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
##### defer语句
defer语句会将其后面跟随的语句进行延迟处理。在defer归属的函数即将返回时，将延迟处理的语句按defer定义的逆序进行执行，也就是说，先被defer的语句最后被执行，最后被defer的语句，最先被执行
```
return x => 返回值=x
            defer 语句
            RET 语句
func f1() int {
	x := 5
	defer func() {
		x++
	}()
	return x
} // 这个时候输出是5,先执行return x,但是在执行defer的时候,已经return

func f2() (x int) {
	defer func() {
		x++
	}()
	return 5
}// 这个时候输出是6,return的结果是5，对于defer来说,x相当于f2的内部变量，所以这个时候结果是6

func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x
}// 输出是5 return x的结果就确认了
func f4() (x int) {
	defer func(x int) {
		x++
	}(x)
	return 5
}//输出的结果是5,defer里面的x作用域只在defer里面
func main() {
	fmt.Println(f1())
	fmt.Println(f2())
	fmt.Println(f3())
	fmt.Println(f4())
}
```
##### panic和recover
panic可以在任何地方引发，但recover只有在defer调用的函数中有效。
```
func funcDemo1() {
	fmt.Println("func demo1")
}

func funcDemo2() {
	defer func() {
		err := recover()
		//如果程序出出现了panic错误,可以通过recover恢复过来
		if err != nil {
			fmt.Println("recover in demo2")
		}
	}() 
	panic("panic in B")
}//没有defer的话,无法捕获异常

func funcDemo3() {
	fmt.Println("func demo3")
}
func main() {
	funcA()
	funcB()
	funcC()
}
``


