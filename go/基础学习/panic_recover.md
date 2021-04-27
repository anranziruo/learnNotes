### panic
```
panic能够改变程序的控制流，调用panic后会立刻停止执行当前函数的剩余代码，并在当前Goroutine中递归执行调用方的defer
recover可以中止panic造成的程序崩溃。它是一个只能在defer中发挥作用的函数，在其他作用域中调用不会发挥作用
```
#### 示例1
```
package main

import "time"

func main() {
	defer println("in main")
	go func() {
		defer println("in goroutine")
		panic("")
	}()
	time.Sleep(1 * time.Second)
}
$ go run main.go
in goroutine
panic: 
//丛输出可以看到panic影响的只有当前Goroutine,然后执行对应的defer，然后退出整个程序,此时主的Goroutine的defer并未执行
```
panic和defer原理如下图:
![结构图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/panic_defer.png)

#### 示例2
```
func main() {
	defer fmt.Println("in main")
	if err := recover(); err != nil {
		fmt.Println(err)
	}
	panic("unknown err")
}
//示例2是无法recover到因为recover在panic之前，最好的方案将recover放到defer之中
```
#### 示例3
```
package main

import "fmt"

func main() {
	defer fmt.Println("in main")
	defer func() {
		defer func() {
			panic("panic again and again")
		}()
		panic("panic again")
	}()
	panic("panic once")
}
//先执行defer,然后在执行panic once,接下来执行panic once,然后执行panic again and again,
可以确定程序多次调用 panic 也不会影响 defer 函数的正常执行，所以使用 defer 进行收尾工作一般来说都是安全的
```
#### panic的执行原理
编译器会将关键字panic转换成runtime.gopanic，该函数的执行过程包含以下几个步骤
```
1.创建新的 runtime._panic 并添加到所在 Goroutine 的 _panic 链表的最前面；
2.在循环中不断从当前 Goroutine 的 _defer 中链表获取 runtime._defer 并调用 runtime.reflectcall 运行延迟调用函数；
3.调用 runtime.fatalpanic中止整个程序
```
### recover
```
如果调用延迟执行函数时遇到了runtime.gorecover就会将_panic.recovered 标记成 true 并返回 panic 的参数；
在这次调用结束之后，runtime.gopanic 会从 runtime._defer 结构体中取出程序计数器 pc 和栈指针 sp 并调用 runtime.recovery 函数进行恢复程序；
runtime.recovery 会根据传入的 pc 和 sp 跳转回 runtime.deferproc；
编译器自动生成的代码会发现 runtime.deferproc 的返回值不为0，这时会跳回runtime.deferreturn并恢复到正常的执行流程
```
#### 总结
```
panic只会触发当前Goroutine的defer；
recover只有在defer中调用才会生效；
panic允许在defer中嵌套多次调用
```