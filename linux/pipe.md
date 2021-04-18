# 进程之管道学习

## 管道

### 管道是用于两个有关联的进程进行父子进程之间的数据交流。管道是半双工的，单向的通信方式，父子进程和其祖先进程的交流

python的multiprocessing库的Pipe可以产生两个通道

python的示例代码如下：
```
# -*- coding:utf-8 -*-

from multiprocessing import Pipe,Process
import time
def send(wr):
    for i in range(0,１0):
        print('proc1 rec:',wr.recv())

def read(re):
    for i in range(0,10):
        re.send(i)

if __name__ == '__main__':
    wr,re = Pipe(duplex=False)
    p1 = Process(target=send, args=(wr,))
    p2 = Process(target=read, args=(re,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
    p1.terminate()
    p2.terminate()
    time.sleep(1000)
```

## golang代码示例
这个示例，标准的输出管道的应用
```
package main

import (
	"bufio"
	"fmt"
	"io"
	"os/exec"
)

func main() {
	cmd := exec.Command("ls", "-all")

	stdout, err := cmd.StdoutPipe()
	if err != nil {
		fmt.Printf("get output pipe error:%s", err.Error())
		return
	}

	if err := cmd.Start(); err != nil {
		fmt.Println("error command:%s", err.Error())
		return
	}

	outputBuf0 := bufio.NewReader(stdout)
	if err != nil {
		fmt.Println("read line error")
		return
	}
	for {
		outputo, err := outputBuf0.ReadString('\n')
		//读取完毕
		if err == io.EOF {
			break
		}
		//读取失败
		if err != nil {
			fmt.Printf("read file failed, err:%v", err)
			break
		}
		fmt.Printf("%s", outputo)
	}
}
```
两个管道进行数据的传输，这一块以linux的grep指令为例