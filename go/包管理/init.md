# init函数

## 作用
* 变量初始化
* 检查和修复程序状态
* 运行前注册，例如decoder，parser的注册
* 运行只需计算一次的模块，像sync.once的作用
* 每个golang源文件中都可以定义一个init函数。golang系统中，所有的源文件都有自己所属的目录，每一个目录都有对应的包名。在包的引用中，一旦某一个包被使用，则这个包下边的init函数将会被执行，且只执行一次.如果一个包被多个地方引用，那么只有在这个包第一次被引用时，才会执行这个包里边的init函数，其他地方对包的再次引用，这个包里边的init函数不会被执行。
## 特点
* init函数不需要传入参数也没有返回值，而且init函数是不能被其他函数调用的。
* 导入的包中前面加了个下划线”_“，这表示只是想执行包中的init函数。
* 每个包可以有多个init函数；
示例代码:
```
package main  
import (
    "fmt"
    _ "./level1"
)

func main() {
    fmt.Println("I am in main")
}  
```

```
// level2.go  
package level2 

import "fmt"  

func init() {
    fmt.Println("I am in level2")
} 
```

```
// level1.go
package level1  

import (
    "fmt"
    _ "../level2"
)

func init() {
    fmt.Println("I am in level1")
}  
```
输出结果:
```
I am in level2
I am in level1
I am in main
```
golang的init函数,go中不同包中init函数的执行顺序是根据包的导入关系决定的。
