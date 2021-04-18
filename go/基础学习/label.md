#### 标签
for、switch 或 select 语句都可以配合标签（label）形式的标识符使用，即某一行第一个以冒号（:）结尾的单词（gofmt 会将后续代码自动移至下一行）

#### 标签结合for循环
```
LABEL1:
	for i := 0; i <= 5; i++ {
		for j := 0; j <= 5; j++ {
			if j == 1 {
				continue LABEL1 //continue会跳出本轮循环
			} 
			fmt.Printf("i is: %d, and j is: %d\n", i, j)
		}
	}
fmt.Println(123)
//输出结果
 0 0 
 1 0 
 2 0 
 3 0 
 4 0
 5 0

LABEL2:
	for i := 0; i <= 5; i++ {
		for j := 0; j <= 5; j++ {
			if j == 4 {
				break LABEL2 //直接会结束内层和外层的循环
			}
			fmt.Printf("i is: %d, and j is: %d\n", i, j)
		}
	}
}
fmt.Println(123)
//输出结果
0 1
0 2
0 3
```
#### goto语句
发生读取错误时，使用 goto 来跳出无限读取循环并关闭相应的客户端链接。
```
i:=0
HERE:
	fmt.Println(i)
	i++
	if i==5 {
		return
	}
	fmt.Println(123)
	goto HERE
输出结果:
0
123
1
123
3
123
4
```
