### 无重复最长字符串
```
package main

import "fmt"

func main() {
	fmt.Println(FindMaxLenString("abcabcbb"))
}

func FindMaxLenString(str string) (string, int) {
	var checkMaps = make(map[string]int)
	var resultMaps = make(map[string]string)
	strLen := len(str)
	if strLen == 1 {
		return str, 1
	}
	tempValue := ""
	for i := 0; i < strLen-1; i++ {
		tempS := string(str[i])
		if _, ok := checkMaps[tempS]; ok {
			tempValue = tempS
		} else {
			tempValue = fmt.Sprintf("%s%s", tempValue, tempS)
		}
		checkMaps[tempS] = i
		resultMaps[tempValue] = tempValue
	}
	maxLen := 0
	maxVal := ""
	for _, val := range resultMaps {
		if len(val) > maxLen {
			maxLen = len(val)
			maxVal = val
		}
	}
	return maxVal, maxLen
}
```