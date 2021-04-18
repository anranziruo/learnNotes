#### break

一个 break 的作用范围为该语句出现后的最内部的结构，它可以被用于任何形式的 for 循环（计数器、条件判断等）。但在 switch 或 select 语句中，break 语句的作用结果是跳过整个代码块，执行后续的代码
```
for i:=0; i<3; i++ {
    for j:=0; j<10; j++ {
        if j>5 {
            break  //只会跳出内层循环
        }
        print(j)
    }
    print("  ")
}
```
#### continue
continue 忽略剩余的循环后代码而直接进入下一次循环的过程，但不是无条件执行下一次循环，执行之前依旧需要满足循环的判断条件
```
for i := 0; i < 10; i++ {
		if i == 5 {
			continue
		}
		print(i)
		print(" ")
	}
```