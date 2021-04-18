##### mysql 数据容器

##### docker-composer文件
```
version: "3.8"
services:
    db_mysql:
      image: "mysql:8.0.19"
      ports:
      - "13306:3306"
      restart: always //docker重启的时候自动重启
      environment:
        MYSQL_ROOT_PASSWORD: "root"
        MYSQL_DATABASE: "gin_demo" //初始化的数据库
      volumes:
        - /home/cr7/dumps/Dump20201202/gin_demo_demo3.sql:/docker-entrypoint-initdb.d/gin_demo_demo3.sql
        - /home/cr7/dumps/Dump20201201/flask_demo_demo1.sql:/docker-entrypoint-initdb.d/flask_demo_demo1.sql   //初始化建表的sql
        - /home/cr7/install/mysql:/var/lib/mysql //sql的容器
    go_demo:
      build:
        context: .
        dockerfile: ./go_demo/Dockerfile
      ports: 
        - "8093:8093"
      depends_on: //依赖项
        - db_mysql
```