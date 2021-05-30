#### sync.map
实现原理
```
1.通过read和dirty两个字段将读写分离，读的数据存在只读字段read 上，将最新写入的数据则存在dirty字段上
2.读取时会先查询read，不存在再查询dirty，写入时则只写入dirty
3.读取read并不需要加锁，而读或写dirty都需要加锁
4.另外有misses字段来统计read被穿透的次数（被穿透指需要读dirty的情况），超过一定次数则将dirty数据同步到read上
5.对于删除数据则直接通过标记来延迟删除

type Map struct {
    // 加锁作用，保护 dirty 字段,是互斥锁
    mu Mutex
    // 只读的数据，实际数据类型为 readOnly
    read atomic.Value
    // 最新写入的数据
    dirty map[interface{}]*entry
    // 计数器，每次需要读 dirty 则 +1
    misses int
}

type readOnly struct {
    // 内建 map
    m  map[interface{}]*entry
    // 表示 dirty 里存在 read 里没有的 key，通过该字段决定是否加锁读 dirty
    amended bool
}
```
#### 总结
```
可见，通过这种读写分离的设计，解决了并发情况的写入安全，又使读取速度在大部分情况可以接近内建map，非常适合读多写少的情况
```



