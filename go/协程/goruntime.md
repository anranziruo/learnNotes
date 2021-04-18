# goruntime 原理

## CSP并发模型
CSP并发模型是在1970年左右提出的概念，属于比较新的概念，不同于传统的多线程通过共享内存来通信，CSP讲究的是“以通信的方式来共享内存”。

Go的CSP并发模型，是通过goroutine和channel来实现的。

* 1.goroutine 是Go语言中并发的执行单位。有点抽象，其实就是和传统概念上的”线程“类似，可以理解为”线程“。
* 2.channel是Go语言中各个并发结构体(goroutine)之前的通信机制。 通俗的讲，就是各个goroutine之间通信的”管道“，有点类似于Linux中的g管道。
* 3.通信机制channel也很方便，传数据用channel <- data，取数据用<-channel。
在通信过程中，传数据channel <- data和取数据<-channel必然会成对出现，因为这边传，那边取，两个goroutine之间才会实现通信。
而且不管传还是取，必阻塞，直到另外的goroutine传或者取为止。

## 含义
M:代表内核线程，一个M直接关联了一个内核线程
P:代表执行Go代码所需的资源，代表了M所需的上下文环境，也是处理用户级代码逻辑的处理器
G:代表一个Ｇo代码片段，其实本质上也是一种轻量级的线程。

三者关系如下图所示：
![avatar](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/68747470733a2f2f69363434383033382e6769746875622e696f2f696d672f6373702f474d5072656c6174696f6e2e706e67.png)

## 详解:
个M会对应一个内核线程，一个M也会连接一个上下文P，一个上下文P相当于一个“处理器”，一个上下文连接一个或者多个Goroutine。P(Processor)的数量是在启动时被设置为环境变量GOMAXPROCS的值，或者通过运行时调用函数runtime.GOMAXPROCS()进行设置。Processor数量固定意味着任意时刻只有固定数量的线程在运行go代码。Goroutine中就是我们要执行并发的代码。图中P正在执行的Goroutine为蓝色的；处于待执行状态的Goroutine为灰色的，灰色的Goroutine形成了一个队列runqueues

## M的作用
在这一块go运行的单个M是可以设置的，可以使用标准包runtime/debug包中的setMaxThreads进行设置
Go语言的线程模型就是一种特殊的两级线程模型
M与KSE之间是一对一的关系,-个M仅能代表一个内核线程。M在创建之初，会被加入到全局的M列表。
## p的作用
Go的运行系统会让p和不同的M建立或者断开，因此可以让p中的G获取运行良机。P是有状态的，P的状态有如下几种:
Pidle:表示p没有和任何M有任何关联
PRuning:表示P和某个M有关联
Psyscall:当前运行P和某个G有关联
Pgcstop:运行系统需要停止调度
Pdead:p已经不会被使用
```
一般我们在main函数开始的时候，使用runtime.GOMAXPROCX函数进行限制。因为设置P有很大的性能消耗
```
## G的作用
G列表分为全局G列表,调度器的可运行G列表,自由G列表
其中包括goid是这个goroutine的ID，status是这个goroutine的状态，如Gidle,Grunnable,Grunning,Gsyscall,Gwaiting,Gdead等。
## Goroutine的均衡工作
上下文P会定期的检查全局的goroutine 队列中的goroutine，以便自己在消费掉自身Goroutine队列的时候有事可做。假如全局goroutine队列中的goroutine也没了呢？就从其他运行的中的P的runqueue里偷。
![示例图](https://github.com/zhangchao1/learnNotes/blob/master/assets/go/stealwork.png)‘

## golang调度器




