### 动态加载module
#### 
比如说我想引入一个文件夹下的所有".py"文件结尾的文件，但是不需要引入_init_.py。
首先，我们通过python的字符串处理过程获取到一个相关的字符串数组。
示例代码如下:
```
import os
n=[]
dir1=os.listdir('E:\\webtest\\test_case')
for a in dir1:
    m=a.find("start")
    if m!=-1:
        test=a.split('.')[1]
        if test=='py':
            test1=a.split('.')[0]
            n.append(test1)
```
这个时候n输出是一个字符串形式的列表:
n=['start_01', 'start_02', 'start_03', 'start_04']
这个时候，如果想动态导入的话，使用_import_()方法，关于这个方法的使用，参考官方指导文档即可
```
for i in range(0,m):
    a="test_case"+"."+n[i]
    load=__import__(a)
```
