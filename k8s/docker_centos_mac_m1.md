### 执行指令
```
cd /etc/yum.repos.d/
mkdir yum-back
mv CentOS-* yum-back/
```
### docker指令
docker run -it --platform linux/arm64/v8 --name doris-centos -p 8030:8030 -p 9030:9030 --privileged -d centos:7.9.2009
### 阿里云centos源
```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the 
# remarked out baseurl= line instead.
#
#
[base]
name=CentOS-$releasever - Base
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/os/$basearch/
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
#released updates 
[updates]
name=CentOS-$releasever - Updates
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
enabled=1
#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=https://mirrors.aliyun.com/centos-altarch/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7
[root@node-01 yum.repos.d]# cat epel.repo 
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch
baseurl=https://mirrors.aliyun.com/epel/7/$basearch
#metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
failovermethod=priority
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
[epel-debuginfo]
name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch/debug
baseurl=https://mirrors.aliyun.com/epel/7/$basearch/debug
#metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1
[epel-source]
name=Extra Packages for Enterprise Linux 7 - $basearch - Source
#baseurl=http://download.fedoraproject.org/pub/epel/7/SRPMS
baseurl=https://mirrors.aliyun.com/epel/7/SRPMS
#metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
gpgcheck=1
```
### 替换密匙
```
curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7  https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

curl -o /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7 https://archive.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7
```

### 生成缓存
```
yum makecache

rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7

rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

### yum安装注意事项
```
yum 安装加这个指令--nogpgcheck
```

### doris-composer.YML
```
version: '3'
services:
  docker-fe:
    image: "apache/doris:1.2.2-fe-arm"
    platform: linux/arm64
    container_name: "doris-fe"
    hostname: "fe"
    environment:
      - FE_SERVERS=fe1:172.18.0.2:9001
      - FE_ID=1
    ports:
      - 8030:8030
      - 9030:9030
    volumes:
      - ./data/fe/doris-meta:/opt/apache-doris/fe/doris-meta
      - ./data/fe/conf:/opt/apache-doris/fe/conf
      - ./data/fe/log:/opt/apache-doris/fe/log
    networks:
      doris_net:
        ipv4_address: 172.18.0.2
  docker-be:
    image: "apache/doris:1.2.2-be-arm"
    platform: linux/arm64
    container_name: "doris-be"
    hostname: "be"
    depends_on:
      - docker-fe
    environment:
      - FE_SERVERS=fe1:172.18.0.2:9001
      - BE_ADDR=172.18.0.3:9050
    ports:
      - 8040:8040
    volumes:
      - ./data/be/storage:/opt/apache-doris/be/storage
      - ./data/be/conf:/opt/apache-doris/be/conf
      - ./data/be/script:/docker-entrypoint-initdb.d
      - ./data/be/log:/opt/apache-doris/be/log
    networks:
      doris_net:
        ipv4_address: 172.18.0.3
networks:
  doris_net:
    ipam:
      config:
        - subnet: 172.18.0.0/16

```
