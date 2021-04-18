### 虚拟机配置静态ip

#### 查看掩码和网关(本机)
cd /Library/Preferences/VMware\ Fusion/vmnet8
```
1.cat nat.conf
2.ip=xxx.xxx.xxx.xxx //网关地址
3.netmask=xxx.xxx.xxx.xxx//子网掩码
```
#### 获取ip范围
cd /Library/Preferences/VMware\ Fusion/vmnet8
```
1.cat dhcpc.conf
2.range ip的范围 //获取可配置的ip
```

#### 配置静态ip
```
vim /etc/sysconfig/network-scripts/ifcfg-ens33
BOOTPROTO=static
IPADDR=192.168.160.128 //本机ip范围之内
NETMASK=255.255.255.0 //子网掩码
GATEWAY=192.168.160.2 //网关
DNS1=192.168.160.2 //配置和网关一样即可
```
