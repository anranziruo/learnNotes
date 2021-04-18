# 切片和数组

## 数组
golang数组和c的数组类似，按照内存空间进行存储的,数组的长度是固定的,数组是值拷贝的，通过示例可以看下:
```
func main() {
	c := [3]int{1, 2, 3}
	d := c
	c[0] = 999
	fmt.Println(d)
}
```
## 切片
切片底层的实现原理:
```
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```
slice的底层结构由一个指向数组的指针ptr和长度len，容量cap构成，也就是说slice的数据存在数组当中
* 当append后，slice长度不超过容量cap，新增的元素将直接加在数组中
* 当append后，slice长度超过容量cap，将会返回一个新的slice
示例代码如下:
```
代码1:
    c := [3]int{1, 2, 3}
	d := c[0:2]
    //d这个时候是切片,d和ｃ是共用的是同一个内存空间
    //c = append(c,1,2),执行append以后，长度超过cap，生成新的slice,内存地址发生了变化
	c[0] = 999
	fmt.Println(d)//这个时候ｄ的值被改变了
代码２:
    c := []int{1, 2, 3}
	d := c[0:2]
	c[0] = 999
	fmt.Println(d)
```
两个判断执行的结果是一样的，谨记slice的底层结构是指针数组，并且len和cap是值类型。
Go的函数传参都是值拷贝传递
* [参考文章](https://www.flysnow.org/2018/12/21/golang-sliceheader.html)