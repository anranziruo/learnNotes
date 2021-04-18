### node_exproter
#### 1.然后运行prometus
* 1.1.使用docker进行安装docker run -p 9090:9090 prom/prometheus(前提是要安装docker)
* 1.2:新建配置文件，使用新配置文件运行
sudo docker run -p 9090:9090 -v /home/xiaoshanshan/prometheus.yml:/etc/prometheus/prometheus.yml   prom/prometheus
#### 2.首先去官网下载https://prometheus.io/download/最新的node_exporter，然后在被监控的机器上进行node_exporter的安装。
这一块简单的方式，直接解压，然后以root用户运行sudo ./node_exporter(ubuntu系统),
将node_exporter的运行地址和端口放到prometheus.yml 中，
# Load and evaluate rules in this file every 'evaluation_interval' seconds.
rule_files:
  # - "first.rules"
  # - "second.rules"
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
 *  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
 *  - job_name: 'node'
    static_configs:
      - targets: ['192.168.1.107:9100']
 *  - job_name: 'memcached'
    static_configs:
      - targets: ['192.168.1.107:9150']
* - job_name: redis_exporter
    static_configs:
      - targets: ['192.168.1.107:9121']
*   - job_name: mongodb
    static_configs:
      - targets: ['192.168.1.107:9216']
检验打开方式http://192.168.1.107:9090看是否打开
#### 3.redis的监控
https://github.com/oliver006/redis_exporter/releases下载最新的redis_node_exporter,
然后解压运行,
同理将redis的配置放到prometheus.yml中
#### 4.memcached
memcached官方提供了对应的node_exporter,这一直接下载运行，然后进行配置化
#### 5.对于mongodb的监控稍微麻烦点
https://github.com/percona/mongodb_exporter
使用go get进行安装 go get  -v github.com/percona/mongodb_exporter
下载完成以后，然后进入目录结构，进行sudo make build ,这一块要特别注意的是需要安装sudo apt-get install glide这个插件才可以
，注意的点是build的时候，需要翻墙
安装完以后，直接运行，然后更新配置文件
#### 6.对于mysql的监控，这一块去官方下载mysqld_exporter
下载以后
首先在监控的机器上，设置用于监控的mysql的账号和密码，然后运行mysqld_exporter，运行以后，在grafana上设置dashboard,这个时候可以推荐使用模板(7362)
#### 5.grafana的使用
1.首先监控promethus的节点。这一块选用模板的时候，需要注意是否和自己的库关联。这一块特别要注意的是mongodb的库要使用模板(5270),其余的可以选用的不用太过小心。
