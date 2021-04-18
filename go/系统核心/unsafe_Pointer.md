### unsafe_pointer
* unsafe.Pointer是一种特殊意义的指针，它可以包含任意类型的地址，有点类似于C语言里的void*指针，全能型的
* go的指针不能进行数学运算
* 不同类型的指针不能相互转换
* 不同类型的指针不能使用 == 或 != 比较
#### 设计unsafe包的前景
Go 语言类型系统是为了安全和效率设计的，有时，安全会导致效率低下。有了 unsafe 包，高阶的程序员就可以利用它绕过类型系统的低效。因此，它就有了存在的意义，阅读 Go 源码，会发现有大量使用 unsafe 包的例子。

##### unsafe 包提供了 2 点重要的能力：
* 1.任何指针都可以转换为unsafe.Pointer
* 2.unsafe.Pointer可以转换为任何指针
* 3.任何类型的指针和 unsafe.Pointer 可以相互转换。
* 4.uintptr 类型和 unsafe.Pointer 可以相互转换。
##### 
* pointer 不能直接进行数学运算，但可以把它转换成 uintptr，对 uintptr 类型进行数学运算，再转换成 pointer 类型。
##### uintptr可以，所以我们可以把指针转为uintptr再进行偏移计算，这样我们就可以访问特定的内存了，达到对不同的内存读写的目的
```
func main() {
	u := new(user)
	fmt.Println(*u)

	pName := (*string)(unsafe.Pointer(u))
	*pName = "张三"

	pSex := (*string)(unsafe.Pointer(uintptr(unsafe.Pointer(u)) + unsafe.Offsetof(u.sex)))
	*pSex = "男"

	pAge := (*int)(unsafe.Pointer(uintptr(unsafe.Pointer(u)) + unsafe.Offsetof(u.age)))
	*pAge = 20

	fmt.Println(*u)
}

type user struct {
	name string
	sex  string
	age  int
}
```
#### 源码分析
* 第一个修改user的name值的时候，因为name是第一个字段，所以不用偏移，我们获取user的指针，然后通过unsafe.Pointer转为*string进行赋值操作即可
* 第二个修改user的age值的时候，因为age不是第一个字段，所以我们需要内存偏移，内存偏移牵涉到的计算只能通过uintptr，所我们要先把user的指针地址转为uintptr，然后我们再通过unsafe.Offsetof(u.age)获取需要偏移的值，进行地址运算(+)偏移即可
* 现在偏移后，地址已经是user的age字段了，如果要给它赋值，我们需要把uintptr转为指针int才可以。所以我们通过把uintptr转为unsafe.Pointer,再转为*int就可以操作了
```
temp:=uintptr(unsafe.Pointer(u))+unsafe.Offsetof(u.age)
pAge:=(*int)(unsafe.Pointer(temp))
*pAge = 20
```
但是这里会牵涉到GC，如果我们的这些临时变量被GC，那么导致的内存操作就错了，我们最终操作的，就不知道是哪块内存了，会引起莫名其妙的问题

#### 计算内存的大小
Sizeof函数可以返回一个类型所占用的内存大小，这个大小只有类型有关，和类型对应的变量存储的内容大小无关
比如bool型占用一个字节、int8也占用一个字节。
#### 内存对齐
Alignof返回一个类型的对齐值，也可以叫做对齐系数或者对齐倍数。对齐值是一个和内存对齐有关的值，合理的内存对齐可以提高内存读写的性能，
