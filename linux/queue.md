# 进程之队列学习

## 队列

### 含义是：消息队列提供了一种从一个进程向另一个进程发送一个数据块的方法。每个数据块都被认为含有一个类型，接收进程可以独立地接收含有不同类型的数据结构。我们可以通过发送消息来避免命名管道的同步和阻塞问题。但是消息队列与命名管道一样，每个数据块都有一个最大长度的限制。

python进程的队列的交互
以生产者和消费者为例子

```
# -*- coding:utf-8 -*-
from multiprocessing import Queue,Process,Lock
import time

def product(queue):
    for i in range(0,10):
        time.sleep(1)
        queue.put(i)
    

def consummer(queue):
    for i in range(0,10):
        time.sleep(1)
        print(queue.get())


if __name__ == '__main__':
    q = Queue()
    p1 = Process(target=product, args=(q,))
    p2 = Process(target=consummer, args=(q,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
    p1.terminate()
    p2.terminate()
```