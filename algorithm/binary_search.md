## 内容

```
二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法。但是，折半查找要求线性表必须采用顺序存储结构，而且表中元素按关键字有序排列
```
### 算法要求：
```
1.必须采用顺序存储结构。
2.必须按关键字大小有序排列。
二分查找的思路: 比如我们要查找的数是 findVal
1. arr 是一个有序数组,并且是从小到大排序
2. 先找到 中间的下标 middle = (leftIndex + rightIndex) / 2, 然后让 中间下标的值和 findVal 进行
3.然后递归调用
```
### 示例代码
```
package main

import "fmt"

func main() {
	test := []int{1, 2, 3, 6, 9, 10}
	BinaySearch(test, 0, len(test)-1, -1)
}

func BinaySearch(datas []int, leftLen, rightLen, findVal int) {
	if leftLen > rightLen {
		fmt.Println("no find val")
		return
	}
	middLen := (leftLen + rightLen) / 2
	if datas[middLen] > findVal {
		BinaySearch(datas, leftLen, middLen-1, findVal)
	} else if datas[middLen] < findVal {
		BinaySearch(datas, middLen+1, rightLen, findVal)
	} else {
		fmt.Println("find val")
	}
}
```
