### 基础指令

#### 获取所有的容器(ps)
```
docker ps --all 获取所有容器
docker ps -s 显示容器的使用存储
```
#### 获取所有的镜像(images|rmi|pull)
```
docker images --all 所有的镜像
docker images 标签名 搜索镜像
docker rmi 镜像id 移除镜像
docker pull 获取镜像
```
#### 在容器内运行指令(exec)
```
docker exec -it 容器id 路径 //进入容器内部
docker exec -it e1 /bin/bash
docker exec -it e1 /bin/sh
docker exec -it --user root 容器id sh //以root用户进入容器内部
```
#### 容器的启动和停止(start|stop|restart|kill|rm)
```
docker start 容器id 启动容器
docker stop 容器id 停止容器
docker restart 容器id 重新启动容器
docker kill 容器id kill容器
docker rm 容器id 移除容器
```
#### 容器日志(logs)
```
docker logs 容器id
-f --follow=false 跟随日志输出
-t 显示时间戳
```
#### 运行容器
```
docker run -it -name 容器名称 -v 挂载目录 -p 端口 -d 后台运行 镜像名称
```
#### 设置容器开启
```
docker update --restart=always 容器id
```