### 内容
```
1.本地开启go mod

版本要求:go版本>=1.11

1.11版本需要配置环境变量:export GO111MODULE=on(mac或者linux系统)

1.13以上版本默认开启

### go mod默认开启会影响老的项目,在go build之前请

export GO111MODULE=off

注意:1.11以下版本不支持go mod,需要升级版本

另外需要增加环境变量用于拉取依赖:export GOPROXY=https://goproxy.io

2.ide的配置(golang和idea)

代码编写的时候需要注意以下的问题(引用包的路径根据你的go mode init 的名字决定)
3.go mod的使用

3.1:cd 自己的仓库目录

3.2:go mod init 名称 建议和自己的仓库名保持一致

*添加依赖 go get 包名 

*更新依赖 go get 包名

*添加依赖到制定版本 go get 包名@版本号

3.3 拉取依赖

go mod tidy -v

####参考资料####

官方文档:https://blog.golang.org/using-go-modules

公司的文档：

Golang 包管理规范

go-model_69742876.html2
```