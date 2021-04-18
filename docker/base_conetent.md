#### 基础内容

##### 系统组成
```
Docker守护进程:它负责容器的创建、运行和监控，还负责镜像的构建和储存 ，容器和镜像都在图的右边。 Docker 守护进程通过 docker daemon 命令启动，一
般会交由主机的操作系统负责执行.
Docker寄存服务:负责储存和发布镜像
docker客户端:通过 HTTP 与 Docker 守护进程通信。默认使用 Unix 域套接字（Unix domain socket）实现，但为了支持远程客户端也可以使用 TCP socket
```

#### 底层技术
```
Docker 守护进程通过一个“执行驱动程序”（execution driver）来创建容器。默认情况下，
它是 Docker 项目自行开发的 runc 驱动程序，但仍支持旧有的 LXC。 runc 与下面提到的内
核功能密不可分。
cgroups，负责管理容器使用的资源（例如 CPU 和内存的使用）。它还负责冻结和解冻
容器这两个 docker pause 命令所需的功能。
Namespaces（命名空间），负责容器之间的隔离；它确保系统的其他部分与容器的文件
系统、主机名、用户、网络和进程都是分开的。
```

##### 容器互联
```
通过--link参数来制定容器id
```
#### 数据卷
```
1.docker run -v 目录 制定容器内数据卷
2.docker file指定 VOLUME 目录 制定容器内数据卷
3.docker run -v HOST_DIR:CONTAINER_DIR 挂载主机目录到容器目录
```
#### 数据容器
```
docker run -d --volumes-from dbdata --name db1 postgres //通过--volumes-from来指定数据卷
删除数据卷
1.运行docker rm -v 指令
2.docker run --rm选项
```

