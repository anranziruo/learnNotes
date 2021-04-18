## 方法和函数

### 函数
```
函数的定义声明没有接收者,
这个函数名称是小写开头的add，所以它的作用域只属于所声明的包内使用，不能被其他包使用，如果我们把函数名以大写字母开头，该函数的作用域就大了，可以被其他包调用
```
### 非指针方法
```
使用值类型接收者定义的方法，在调用的时候，使用的其实是值接收者的一个副本，所以对该值的任何操作，不会影响原来的类型变量。
示例代码如下:
type person struct {
	name string
}

func (p person) String() string{
	return "the person name is "+p.name
}

func (p person) modify(){
	p.name = "李四"
}
```

### 指针方法
```
使用指针类型定义方法:指针接收者传递的是一个指向原值指针的副本，指针的副本，指向的还是原来类型的值，所以修改时，同时也会影响原来类型变量的值。
示例如下:
type person struct {
	name string
}

func (p person) String() string{
	return "the person name is "+p.name
}

func (p *person) modify(){
	p.name = "李四"
}
```



