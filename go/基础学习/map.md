## 内容
*[map的实现](https://www.cnblogs.com/maji233/p/11070853.html)
概念
map 是引用类型,在声明的时候不需要知道 map 的长度，map 是可以动态增长的,未初始化的 map 的值是 nil,内存用 make 方法来分配
不要使用 new，永远用 make 来构造 map

### map的值
```
值可以是任意类型的，这里给出了一个使用 func() int 作为值的 map
mf := map[int]func() int{
		1: func() int { return 10 },
		2: func() int { return 20 },
		5: func() int { return 50 },
	}
```

### map的容量
```
map 可以根据新增的 key-value 对动态的伸缩，因此它不存在固定长度或者最大限制,map 增长到容量上限的时候，如果再增加新的 key-value 对，map 的大小会自动加 1。所以出于性能的考虑，对于大的 map 或者会快速扩张的 map
```

### 判断值是否存在
```
如果你只是想判断某个 key 是否存在而不关心它对应的值到底是多少，你可以这么做：

_, ok := map1[key1] // 如果key1存在则ok == true，否则ok为false
或者和 if 混合使用：

if _, ok := map1[key1]; ok {
	// ...
}
```

### 删除key
```
delete(map1, key1)
map 不是按照 key 的顺序排列的，也不是按照 value 的序排列的
```