## context包的使用

## 作用
context 用来解决 goruntime 之间退出通知、元数据传递的功能.
context的功能是用来解决多层goruntime的上下文传递
示例代码如下:
```
func main() {
	ctx, cancel := context.WithCancel(context.Background())
	go watch(ctx,"【监控1】")
	go watch(ctx,"【监控2】")
	go watch(ctx,"【监控3】")

	time.Sleep(10 * time.Second)
	fmt.Println("可以了，通知监控停止")
	cancel()
	//为了检测监控过是否停止，如果没有监控输出，就表示停止了
	time.Sleep(5 * time.Second)
}

func watch(ctx context.Context, name string) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println(name,"监控退出，停止了...")
			return
		default:
			fmt.Println(name,"goruntime监控中...")
			time.Sleep(2 * time.Second)
		}
	}
}
```
示例代码说明context包可以控制goruntime进行追踪，这就是Context的控制能力，它就像一个控制器一样，按下开关后，所有基于这个Context或者衍生的子Context都会收到通知，这时就可以进行清理操作了，最终释放goruntime，这就优雅的解决了goruntime启动后不可控的问题。
## Context的继承衍生
context包为我们提供的With系列的函数：
* WithCancel函数，传递一个父Context作为参数，返回子Context，以及一个取消函数用来取消Context。 
* WithDeadline函数，和WithCancel差不多，它会多传递一个截止时间参数，意味着到了这个时间点，会自动取消Context，当然我们也可以不等到这个时候，可以提前通过取消函数进行取消。
* WithTimeout和WithDeadline基本上一样，这个表示是超时自动取消，是多少时间后自动取消Context的意思。
* WithValue函数和取消Context无关，它是为了生成一个绑定了一个键值对数据的Context，这个绑定的数据可以通过Context.Value方法访问到。
### 1.使用context包进行内容的传递,示例代码如下:
```
var key string="name"

func main() {
	ctx, cancel := context.WithCancel(context.Background())
	//附加值
	valueCtx:=context.WithValue(ctx,key,"【监控1】")
	go watch(valueCtx)
	time.Sleep(10 * time.Second)
	fmt.Println("可以了，通知监控停止")
	cancel()
	//为了检测监控过是否停止，如果没有监控输出，就表示停止了
	time.Sleep(5 * time.Second)
}

func watch(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			//取出值
			fmt.Println(ctx.Value(key),"监控退出，停止了...")
			return
		default:
			//取出值
			fmt.Println(ctx.Value(key),"goroutine监控中...")
			time.Sleep(2 * time.Second)
		}
	}
}
```
我们可以使用context.WithValue方法附加一对K-V的键值对，这里Key必须是等价性的，也就是具有可比性；
Value值要是线程安全的。(备注像是map这种就不可以传值)

### 2.context Done方法取消
```
func Stream(ctx context.Context, out chan<- Value) error {
	for {
		v, err := DoSomething(ctx)

		if err != nil {
			return err
		}
		select {
		case <-ctx.Done():

			return ctx.Err()
		case out <- v:
		}
	}
}
```
### 3.context WithTimeout的方法示例
```
ctx, cancel := context.WithTimeout(context.Background(), 50*time.Millisecond)
defer cancel()

select {
case <-time.After(1 * time.Second):
    fmt.Println("overslept")
case <-ctx.Done():
    fmt.Println(ctx.Err()) // prints "context deadline exceeded"
}
```

#### 4.WithDeadline的方法示例
```
d := time.Now().Add(50 * time.Millisecond)
ctx, cancel := context.WithDeadline(context.Background(), d)
defer cancel()

select {
case <-time.After(1 * time.Second):
    fmt.Println("overslept")
case <-ctx.Done():
    fmt.Println(ctx.Err())
}
```
WithTimeout和WithDeadline基本上一样，这个表示是超时自动取消，是多少时间后自动取消Context的意思

##总结
* 1.以Context作为参数的函数方法，应该把Context作为第一个参数，放在第一位。
* 2.给一个函数方法传递Context的时候，不要传递nil，如果不知道传递什么，就使用context.TODO
* 3.Context的Value相关方法应该传递必须的数据，不要什么数据都使用这个传递
* 4.Context是线程安全的，可以放心的在多个goroutine中传递