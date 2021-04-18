### k8s部署nginx

#### 创建pod的yaml文件
vim nginx-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1 //副本数量
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
kubectl apply -f nginx-deployment.yaml //创建pod
成功以后,运行指令 kubectl get pods //查看是否用nginx的pod

#### 创建service的yaml
vim nginx-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    app: nginx
spec:
  ports:
  - port: 88
    targetPort: 80
  selector:
    app: nginx
  type: NodePort
```
kubectl create -f nginx-service.yaml //创建service
kubectl get service/nginx-service //查看已经创建成功
//已经安装成功
```
NAME            TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE   SELECTOR
nginx-service   NodePort   10.105.90.51   <none>        88:30103/TCP   21m   app=nginx
```
PORT(s)就是访问的端口

#### 检查是否安装成功
打开浏览器,输入http://k8s-master:30103/
出现欢迎页就表示安装成功


