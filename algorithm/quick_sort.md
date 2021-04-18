#### 快速排序
```
先从数列中取出一个数作为基准数。
分区过程，将比这个数大的数全放到它的右边，小于或等于它的数全放到它的左边。
再对左右区间重复第二步，直到各区间只有一个数。
快速排序采用的是分治算法
```
示例代码
```
func quickSort(values []int, left int, right int) {

  if left < right {

    // 设置分水岭

    temp := values[left]

    // 设置哨兵

    i, j := left, right

    for {

      // 从右向左找，找到第一个比分水岭小的数

      for values[j] >= temp && i < j {

        j--

      }

      // 从左向右找，找到第一个比分水岭大的数

      for values[i] <= temp && i < j {

        i++

      }

      // 如果哨兵相遇，则退出循环

      if i >= j {

        break

      }
    values[i], values[j] = values[j], values[i]

    }
    values[left] = values[i]
    values[i] = temp
    quickSort(values, left, i-1)
    quickSort(values, i+1, right)
  }
}
```