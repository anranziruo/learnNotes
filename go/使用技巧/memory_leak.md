## 内容

### 内存泄漏概念
```
由于疏忽或错误造成程序未能释放已经不再使用的内存。内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，导致在释放该段内存之前就失去了对该段内存的控制，从而造成了内存的浪费
```

### time.after导致的内存泄漏
```
场景一:
for{
    select {
        case <-time.After(time.Second*10):
            fmt.Println("sleep")
    default:
        fmt.Println("6666666")
    }
}
原因是:如果定时器没有达到预计的时间，gc就不会启动内存回收
解决方案:
tim:=time.NewTicker(time.Second*10)
defer tim.Stop()
for{
    select {
        case <-tim.C:
            fmt.Println("sleep")
    }
}
```
### http的response的body未关闭
```
示例代码1:
for{
    resp, _ := http.Get("https://www.baidu.com")
    fmt.Println(resp)
    time.Sleep(time.Second*3)
    fmt.Println(runtime.NumGoroutine())sss
}
第一:resp.Body没有关闭，会导致内存泄漏
第二:会发现这个时候，协程的数量是奇数增加，依次是3，5，7，9
原因是:http请求底层实现的时候，每次请求的时候，会产生两个协程，协程退出会和resp.Body.close()有关系,所以协程会加2
示例代码2:
for i := 0; i < 6; i++ {
    resp, _ := http.Get("https://www.baidu.com")
    _, _ = ioutil.ReadAll(resp.Body)
}
fmt.Println(runtime.NumGoroutine())
此时输出的数量是3，分别是一个读协程，一个是写协程，和一个主协程
```
[参考文档(https://studygolang.com/articles/31717)
