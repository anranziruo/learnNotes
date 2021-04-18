#### 安装教程

#### 下载源
```
yum install epel-release //fedora的epel仓库
```
#### 安装redis
```
yum install redis
```
#### 设置开机重启
```
systemctl enable redis.service//设置开机自启
systemctl start redis //启动redis
```
