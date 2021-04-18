# 内容
## 文件编辑
### grep
使用规则如下:
```
grep -n 显示行号
-vn 反向选择
grep -n  [0-9A-Za-z]//匹配简单的正则表达式,只取字母和数字
grep -n  [^0-9A-Za-z]//不匹配数字字母
grep -n ^h //从行首开始匹配
grep -n 0$//匹配行尾
grep -n '^[^a-z]' //匹配行首不是字母
grep -n "\."  //匹配特殊的字符用‘\’
.*匹配任意的字符0个到无穷个
```
### sed
使用规则如下:
sed -i 's/要被替换的字符串/新的字符串/g'  源文件
备注:这个指令在mac需要注意
sed -i "bak" 's/要被替换的字符串/新的字符串/g'  源文件//是否要进行备份
### egrep
相关正则表达式
+ //重复一个或者一个以上的前一个re字符
egrep -n 'go+d' regular_express.txt
?//零个或者一个的前一个re字符
egrep -n 'go?d' regular_express.txt
用or的表达式匹配字符串
egrep -n 'gd|good' regular_express.txt
群组的方式匹配
egrep -n 'g(la|oo)d' regular_express.txt
###　basename
示例如下:
```
basename /etc/sysconfig/network
```
### dirname
```
dirname /etc/sysconfig/network
```
## 文件管理
### tail
tail -n 行数 文件名 查看文件的从最后一行到目标行数
tail -f  文件名 滚动查看文件
### cat
cat  文件名 //从第一行显示全部文件内容
### tac
tac从最后一行显示文件内容
### nl
按照行号显示内容
### less
按照翻页显示内容，支持向前和向后翻页
### more
可以进行翻页,只支持向后翻页
### head
head从头开始显示
###　od
显示的是非纯文本的文档
```
示例如下:
od -t c /usr/bin/passwd //按照ascii显示
```
## 文本移动和复制
### mv
mv 文件名 文件名 作用:将源文件名改为目标文件名
mv 文件名 目录名 作用:将文件移动到目标目录
mv 目录名 目录名 作用:目标目录已存在，将源目录移动到目标目录；目标目录不存在则改名
### cp
拷贝一个文件下的所有文件
sudo cp /a/* /b
拷贝a目录下的所有文件到b目录
cp -r 源目录 目标目录名
cp -r 源目录/* 目标目录名 拷贝所有的文件目录到对应的文件
### awk
使用示例
```
kill -9 `ps -ef | grep "/app/php7/bin/php /app/www/service.auth/rpcserver/server.php" | awk '{print $2}'`
```
## 系统管理
### 查看进程的端口　netstat
```
netstat -ntlp   //查看当前所有tcp端口·
netstat -ntulp |grep 80   //查看所有80端口使用情况·
netstat -an | grep 3306   //查看所有3306端口使用情况·
```
### 查看端口号对应的进程
```
lsof -i:9999//端口号查看进程
```
