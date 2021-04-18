#### 插入排序
```
顺序从序列中取一个数与左侧的元素们做比较，如果左侧的元素比取的数大，就向右移，直到把取的数插入到不小于左侧元素的位置处
```
示例代码如下:
```
func InsertionSort(s []int) {
    n := len(s)
    if n < 2 {
        return
    }
    for i := 1; i < n; i++ {
        for j := i; j > 0 && s[j] < s[j - 1]; j-- {
            swap(s, j, j - 1)
        }
    }
}
 
func swap(slice []int, i int, j int) {
    slice[i], slice[j] = slice[j], slice[i]
}
```