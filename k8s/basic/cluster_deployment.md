### deployment

#### 概念
```
1.Deployment 指不具有唯一标识的一组多个相同的 Pod。Deployment 运行应用的多个副本，并自动替换任何失败或无响应的实例。通过这种方式，Deployment 可帮助确保应用的一个或多个实例可用于处理用户请求。Deployment 由 Kubernetes Deployment 控制器管理。
2.Deployment 使用 Pod 模板
3.Deployment 的 Pod 模板更改时，系统会自动创建新 Pod，每次创建一个
```
#### 无状态应用
```
无状态应用是不将数据或应用状态存储到集群或永久性存储空间的应用。
Kubernetes 使用 Deployment 控制器将无状态应用部署为统一且非唯一的 Pod。Deployment 管理应用的所需状态：应运行应用的 Pod 数,应运行的容器映像版本、Pod 应标记的内容
```

#### 使用模式
```
Deployment 非常适合使用在多个副本上挂载的 ReadOnlyMany 或 ReadWriteMany 卷的无状态应用
```

#### 创建deployment

##### 1.创建yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx //deployment的名称
spec:
  replicas: 3 //副本个数
  selector:
    matchLabels:
      app: nginx
//匹配标签  matchLabels 字段是 {key,value} 偶对的映射。在 matchLabels 映射中的单个 {key,value} 映射等效于 matchExpressions 中的一个元素，即其 key 字段是 “key”，operator 为 “In”，value 数组仅包含 “value”。在 matchLabels 和 matchExpressions 中给出的所有条件都必须满足才能匹配。
  template:
    metadata:
      labels:
        app: nginx //标签
    spec:
      containers:
      - name: nginx //容器名称
        image: nginx:1.7.9  //镜像名称
        ports:
        - containerPort: 80 //对外端口
```

##### 命令行创建deployment
```
kubectl apply -f nginx-deployment.yaml 
说明:
你可以设置 --record 标志将所执行的命令写入资源注解 kubernetes.io/change-cause 中。 这对于以后的检查是有用的。例如，要查看针对每个 Deployment 修订版本所执行过的命令
```
###### deployment验证
```
kubectl describe -f nginx-deployment.yaml//检查deployment
kubectl get deployments //获取deployments的状态信息
kubectl get rs //查看副本信息
kubectl get pods --show-labels //查看pods生成的标签
```

##### 更新deployment
```
kubectl apply -f recommended.yaml --record //记录更改记录
```
##### 回滚deployment
```
kubectl rollout undo deployment deployment名称 --to-revision=2 //回滚特定版本
kubectl rollout history deployment deployment名称 //检查版本记录
kubectl rollout status deployment deployment名称
```
##### 缩放deployment
```
kubectl scale deployment nginx-deployment --replicas=5 //调整副本的数量
//自动缩放
kubectl autoscale deployment deployment名称 --min=2 --max=15 --cpu-percent=80
```





