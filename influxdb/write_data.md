### 数据写入
#### 要求如下
一行Line Protocol表示InfluxDB中的一个数据点。它向InfluxDB通知点的measurement，tag set，field set和timestamp。以下代码块显示了行协议的示例，并将其分解为其各个组件：
注意事项：
* 1.measurement和tag set是用不带空格的逗号分开的
* 2.使用空格分隔field set和可选的时间戳。如果你包含时间戳，则行协议中需要空格
* 3.数据点的时间戳记以纳秒精度Unix时间。行协议中的时间戳是可选的。 如果没有为数据点指定时间戳，InfluxDB会使用服务器的本地纳秒时间戳。
* 4.时间戳不要双或单引号
* 5.field value不要单引号,即时是字符串类型,字符串类型的话使用双引号
* 6.measurement名称，tag keys，tag value和field key不用单双引号
* 7.field value是整数，浮点数或是布尔型时，不要使用双引号
#### 重复数据
一个点由measurement名称，tag set和timestamp唯一标识。如果您提交具有相同measurement，tag set和timestamp，但具有不同field set的行协议，则field set将变为旧field set与新field set的合并，并且如果有任何冲突以新field set为准