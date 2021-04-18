### statefulSets

#### 基础概念
```
StatefulSet 是用来管理有状态应用的工作负载 API 对象。
StatefulSet 用来管理某 Pod 集合的部署和扩缩， 并为这些 Pod 提供持久存储和持久标识符
StatefulSet 为它们的每个 Pod 维护了一个有粘性的 ID。这些 Pod 是基于相同的规约来创建的， 但是不能相互替换：无论怎么调度，每个 Pod 都有一个永久不变的 ID
```
#### 限制
```
给定 Pod 的存储必须由 PersistentVolume 驱动 基于所请求的 storage class 来提供，或者由管理员预先提供。
删除或者收缩 StatefulSet 并不会删除它关联的存储卷。 这样做是为了保证数据安全，它通常比自动清除 StatefulSet 所有相关的资源更有价值。
StatefulSet 当前需要无头服务 来负责 Pod 的网络标识。你需要负责创建此服务。
当删除 StatefulSets 时，StatefulSet 不提供任何终止 Pod 的保证。 为了实现 StatefulSet 中的 Pod 可以有序地且体面地终止，可以在删除之前将 StatefulSet 缩放为 0。
在默认 Pod 管理策略(OrderedReady) 时使用 滚动更新，可能进入需要人工干预 才能修复的损坏状态。
```
