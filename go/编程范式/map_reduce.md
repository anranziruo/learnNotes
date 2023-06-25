### map和reduce

#### map的示例

```
package main

import "strings"

func mapToStr(datas []string, fn func(str string) string) []string {
	var result []string
	for _, str := range datas {
		result = append(result, fn(str))
	}
	return result
}

func main() {
	var datas = []string{"apple", "banana", "orange"}
	var result = mapToStr(datas, func(str string) string {
		return strings.ToUpper(str)
	})
	for _, str := range result {
		println(str)
	}
}
```
#### reduce的示例
```
package main

func reduceStr(datas []string, fn func(s string) string) string {
	var result string
	for _, data := range datas {
		result += fn(data)
	}
	return result
}
func main() {
	var datas = []string{"hello", "world", "!"}
	var result = reduceStr(datas, func(s string) string {
		return s + " "
	})
	println(result)
}
```
#### filter的示例
```
package main

import "strings"

func filterStr(datas []string, fn func(s string) bool) string {
	var result string
	for _, data := range datas {
		if !fn(data) {
			result += data
		}
	}
	return result
}
func main() {
	var datas = []string{"hello", "world", "!"}
	var result = filterStr(datas, func(s string) bool {
		return strings.Contains(s, "!")
	})
	println(result)
}
```
通过示例说明,map,reduce,filter只是控制逻辑，控制逻辑和业务逻辑分离


