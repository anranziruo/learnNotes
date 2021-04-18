### new和make的区别

#### make
make用于内存分配,但是只用于chan,map以及切片的内存创建，它返回的类型就是这三个类型的本身，不是他们的指针类型。
map,slice,chan都是属于引用类型
```
func make(t Type, size ...IntegerType) Type
```
#### new
```
func new(Type) *Type
```
它只接受一个参数，这个参数是一个类型，分配好内存后，返回一个指向该类型内存地址的指针。同时请注意它同时把分配的内存置为零，也就是类型的零值。
它返回的永远是类型的指针，指向分配类型的内存地址

#### 引用类型
引用类型的零值是nil
int值的零值是０,string值的零值是“”

#### 共同点
new和make都用于分配内存