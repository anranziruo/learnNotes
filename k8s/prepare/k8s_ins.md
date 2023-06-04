### 安装k8s master节点

#### k8s配置文件
```
###固定在特定目录
kubeadm config print init-defaults > kubeadm-config.yaml

#### 修改配置
advertiseAddress: 172.16.6.132//本机ip
podSubnet: 10.244.0.0/16 //不加这个可能会使得Node间Cluster IP不通。

#### 增加配置
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
featureGates:
  SupportIPVSProxyMode: true
mode: ipvs
```

#### 启动k8s master
kubeadm init --config=kubeadm-config.yaml --upload-certs  | tee kubeadm-init.log 启动成功以后
```
### 运行kubectl get node出现报错:The connection to the server localhost:8080 was refused - did you specify the right host or port?
#### 原因是因为kubectl命令需要使用kubernetes-admin来运行，解决方法如下，将主节点中的【/etc/kubernetes/admin.conf】文件拷贝到从节点相同目录下，然后配置环境变量
echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> ~/.bash_profile
source ~/.bash_profile
```
#### 配置flannel
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
1.https://github.com/coreos/flannel/releases/download/v0.13.0/flanneld-v0.13.0-amd64.docker 下载docker镜像
2.docker load < flanneld-v0.13.0-amd64.docker 导入镜像
3.wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
//下载有问题的话,记得增加hosts
199.232.68.133 raw.githubusercontent.com
4.kubectl apply -f kube-flannel.yml
```

#### 查看状态
```
kubectl get pod -A -n kube-system
所有状态都是running就是启动成功
kubectl get nodes
状态是ready就是启动成功
```
