#### 函数定义
```
New 返回一个 Value ，表示指向指定类型的新零值的指针。也就是说，返回值的类型是 PtrTo(typ) 
```

#### 使用示例
```
package main

import (
	"fmt"
	"reflect"
)

type Geek struct {
	A int `tag1:"First Tag" tag2:"Second Tag"`
	B string
}

// Main function
func main() {
	greeting := "GeeksforGeeks"
	f := Geek{A: 10, B: "Number"}


	gpVal := reflect.ValueOf(&greeting)
	gpVal.Elem().SetString("Articles")

	fType := reflect.TypeOf(f)
	fVal := reflect.New(fType)
	fVal.Elem().Field(0).SetInt(20)
	fVal.Elem().Field(1).SetString("Number")
	f2 := fVal.Elem().Interface().(Geek)
	fmt.Printf("%+v, %d, %s\n", f2, f2.A, f2.B)
}
```