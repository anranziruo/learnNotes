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