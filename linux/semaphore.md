# 进程之信号和信号量的学习

## 信号和信号量的区别


### 信号:用来通知进程发生了异步事件

#### 比如说进程的中断，我们通过ctrl+c进行脚本的中断。示例是python代码

```
import signal

def fun(sig, stack_frame):
    print('eixt %d, %s' % (sig,stack_frame))
    exit(1)

while(1):
    signal.signal(signal.SIGINT, fun)
```

#### golang的信号调用

```
import (
	"fmt"
	"os"
	"os/signal"
	"syscall"
)

func main() {
	var sg = make(chan os.Signal, 1)
	signal.Notify(sg, syscall.SIGINT, syscall.SIGHUP)
	fmt.Println("启动")
	s := <-sg
	fmt.Println("退出信号", s)
}
```

### 进程信号量:信号量在创建时需要设置一个初始值，表示同时可以有几个任务可以访问该信号量保护的共享资源，初始值为1就变成互斥锁（Mutex），即同时只能有一个任务可以访问信号量保护的共享资源。
### 一个任务要想访问共享资源，首先必须得到信号量，获取信号量的操作将把信号量的值减1，若当前信号量的值为负数，表明无法获得信号量，该任务必须挂起在该信号量的等待队列等待该信号量可用；若当前信号量的值为非负数，表示可以获得信号量，因而可以立刻访问被该信号量保护的共享资源。
### 当任务访问完被信号量保护的共享资源后，必须释放信号量，释放信号量通过把信号量的值加1实现，如果信号量的值为非正数，表明有任务等待当前信号量，因此它也唤醒所有等待该信号量的任务。

#### python2.7的示例代码
```
#!/usr/bin/python
# -*- coding: UTF-8 -*- 
from multiprocessing import Process,Semaphore
import time,random

def go_wc(sem,user):
    sem.acquire()
    print('%s 占到一个茅坑' %user)
    time.sleep(random.randint(0,3)) #模拟每个人拉屎速度不一样，0代表有的人蹲下就起来了
    sem.release()

if __name__ == '__main__':
    sem=Semaphore(3)
    p_l=[]
    for i in range(10):
        p=Process(target=go_wc,args=(sem,'user%s' %i,))
        p.start()
        p_l.append(p)

    for i in p_l:
        i.join()
    print('============》')
```

### golang使用channel模拟信号的PV和SV的信号操作

```
package main

import (
	"fmt"
	"sync"
)

func main() {
	wg := sync.WaitGroup{}
	wg.Add(3)

	sem := make(chan int, 1)

	for i := 0; i < 3; i++ {
		go func(id int) {
			defer wg.Done()

			sem <- 1

			for x := 0; x < 3; x++ {
				fmt.Println(id, x)
			}

			<-sem
		}(i)
	}

	wg.Wait()
}
```