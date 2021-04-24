### defer
```
defer会在当前函数返回前执行传入的函数，它会经常被用于关闭文件描述符、关闭数据库连接以及解锁资源
defer关键字的调用时机以及多次调用 defer 时执行顺序是如何确定的；
defer关键字使用传值的方式传递参数时会进行预计算，导致不符合预期的结果；
```
#### 示例1
```
func main() {
	for i := 0; i < 5; i++ {
		defer fmt.Println(i)
	}
}

$ go run main.go
4
3
2
1
0
这个示例说明后调用的defer函数会先执行,类似于数据结构的堆，先进后出
后调用的 defer函数会被追加到 Goroutine _defer链表的最前面；
运行runtime._defer时是从前到后依次执行；
```
#### 示例2
```
package main

import "fmt"

func calc(index string, a, b int) int { ret := a + b
	fmt.Println(index, a, b, ret)
	return ret
}
func main() {
	a := 1
	b := 2
	defer calc("1", a, calc("10", a, b))
	a=0
	defer calc("2", a, calc("20", a, b))
	b=1
}
$ go run main.go
10 1 2 3
20 0 2 2
2 0 2 2
1 1 3 4
//此示例说明:
1.先计算calc("10", a, b)=>这个时候先输出10，1，2，3，然后第一个defer的a的参数也会被赋值，
此时第一个defer的参数值分别是index=1,a=1,b=3
接下来执行的是calc("20", a, b)，参数分别是,index=20,a=0,b=2,输出20，0，2，2
此时第二个defer的参数值分别是index=2,a=0,b=2
最后依次执行defer就可以，结果就是:
2 0 2 2
1 1 3 4
```
此示例说明的就是:
函数的参数会被预先计算；调用runtime.deferproc函数创建新的延迟调用时就会立刻拷贝函数的参数，函数的参数不会等到真正执行时计算
#### 示例3
```
package main

import "fmt"

func main() {
	var x int = 0
	fmt.Printf("参数x：%p\n", &x)
	defer func(x int) {
		fmt.Printf("参数x：%p\n", &x)
		x++
	}(x)
	fmt.Println(x)
}
//defer内的x和x外边的x不是一个变量，作用域是不一样的
```

#### defer的实现原理
```
defer的数据结构:
type _defer struct {
	siz       int32
	started   bool
	openDefer bool
	sp        uintptr
	pc        uintptr
	fn        *funcval
	_panic    *_panic
	link      *_defer
}
runtime._defer结构体是延迟调用链表上的一个元素，所有的结构体都会通过link字段串联成链表。
新声明的defer总是添加到链表头部,函数返回前执行defer则是从链表首部依次取出执行
一个goroutine可能连续调用多个函数，defer添加过程跟上述流程一致，进入函数时添加defer,离开函数时取出defer，
所以即便调用多个函数，也总是能保证defer是按FIFO方式执行的
```
执行原理如下图:
![结构图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/defer_stru.png)


