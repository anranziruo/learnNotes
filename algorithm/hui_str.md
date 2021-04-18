### 最长回文字符串

### 定义
```
"回文串”是一个正读和反读都一样的字符串
```

```
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(MaxStringLen("babad"))
}

func MaxStringLen(s string) (string, int) {
	s = strings.Trim(s, " ")
	strLen := len(s)
	if strLen == 0 {
		return "", 0
	}
	var infoMaps = make(map[string]int)
	for i := 0; i < strLen; i++ {
		for j := i + 1; j <= strLen; j++ {
			tempStr := s[i:j]
			infoMaps[tempStr] = len(tempStr)
		}
	}
	tempLen := 0
	tempV := ""
	for key, val := range infoMaps {
		res := ""
		for i := len(key) - 1; i >= 0; i-- {
			res = fmt.Sprintf("%s%s", res, string(key[i]))
		}
		if val > tempLen && key == res {
			tempLen = val
			tempV = key
		}
	}
	return tempV, tempLen
}
```