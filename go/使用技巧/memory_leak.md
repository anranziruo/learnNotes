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
