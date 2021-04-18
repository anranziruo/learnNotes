
### 斐波那契
```
这个数列从第3项开始，每一项都等于前两项之和
```

### 代码如下
```
package main

import "fmt"

func main() {
	var list []int
	for i := 1; i < 5; i++ {
		list = append(list, fibonacci(i))
	}
	fmt.Println(list)
}

func fibonacci(dataLen int) int {
	if dataLen <= 1 {
		return dataLen
	}
	return fibonacci(dataLen-1) + fibonacci(dataLen-2)
}
```