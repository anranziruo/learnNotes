#### 选择排序
```
基于键值的且交换操作只有在需要的时候才进行操作,重复选择最小的元素
```
#### 代码示例如下:
```
func SelectSort(values []int) {
    length := len(values)
    if length <= 1 {
        return
    }

    for i := 0; i < length; i++ {
        min := i // 初始的最小值位置从0开始，依次向右

        // 从i右侧的所有元素中找出当前最小值所在的下标
        for j := length - 1; j > i; j-- {
            if values[j] < values[min] {
                min = j
            }
        }
        values[i], values[min] = values[min], values[i]
    }
}
```