### return
```
defer、return、返回值三者的执行逻辑应该是：return最先执行，return负责将结果写入返回值中；接着defer开始执行一些收尾工作；最后函数携带当前返回值退出
return 并非原子操作,分为赋值,和返回值两步操作
```
结构图如下:
![执行步骤](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/defer_stru.png)

#### 示例1
```
package main

import "fmt"

func fun2() (ret int) { //这里指定了返回值是ret
	defer func() {
		ret++
	}()
	return 5
}

func main() {
	fmt.Println(fun2())
}
$ go run main.go
6
//先执行return操作，此时因为返回值ret的变量,在defer函数中进行自增操作，这个时候ret的值就变成6了
```
#### 示例2
```
package main

import "fmt"

func fun4() (x int) {
	fmt.Printf("返回值x：%p\n", &x)
	defer func(x int) {
		x++
		fmt.Printf("匿名函数内的x：%p\n", &x)
	}(x)
	return 5
}
func main() {
	fmt.Println(fun4())
}
$ go run main.go
5
//
这个时候返回值x和defer函数内的x不是同一个参数,defer函数内的x=0,所以这个时候defer函数的执行不会影响整个fun4函数的返回值
```