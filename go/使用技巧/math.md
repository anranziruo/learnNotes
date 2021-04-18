#### math.ceil
```
a := 1000 / 2000
log.Printf("%T", a) //a是int型
fmt.Println(math.Ceil(float64(a)))
fmt.Println(math.Ceil(float64(1000) / float64(2000)))//计算分页的时候,使用这个函数
```
