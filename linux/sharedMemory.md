## 共享内存


## 共享内存是进程间通信(Inter Process Communication)的最快方式。linux共享内存有两种方式：
第一种：mmap方式，适用场景：父子进程之间，创建的内存非常大时
第二种：shmget方式，适用场景：同一台电脑上不同进程之间，创建的内存相对较小时