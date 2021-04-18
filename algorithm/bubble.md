### 冒泡排序
```
冒泡算法指的是将键值较小的元素如同气泡一样漂浮到序列的前端
```
示例代码如下:
```
func BubbleAsort(datas []int) []int {
	for i := 0; i < len(datas)-1; i++ {
		for j := i + 1; j < len(datas); j++ {
			if datas[i] > datas[j] {
				datas[i], datas[j] = datas[j], datas[i]
			}
		}
	}
	return datas
}
```
