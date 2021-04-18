### proｍethus作用

#### 1.基本模块构成
Prometheus 生态圈中包含了多个组件，其中许多组件是可选的：
* Prometheus Server: 用于收集和存储时间序列数据。
* Client Library: 客户端库，为需要监控的服务生成相应的 metrics 并暴露给 Prometheus server。当 Prometheus server 来 pull 时，直接返回实时状态的 metrics。
* Push Gateway: 主要用于短期的 jobs。由于这类 jobs 存在时间较短，可能在 Prometheus 来 pull 之前就消失了。为此，这次 jobs 可以直接向 * * * * Prometheus server 端推送它们的 metrics。这种方式主要用于服务层面的 metrics，对于机器层面的 metrices，需要使用 node exporter。
* Exporters: 用于暴露已有的第三方服务的 metrics 给 Prometheus。
* Alertmanager: 从 Prometheus server 端接收到 alerts 后，会进行去除重复数据，分组，并路由到对收的接受方式，发出报警。常见的接收方式有：电子邮件，pagerduty，OpsGenie, webhook 等。
* 使用Prometheus结合node_exporter和对应监控对象的exporter监控
#### 数据模型
* Prometheus 中存储的数据为时间序列，是由 metric 的名字和一系列的标签（键值对）唯一标识的，不同的标签则代表不同的时间序列。
* metric 名字：该名字应该具有语义，一般用于表示 metric 的功能，例如：http_requests_total, 表示 http 请求的总数。其中，metric 名字由 ASCII 字符，数字，下划线，以及冒号组成，且必须满足正则表达式 [a-zA-Z_:][a-zA-Z0-9_:]*。
标签：使同一个时间序列有了不同维度的识别。例如 http_requests_total{method="Get"} 表示所有 http 请求中的 Get 请求。当 method="post" 时，则为新的一个 metric。标签中的键由 ASCII 字符，数字，以及下划线组成，且必须满足正则表达式 [a-zA-Z_:][a-zA-Z0-9_:]*。
样本：实际的时间序列，每个序列包括一个 float64 的值和一个毫秒级的时间戳。
格式：<metric name>{<label name>=<label value>, …}，例如：http_requests_total{method="POST",endpoint="/api/tracks"}。
* 3.Prometheus 客户端库主要提供四种主要的 metric 类型：
Counterinstance 和 jobs
instance: 一个单独 scrape 的目标， 一般对应于一个进程。
jobs: 一组同种类型的 instances（主要用于保证可扩展性和可靠性），例如：
清单 1. job 和 instance 的关系
![示例图](https://github.com/zhangchao1/learnNotes/blob/master/assets/promethus/job.png)
* 当 scrape 目标时，Prometheus 会自动给这个 scrape 的时间序列附加一些标签以便更好的分别
一种累加的 metric，典型的应用如：请求的个数，结束的任务数， 出现的错误数等等。
例如，查询 http_requests_total{method="get", job="Prometheus", handler="query"} 返回 8，10 秒后，再次查询，则返回 14。
Gauge
* 一种常规的 metric，典型的应用如：温度，运行的 goroutines 的个数。
可以任意加减。
例如：go_goroutines{instance="172.17.0.2", job="Prometheus"} 返回值 147，10 秒后返回 124。
Histogram
可以理解为柱状图，典型的应用如：请求持续时间，响应大小。
可以对观察结果采样，分组及统计。
* Summary
类似于 Histogram, 典型的应用如：请求持续时间，响应大小。
提供观测值的 count 和 sum 功能。
提供百分位的功能，即可以按百分比划分跟踪结果。

