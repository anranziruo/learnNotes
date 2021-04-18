### 基础指令

#### 数据库导入指令:
influx -import -path=NOAA_data.txt -precision=s -database=NOAA_water_database
```
--import-path :导入的路径的文件
-database:数据库名称
-precision:数据的时间精度
```
##### 展示所有的measurements
SHOW measurements
##### 删除对应的MEASUREMENT
DROP MEASUREMENT <measurement_name>
##### 展示measurements的series
show series from h2o_feet;
####CQ的命令行###########
##### 展示所有的CQ:
SHOW CONTINUOUS QUERIES
##### 删除CQ：
DROP CONTINUOUS QUERY <cq_name> ON <database_name>
##### 示例如下：
DROP CONTINUOUS QUERY "idle_hands" ON "telegraf"`
修正CQ是不可以的