### replicaset

#### 概念
```
ReplicaSet 的目的是维护一组在任何时候都处于运行状态的 Pod 副本的稳定集合。 因此，它通常用来保证给定数量的、完全相同的 Pod 的可用性。
RepicaSet 是通过一组字段来定义的，包括一个用来识别可获得的 Pod 的集合的选择算符、一个用来标明应该维护的副本个数的数值、一个用来指定应该创建新 Pod 以满足副本个数条件时要使用的 Pod 模板等等。 每个 ReplicaSet 都通过根据需要创建和 删除 Pod 以使得副本个数达到期望值， 进而实现其存在价值。当 ReplicaSet 需要创建新的 Pod 时，会使用所提供的 Pod 模板。
ReplicaSet 通过 Pod 上的 metadata.ownerReferences 字段连接到附属 Pod，该字段给出当前对象的属主资源。 ReplicaSet 所获得的 Pod 都在其 ownerReferences 字段中包含了属主 ReplicaSet 的标识信息。正是通过这一连接，ReplicaSet 知道它所维护的 Pod 集合的状态， 并据此计划其操作行为。
ReplicaSet 使用其选择算符来辨识要获得的 Pod 集合。如果某个 Pod 没有 OwnerReference 或者其 OwnerReference 不是一个 控制器，且其匹配到 某 ReplicaSet 的选择算符，则该 Pod 立即被此 ReplicaSet 获得。
ReplicaSet 确保任何时间都有指定数量的 Pod 副本在运行。 然而，Deployment 是一个更高级的概念，它管理 ReplicaSet，并向 Pod 提供声明式的更新以及许多其他有用的功能。 因此，我们建议使用 Deployment 而不是直接使用 ReplicaSet
```

#### 查看部署的ReplicaSet的
```
kubectl get rs -o wide
```

#### 查看部署ReplicaSet的状态
```
kubectl describe rs/rs名称  //get rs的结果NAME
```
#### 删除ReplicaSet
```
kubectl delete 命令
只删除 ReplicaSet //--cascade=orphan
```
#### 缩放 RepliaSet
```
kubectl autoscale rs frontend --max=10 --min=3 --cpu-percent=50 //使用这个指令
```



