### 安装流程

#### 下载镜像
```
1.http://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/ 进入这个链接
2.下载CentOS-7-x86_64-DVD-2009.iso文件
```
#### vmware_fusion创建镜像
```
1.打开vmware_fusion创建镜像,直接一直下一步
2.自定义虚拟机配置到新建的目录
3.打开虚拟机,选择安装install_centos
4.选择语言为中文
5.选择安装位置->点击完成
6.点击开始安装
7.设置root用户密码
```
#### 虚拟机初始设置
```
1.添加设备->增加网络设备
2.设置虚拟机内存为2G,选择2个处理器内核
3.yum install wget vim net-tools.x86_64
```
#### 设置yum的源为阿里的源
```
1.mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
2.wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
3.yum clean all
4.yum makecache
```
