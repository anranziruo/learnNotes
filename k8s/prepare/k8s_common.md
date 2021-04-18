#### 配置k8s的源
```
cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

#### docker配置阿里源
```
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum update -y

```

```
yum list docker-ce --showduplicates | sort -r  //查看docker的版本

yum install docker-ce-3:18.09.9-3.el7.x86_64 

systemctl enable docker
systemctl start docker

vim /etc/docker/daemon.json
{"registry-mirrors": ["https://wv1h618x.mirror.aliyuncs.com"],"exec-opts": ["native.cgroupdriver=systemd"],"log-driver": "json-file","log-opts": {"max-size": "100m"}}
```

##### 预先下载好镜像
```
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.16.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.16.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.16.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.16.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.3.15-0
docker pull mirrorgooglecontainers/pause:3.1
docker pull coredns/coredns:1.6.2

##### 修改镜像的tag ####
docker tag docker.io/registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.16.0 k8s.gcr.io/kube-apiserver:v1.16.0
docker tag docker.io/registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.16.0 k8s.gcr.io/kube-controller-manager:v1.16.0
docker tag docker.io/registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.16.0 k8s.gcr.io/kube-scheduler:v1.16.0
docker tag docker.io/registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.16.0 k8s.gcr.io/kube-proxy:v1.16.0
docker tag docker.io/registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.3.15-0 k8s.gcr.io/etcd:3.3.15-0
docker tag docker.io/mirrorgooglecontainers/pause:3.1 k8s.gcr.io/pause:3.1
docker tag docker.io/coredns/coredns:1.6.2 k8s.gcr.io/coredns:1.6.2
```

#### 安装k8s
```
yum -y  install  kubeadm-1.16.9-0.x86_64 kubectl-1.16.9-0.x86_64 kubelet-1.16.9-0.x86_64

### 如果出现公钥的问题 ####
yum install kubeadm-1.16.9-0.x86_64 kubelet-1.16.9-0.x86_64 kubectl-1.16.9-0.x86_64 --nogpgcheck
#### 设置k8s重启 ####
systemctl enable kubelet && systemctl start kubelet
```