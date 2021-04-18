#### 获取tc的server镜像
```
docker pull jetbrains/teamcity-server:2020.1.5 //安装2020.1.5版本
docker pull jetbrains/teamcity-server:latest  //安装最新的
```

#### tc_server部署
```
docker run -it --name teamcity-server-instance_latest \
    -e TEAMCITY_DATA_PATH=/home/teamcity/work \
    -v /home/cr7/app/dockwork/teamcity/20200114/teamcity/data:/home/teamcity/work \
    -p 8118:8111 \ //本地的端口:映射docker的端口
    -d jetbrains/teamcity-server:latest 
```