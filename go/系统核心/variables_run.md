### 内容

```
逃逸分析是一种确定指针动态范围的方法——分析在程序的哪些地方可以访问到指针。也是就是说逃逸分析是解决指针作用范围的编译优化方法。编程中常见的两种逃逸情景：
1.函数中局部对象指针被返回（不确定被谁访问）
2.对象指针被多个子程序（如线程 协程）共享使用
```
#### 指针逃逸
```
package main

func main() {
	test()
	return
}
func test() *int {
	var a = 10
	return &a
}
```
#### 栈空间逃逸
```
package main

func main() {
	run()
	return
}
func run() {
	s := make([]int, 10000, 10000)
	for i := 0; i < len(s); i++ {
		s[i] = i
	}
	return
}
```
#### 动态类型逃逸
```
package main

import "fmt"

func main() {
	t := make([]int, 10)
	for i := 0; i < 10; i++ {
		t[i] = i
	}
	run(t)
}
func run(s interface{}) {
	fmt.Println(s)
}
```
