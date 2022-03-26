### 无重复最长字符串
代码如下:
```
func lengthOfLongestSubstring(input string) int {
    if len(input)==0 {return 0}
    var hashMap = make(map[byte]int)
	left := 0
	max := 0
	for i := 0; i < len(input); i++ {
		if hashMap[input[i]] > 0 && left < hashMap[input[i]] {
			left = hashMap[input[i]]
		}
		hashMap[input[i]] = i+1
		max = maxInt(max, i-left+1)
	}
	return max
}

func maxInt(x, y int) int {
	z := x
	if x < y {
		z = y
	}
	return z
}
```
### 算法如下:
```
滑动窗口协议（Sliding Window Protocol），该协议是 TCP协议 的一种应用，用于网络数据传输时的流量控制，以避免拥塞的发生。该协议允许发送方在停止并等待确认前发送多个数据分组。由于发送方不必每发一个分组就停下来等待确认。因此该协议可以加速数据的传输，提高网络吞吐量

窗口滑动算法:
滑动窗口算法是在给定特定窗口大小的数组或字符串上执行要求的操作,该技术可以将一部分问题中的嵌套循环转变为一个单循环，因此它可以减少时间复杂度。
```
