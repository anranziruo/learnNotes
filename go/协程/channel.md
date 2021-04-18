## 内容

### 判断channel是否关闭
```
package main

import (
    "fmt"
)

func main() {
    c := make(chan int, 10)
    c <- 1
    c <- 2
    c <- 3
    close(c)

    for {
        i, isClose := <-c
        if !isClose {
            fmt.Println("channel closed!")
            break
        }
        fmt.Println(i)
    }
}
```