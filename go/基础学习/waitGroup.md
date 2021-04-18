### waitGroup
waitGroup是用于同步多个Goroutine,同步多个协程执行状态和数据的问题
#### waitGroup的作用
* １.waitGrou可以用于等待一系列的Goroutine完成任务
* 2.done方法在新的Goroutine中使用
* 3.Wait方法可以阻塞等待直至其它goroutine完成
示例代码
```
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	var dataChan = make(chan int, 4)
	for i := 0; i < 4; i++ {
		wg.Add(1)
		go send(dataChan, i, &wg)
	}
	wg.Wait()
	close(dataChan)
	for val := range dataChan {
		fmt.Println(val)
	}
}
func send(data chan int, val int, wg *sync.WaitGroup) {
	defer wg.Done()
	data <- val
}
```
