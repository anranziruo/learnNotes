# select原理

## 作用
select监听进入通道的数据，也可以是用通道发送值的时候
select做的就是：选择处理列出的多个通信情况中的一个。
如果都阻塞了，会等待直到其中一个可以处理
如果多个可以处理，随机选择一个
如果没有通道操作可以处理并且写了 default 语句，它就会执行：default 永远是可运行的（这就是准备好了，可以执行）。
select的case后只能是IO操作
注意事项:
1.当case没有准备好（获取到数据）时， default语句会执行，这样就防止了select的阻塞状态出现
```
package main
import (
	"fmt"
	"time"
)
func process(in chan string) {
	for {
		time.Sleep(4 * time.Second)
		in <- "数据写入"
	}
}
func main(){
	in := make(chan string)

	go process(in)
	for{
		time.Sleep(2*time.Second)
		select{
			case str := <-in :
				fmt.Println("接受到数据：",str)
		default:
			fmt.Println("等待中...")
		}
	}
}
```
2.空的select会造成永久的死锁
```
package main
import (
	"time"
)

func main(){
	select {
	}

}
```
3.多个通道发送channel过来的时候,channel会随机输出

4.优先级的select
```
func worker2(ch1, ch2 <-chan int) {
	for {
		select {
		case job1 := <-ch1:
			fmt.Println(job1)
		case job2:=<-ch2:
		priority:
			for {
				select {
				case job1 := <-ch1:
					fmt.Println(job1)
				default:
					fmt.Println("break")
					break priority
				}
			}
			fmt.Println(job2)
		}
	}
}
```
