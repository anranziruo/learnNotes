### k8s准备事项

#### 升级内核
```
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
//安装完成后检查 /boot/grub2/grub.cfg 中对应内核 menuentry 中是否包含 initrd16 配置，如果没有，再安装一次！
yum --enablerepo=elrepo-kernel install -y kernel-lt
//设置新内核启动###
grub2-set-default 'CentOS Linux (5.4.91-1.el7.elrepo.x86_64) 7 (Core)'
grub2-set-default 'CentOS Linux (5.4.246-1.el7.elrepo.x86_64) 7 (Core)'
```

#### 安装相关依赖
```
yum install -y conntrack ntpdate ntp ipset jq curl sysstat libseccomp wgetvimnet-tools git
yum -y install lrzsz
yum -y install yum-utils
```

#### 关闭防火墙
```
systemctl stop firewalld
systemctl disable firewalld
```

#### 关闭selinux
```
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
### 查看selinux的状态 ####
/usr/sbin/sestatus -v
```

#### 关闭swap
```
swapoff -a && sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

#### 设置主机名和设置hosts
```
hostnamectl set-hostname k8s-master
### 配置host vim /etc/hosts ####
172.16.6.132 k8s-master
```
#### 设置时区和关闭无用服务
```
# 设置系统时区为中国/上海
timedatectl set-timezone Asia/Shanghai

# 将当前的 UTC 时间写入硬件时钟
timedatectl set-local-rtc 0

# 重启依赖于系统时间的服务
systemctl restart rsyslog
systemctl restart crond

### 关闭无关的服务 ###
systemctl stop postfix && systemctl disable postfix 
```

#### 桥接的IPV4流量传递到iptables的链
```
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward                 = 1
EOF
sysctl --system
```
#### kube-proxy开启ipvs的前置条件
```
yum -y install ipvsadm
modprobe br_netfilter
echo 1 > /proc/sys/net/ipv4/ip_forward

cat > /etc/sysconfig/modules/ipvs.modules <<EOF
#!/bin/bash
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack
EOF
### 修正权限
chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules && lsmod | grep -e ip_vs -e nf_conntrack
```
