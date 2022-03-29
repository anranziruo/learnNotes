### 算法如下:
```
滑动窗口协议（Sliding Window Protocol），该协议是 TCP协议 的一种应用，用于网络数据传输时的流量控制，以避免拥塞的发生。该协议允许发送方在停止并等待确认前发送多个数据分组。由于发送方不必每发一个分组就停下来等待确认。因此该协议可以加速数据的传输，提高网络吞吐量

窗口滑动算法:
滑动窗口算法是在给定特定窗口大小的数组或字符串上执行要求的操作,该技术可以将一部分问题中的嵌套循环转变为一个单循环，因此它可以减少时间复杂度。
```
适用的示例:
https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-dong-chuang-kou-by-powcai/
#### 示例 长度最小的子数组
```
给定一个含有 n 个正整数的数组和一个正整数 target 。
找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0

示例1:
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```
基于滑动窗口的解决方案
```
func minSubArrayLen(target int, nums []int) int {
	left := 0
	sum := 0
	maxLen := math.MaxInt32
	for index, _ := range nums {
		sum = sum + nums[index]
		for sum >= target {
			maxLen = minInt(maxLen, index-left+1)
			sum = sum - nums[left]
			left++
		}
	}
	if maxLen == math.MaxInt32 {
		return 0
	}
	return maxLen
}
```

### 无重复最长字符串
```
给定一个字符串s,请你找出其中不含有重复字符的最长子串的长度。
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为3。
```
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