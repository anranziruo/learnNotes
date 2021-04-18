#### 安装教程
```
1.wget http://repo.mysql.com/mysql57-community-release-el7-10.noarch.rpm(下载rpm)
2.rpm -Uvh mysql57-community-release-el7-10.noarch.rpm
3.yum install -y mysql-community-server
4.systemctl start mysqld.service//启动
5.设置开机自启
systemctl enable mysqld
systemctl daemon-reload
6.获取临时密码
grep 'temporary password' /var/log/mysqld.log //localhost的内容
7.设置root用户密码
(1):登录mysql -uroot -p临时密码
(2):set global validate_password_policy=0;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';
8.配置远程登录
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;
9.FLUSH PRIVILEGES;(刷新)
```