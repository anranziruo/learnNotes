### 内容

#### 基于ssh-agent

```
ssh-agent sh -c 'ssh-add ~/.ssh/id_rsa; git ls-remote -h git@gitee.com:zhangchao_zhuifeng/learn-notes.git' 

// ssh-add 添加的私钥
// git 命令 ssh的git地址
```

#### 基于GIT_SSH_COMMAND(对于git版本有要求)

```
GIT_SSH_COMMAND='ssh -i ~/.ssh/id_rsa -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no' \
  git ls-remote -h git@gitee.com:zhangchao_zhuifeng/learn-notes.git
  //ssh -i 指向私钥
```
#### 通过GIT_SSH
```
$ echo 'ssh -i ~/.ssh/id_rsa -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no $*' > id_rsa_cli //临时文件
$ chmod +x id_rsa_cli //增加执行权限
$ GIT_TRACE=1 GIT_SSH='./id_rsa_cli' git ls-remote -h git@gitee.com:zhangchao_zhuifeng/learn-notes.git //执行git指令
```


