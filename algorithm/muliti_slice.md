#### 多维数组搜索
```
package main

import "fmt"

func main() {
	var datas = [][]int{
		{0, 1, 2, 3},
		{4, 5, 6, 7},
		{8, 9, 10, 11},
	}
	fmt.Println(FindValFromArr(datas, -1))
}

func FindValFromArr(datas [][]int, findval int) bool {
	row := len(datas)
	if row == 0 {
		return false
	}
	column := len(datas[0])
	if column == 0 {
		return false
	}
	i := 0
	j := 0
	hasExist := false
	for j < column-1 && i < row-1 {
		if datas[i][j] == findval {
			hasExist = true
			break
		}
		if datas[i][j] < findval {
			j++
		} else {
			i++
		}
	}
	return hasExist
}
```