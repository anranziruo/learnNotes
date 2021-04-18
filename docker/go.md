### docker部署go项目

#### 项目目录示例如下
```
docker-compose.yml
go_demo
    Dockerfile
    main.go
    templates
        index.tmpl
```

#### docker-file详解
```
FROM golang:alpine as build_dev  //获取go的apline的源
ENV GO111MODULE=on
ENV GOPROXY=https://goproxy.cn  //设置go的环境变量

WORKDIR /builder //设置容器的工作目录

COPY ./go_demo /builder //复制go项目到容器内部

RUN cd /builder && go mod init app && go mod tidy && go build -o app . //go的mod 初始化,安装依赖,编译到固定文件

//创建一个新的镜像
FROM alpine
COPY ./go_demo/templates /templates //从磁盘拷贝代码到新的镜像内

COPY --from=build_dev /builder/app //拷贝编译好的程序
EXPOSE 8093 //指定端口

ENTRYPOINT ["/app"] //启动程序
```

#### docker-composer详解 示例如下
```
version: "3.8"
services:
    db_mysql:
      image: "mysql:8.0.19"
      ports:
      - "13306:3306"
      restart: always //随着docker开机重启
      environment:
        MYSQL_ROOT_PASSWORD: "root"
        MYSQL_DATABASE: "gin_demo" //制定自定义创建数据库
      volumes:
        - /home/cr7/dumps/Dump20201202/gin_demo_demo3.sql:/docker-entrypoint-initdb.d/gin_demo_demo3.sql
        - /home/cr7/dumps/Dump20201201/flask_demo_demo1.sql:/docker-entrypoint-initdb.d/flask_demo_demo1.sql //制定初始化sql文件
        - /home/cr7/install/mysql:/var/lib/mysql
    db_redis:
      image: "redis:5.0.10"
      ports:
      - "6379:6379"
      restart: always
      command: redis-server --requirepass 123456 --appendonly yes //制定密码
      volumes:
        - /home/cr7/install/redis:/data //rdb文件目录
    go_demo:
      build:
        context: .
        dockerfile: ./go_demo/Dockerfile
      ports: 
        - "8093:8093"
      restart: always
      depends_on:
        - db_mysql
        - db_redis

```

#### 使用docker-composer
```
docker-compose build 构建镜像
docker-compose up -d 后台启动
docker-compose down 关闭所有容器
```
