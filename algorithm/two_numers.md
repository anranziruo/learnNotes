### 两数之和
```
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出和为目标值target的那两个整数，并返回它们的数组下标。
示例1:
nums = [2,7,11,15], target = 9
输出[0,1]
```
代码如下:
```
func sumTwoInt(nums []int, target int) []int {
	var hashTable = map[int]int{}
	for index, val := range nums {
		if p, ok := hashTable[target-val]; ok {
			return []int{p, index}
		}
		hashTable[val] = index
	}
	return nil
}
```
升级版如下：
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出和为目标值target的整数
代码如下:
```
func twoSum(nums []int, target int) []int {
	var checkMap = make(map[int][]int)
	var cota = make([]int, 0)
	for index, val := range nums {
		de := target - val
		if index > 0 {
			if checkVals, ok := checkMap[val]; ok {
				temp := make(map[int]int)
				cota = append(cota, checkVals...)
				for _, indexVal := range checkVals {
					temp[indexVal] = indexVal
				}
				if _, tok := temp[index]; !tok {
					cota = append(cota, index)
				}
			}
		}
		checkMap[de] = append(checkMap[de], index)
	}
	return cota
}
```