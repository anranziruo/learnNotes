### 部署安卓环境(基于tc的agent)

```
From jetbrains/teamcity-agent:2020.1.4
COPY . /teamcity/android_agent
WORKDIR /teamcity/android_agent
USER root //以root用户启动
RUN apt-get install software-properties-common; \
    echo -e "\n" | add-apt-repository ppa:ts.sch.gr/ppa; \ 
    apt-get update; \ 
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections; \ //默认输入许可
    apt-get install oracle-java8-installer -y; \ //安装java8
    mkdir flutter_install; \
    wget -P flutter_install https://storage.googleapis.com/flutter_infra/releases/stable/linux/flutter_linux_1.22.3-stable.tar.xz; \
    tar -xvf /teamcity/android_agent/flutter_install/flutter_linux_1.22.3-stable.tar.xz; \ //安装flutter
    mkdir android_sdk; \
    wget -P android_sdk https://dl.google.com/android/repository/platform-tools-latest-linux.zip; \
    unzip /teamcity/android_agent/android_sdk/platform-tools-latest-linux.zip //安装android sdk

```