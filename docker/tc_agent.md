#### 获取agent镜像
```
docker pull jetbrains/teamcity-agent:2020.1.5 //安装2020.1.5版本
docker pull jetbrains/teamcity-agent:latest  //安装最新的
```
#### 启动agent
```
sudo docker run -it --name teamcity-agent2 \
    -e SERVER_URL=http://172.17.0.1:8117/ \ //tc_server的url
    -e AGENT_NAME=agent_222 \ //agent_name
    --link 9df837fe1352 \ //docker的镜像id 
    -v /home/cr7/app/install/teamcityagents/agent1:/opt/teamcity_agent/conf \
    -d jetbrains/teamcity-agent:latest
```
