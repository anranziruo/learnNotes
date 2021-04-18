#### k8s安装node
参考这个配置安装k8s
* [k8s通用安装(master和node都适用)](https://github.com/zhangchao1/learnNotes/blob/master/k8s/k8s_common.md)

#### join到master节点
```
kubeadm join k8s-master:6443 --token abcdef.0123456789abcdef     --discovery-token-ca-cert-hash sha256:6868664e85f8716127a6c56c4733c23f016ea63605945c96769ea8d287bb8123 
k8s-master是master节点ip
```
#### 生成新的token
```
kubeadm token create --print-join-command //生成token
kubeadm join 172.16.6.132:6443 --token unb1m9.q3mj0bxw5f5jgd56     --discovery-token-ca-cert-hash sha256:6868664e85f8716127a6c56c4733c23f016ea63605945c96769ea8d287bb8123 //生成join指令
```