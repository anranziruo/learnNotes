### dashboard部署

#### 获取dashborad的yml
```
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```
#### 下载相关的镜像
```
docker pull registry.cn-hangzhou.aliyuncs.com/ccgg/metrics-scraper:v1.0.4
docker pull registry.cn-hangzhou.aliyuncs.com/ccgg/dashboard:v2.0.0
```
#### 替换配置文件
```
sed -i 's#kubernetesui/dashboard:v2.0.0#registry.cn-hangzhou.aliyuncs.com/ccgg/dashboard:v2.0.0#g' recommended.yaml 
sed -i 's#kubernetesui/metrics-scraper:v1.0.4#registry.cn-hangzhou.aliyuncs.com/ccgg/metrics-scraper:v1.0.4#g' recommended.yaml 
```
#### 修改访问方式
```
spec:
  type: NodePort //添加node port
  ports:
    - port: 443
      targetPort: 8443
      nodePort: 30001 //对外访问端口
  selector:
    k8s-app: kubernetes-dashboard
```
#### 创建dashboard
```
kubectl apply -f recommended.yaml
```

#### 访问dashboard
```
1.https://k8s-master:30001/
2.kubectl create serviceaccount dashboard-admin -n kube-system
kubectl create clusterrolebinding dashboard-admin --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin
kubectl describe secrets -n kube-system $(kubectl -n kube-system get secret | awk '/dashboard-admin/{print $1}')
3.输入token到登陆页
```
#### 注意事项
```
出现Client sent an HTTP request to an HTTPS server. 这个报错的话，使用https
当mac下出现https的访问不是私密链接，直接在键盘输入thisisunsafe
```