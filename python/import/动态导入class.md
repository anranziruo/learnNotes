#### 动态导入class
我们test_case文件夹地下，我们需要动态获取start_01到start_04文件夹底下，对应的class，这一块为了调用方便，我们文件名和类名保持一致。
思想如下：首先我们获取类的前提，是先获取module，刚才只是动态导入module，并没有获取导入module.
获取的源码如下：
```
test2=[]
for i in range(0,m):
    a="test_case"+"."+n[i]
    load=__import__(a)
    a=getattr(load,n[i])#获取module
    b=getattr(a,n[i])#获取class
    test2.append(b)
```
这一段源码就完成动态获取类的过程。
关于getattr方法的使用，具体的要参考官网文档。
