# 进程之fork学习

## fork

### fork进程对父进程的复制：

#### python的fork进程
相同点:
    1.堆指针，栈指针和标志寄存器的数值是相同的
    2.子进程代码和父进程相同，同时复制父进程的数据，采用的模式是写时复制
    3.当父进程内存的数据比较大的时候，使用fork要特别谨慎。
不同点：
    ppid是不一致的，信号位图被清除的

示例:通过python提供的os.fork进程
```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# author zhangchao

import os
import time

res=os.fork()
print(123)
print('res == %d'%res)
if res == 0:
    print('我是子进程,我的pid是:%d,我的父进程id是:%d'%(os.getpid(),os.getppid()))
else:
    print('我是父进程,我的pid是:%d'%os.getpid())
```
运行结果告诉我们执行两次