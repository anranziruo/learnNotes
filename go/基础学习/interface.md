#### 接口

接口（interface）是一种类型，一种抽象的类型。
interface是一组method的集合,接口做的事情就像是定义一个协议（规则），不关心属性（数据），只关心行为（方法）。
一个对象只要全部实现了接口中的方法，那么就实现了这个接口。换句话说，接口就是一个需要实现的方法列表

```

type PrintS interface {
	PrintString()
}

type An1 struct {}

type An2 struct {}

func (an1 An1) PrintString(){
	print("an1")
}

func (an2 An2) PrintString(){
	print("an2")
}

func main() {
	var x1 PrintS
	a:=An1{}
	x1=a
	x1.PrintString() //a实现 PrintString，就实现x1接口
	
	b:=An2{}
	x1=b
	x1.PrintString()
}
```
##### 嵌入式接口
```
type Reader interface {
	Read(p []byte) (n int, err error)
}

type Writer interface {
	Write(p []byte) (n int, err error)
}

type Closer interface {
	Close() error
}

type ReadWriteCloser interface {
	Reader
	Writer
	Closer
} //嵌套接口

var w io.Writer
w = os.Stdout
w= time.Time{}  //这种属于非法的调用，因为没有实现接口的方法
n,err :=w.Write([]byte("122333"))
fmt.Println(n,err)


type CR7 struct {
    name string
}

func main(){
    var w io.WriteCloser
    w = new(CR7) //类似于这种调用，也是非法调用，因为cr7这个结构体未实现w的所有方法
    fmt.Println(w.Write([]byte("12222")))
}

func (s *CR7) Write(p []byte) (n int, err error){
    n = len(p)
    return n,nil
}

//只有当增加了close的方法，就可以调用
func (s *CR7) Close() (err error){
   return nil
}

//使用示例如下
var s bytes.Buffer
s.WriteString("牛逼的人")
fmt.Fprintf(&s,"%s%s","你好","小名")
fmt.Println(s.String())

var s1=strings.NewReader("11111 22222")
fmt.Fscan(s1)
fmt.Println(s1.Len())
```

#### 接口断言
```
type A struct {
   Name string
}

type WriteSyncer interface {
   io.Writer
   Sync() error
   String() string
}

type writerWrapper struct {
   io.Writer
}

func (w writerWrapper) Sync() error {
   return nil
}

func (w writerWrapper) String() string {
   return ""
}

//A结构体实现了io.Writer方法，然后writerWrapper实现Sync的方法和String的方法，就相当于实现了WriteSyncer的接口，所以可以实现类型断言
func Adds (w io.Writer) WriteSyncer{
   switch w:=w.(type){
      case WriteSyncer:
         return w
      default:
            return writerWrapper{w}
   }
}

func (s *A) Write(p []byte)(int,error){
   s.Name = string(p)
   return 0,nil
}
```




