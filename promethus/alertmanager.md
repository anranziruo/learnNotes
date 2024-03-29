### alertmanager
#### Alertmanager特性：
* 1.分组特性
分组机制可以将详细的告警信息合并成一个通知。在某些情况下，比如由于系统宕机导致大量的告警被同时触发，在这种情况下分组机制可以将这些被触发的告警合并为一个告警通知，避免一次性接受大量的告警通知，而无法对问题进行快速定位。
* 2.抑制
是指当某一告警发出后，可以停止重复发送由此告警引发的其它告警的机制。
例如，当集群不可访问时触发了一次告警，通过配置Alertmanager可以忽略与该集群有关的其它所有告警。这样可以避免接收到大量与实际问题无关的告警通知。
* 3.静默
提供了一个简单的机制可以快速根据标签对告警进行静默处理。如果接收到的告警符合静默的配置，Alertmanager则不会发送告警通知。
静默设置需要在Alertmanager的Werb页面上进行设置。
#### 告警规则
在告警规则文件中，我们可以将一组相关的规则设置定义在一个group下。在每一个group中我们可以定义多个告警规则(rule)。一条告警规则主要由以下几部分组成：
* alert：告警规则的名称。
* expr：基于PromQL表达式告警触发条件，用于计算是否有时间序列满足该条件。
* for：评估等待时间，可选参数。用于表示只有当触发条件持续一段时间后才发送告警。在等待期间新产生告警的状态为pending。
* labels：自定义标签，允许用户指定要附加到告警上的一组附加标签。
* annotations：用于指定一组附加信息，比如用于描述告警详细信息的文字等。
* Prometheus根据global.evaluation_interval定义的周期计算PromQL表达式。如果PromQL表达式能够找到匹配的时间序列则会为每一条时间序列产生一个告警实例。