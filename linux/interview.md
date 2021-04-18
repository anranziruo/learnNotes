## 内容

### find
```
find -iname 不区分大小写
find ~ -empty 查找空文件
find /usr -type f -size +10240k 查找超过10MB的文件
find /var \! -atime -90 90天未被访问的文件
find /home -mtime +120 120天以内修改的文件
```
### pwd
```
pwd 输出当前目录
```
### ls
```
ls -F 显示文件类型
ls -lh 显示文件大小
ls -all 显示所有文件
```
### mkdir
```
mkdir ~/temp 创建目录
mkdir -p dir1/dir2/dir3/dir4/ 创建一个不存在的目录
```
### df
```
df -h 显示磁盘使用量
df -T 显示文件系统类型
```
### rm
```
rm -i filename.txt 删除文件先确认
rm -r example 递归删除目录下的文件
```
### mv
```
mv -i file1 file2 是否覆盖
mv -v file1 file2 输出重命名的过程
```
### cp
```
cp -p file1 file2 拷贝保持文件的权限，属性和时间戳
cp -i file1 file2 拷贝文件是否覆盖
cp -r file1 file2 拷贝目录下的所有文件
```
### mount
```
mount /dev/sdb1 /u01 这个文件系统挂载到这个目录上
```
### cat
```
cat file1 file2 查看多个文件的内容
cat -n /etc/logrotate.conf 按照行号去查询数据
```
### tail
```
tail -n N filename.txt 按照行数查看
tail -n -f 1000 filename.txt 实时查看
```
### less
```
less huge-log-file.log 加载大型文件
```
### sed(替换指令)
```
sed 's/.$//' filename 
sed 's#/usr/local/#/usr/src/local/#'
```
### awk(指令)
```
删除重复行：$ awk '!($0 in array) { array[$0]; print}' temp 。
打印 /etc/passwd 中所有包含同样的 uid 和 gid 的行：awk -F ':' '$3=$4' /etc/passwd 。
打印文件中的指定部分的字段：awk '{print $2,$5;}' employee.txt 。
```
### diff(比较指令)
```
diff -w name_list.txt name_list_new.txt
```
### sort(排序指令)
```
以升序对文件内容排序：sort names.txt 。
以降序对文件内容排序：sort -r names.txt 。
以第三个字段对 /etc/passwd 的内容排序：sort -t: -k 3n /etc/passwd | more
```
### xargs(xargs 是给命令传递参数的一个过滤器，也是组合多个命令的一个工具)
```
将所有图片文件拷贝到外部驱动器：ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory 。
将系统中所有 jpg 文件压缩打包：find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz 。
下载文件中列出的所有 url 对应的页面：cat url-list.txt | xargs wget –c 。
```
### tar
```
创建一个新的 tar 文件： tar cvf archive_name.tar dirname/ 。
解压 tar 文件：tar xvf archive_name.tar 。
查看 tar 文件：tar tvf archive_name.tar 。
```
### chmod
```
给指定文件的属主和属组所有权限(包括读、写、执行)：chmod ug+rwx file.txt 。
删除指定文件的属组的所有权限：chmod g-rwx file.txt 。
修改目录的权限，以及递归修改目录下面所有文件和子目录的权限：chmod -R ug+rwx file.txt
```
### chown
```
同时将某个文件的属主改为 oracle ，属组改为 db ：chown oracle:dba dbora.sh 。
使用 -R 选项对目录和目录下的文件进行递归修改：chown -R oracle:dba /home/oracle 。
```
### ps
```
查看当前正在运行的所有进程：ps -ef | more 。
以树状结构显示当前正在运行的进程，H 选项表示显示进程的层次结构：ps -efH | more 。
```
### uptime(机器负载)
```
这个命令可以快速查看机器的负载情况。在 Linux 系统中，这些数据表示等待 CPU 资源的进程和阻塞在不可中断 IO 进程（进程状态为 D）的数量。这些数据可以让我们对系统资源使用有一个宏观的了解
```

### dmesg(机器负载)
```
该命令会输出系统日志的最后 10 行。示例中的输出，可以看见一次内核的 oom kill 和一次 TCP 丢包。这些日志可以帮助排查性能问题
```
### vmstat(核心指标)
```
vmstat 命令，每行会输出一些系统核心指标，这些指标可以让我们更详细的了解系统状态。后面跟的参数 1 ，表示每秒输出一次统计信息，表头提示了每一列的含义，这几介绍一些和性能调优相关的列：

r：等待在 CPU 资源的进程数。这个数据比平均负载更加能够体现 CPU 负载情况，数据中不包含等待 IO 的进程。如果这个数值大于机器 CPU 核数，那么机器的 CPU 资源已经饱和。
free：系统可用内存数（以千字节为单位），如果剩余内存不足，也会导致系统性能问题。下文介绍到的 free 命令，可以更详细的了解系统内存的使用情况。
si，so：交换区写入和读取的数量。如果这个数据不为 0 ，说明系统已经在使用交换区（swap），机器物理内存已经不足。
us, sy, id, wa, st：这些都代表了 CPU 时间的消耗，它们分别表示用户时间(user)、系统（内核）时间(sys)、空闲时间(idle)、IO等待时间(wait)和被偷走的时间(stolen，一般被其他虚拟机消耗)。
上述这些 CPU 时间，可以让我们很快了解 CPU 是否处于繁忙状态。一般情况下，如果用户时间和系统时间相加非常大，CPU 出于忙于执行指令。如果IO等待时间很长，那么系统的瓶颈可能在磁盘 IO 。
```
### mpstat
```
该命令可以显示每个 CPU 的占用情况，如果有一个 CPU 占用率特别高，那么有可能是一个单线程应用程序引起的
```
### pidstat
```
pidstat 命令输出进程的 CPU 占用率，该命令会持续输出，并且不会覆盖之前的数据，可以方便观察系统动态
```
### iostat
```
r/s, w/s, rkB/s, wkB/s：分别表示每秒读写次数和每秒读写数据量（千字节）。读写量过大，可能会引起性能问题。
await：IO 操作的平均等待时间，单位是毫秒。这是应用程序在和磁盘交互时，需要消耗的时间，包括 IO 等待和实际操作的耗时。如果这个数值过大，可能是硬件设备遇到了瓶颈或者出现故障。
avgqu-sz：向设备发出的请求平均数量。如果这个数值大于 1 ，可能是硬件设备已经饱和（部分前端硬件设备支持并行写入）。
%util：设备利用率。这个数值表示设备的繁忙程度，经验值是如果超过 60 ，可能会影响 IO 性能（可以参照 IO 操作平均等待时间）。如果到达 100% ，说明硬件设备已经饱和。
如果显示的是逻辑设备的数据，那么设备利用率不代表后端实际的硬件设备已经饱和。值得注意的是，即使 IO 性能不理想，也不一定意味这应用程序性能会不好，可以利用诸如预读取、写缓存等策略提升应用性能
```
### free
```
free 命令可以查看系统内存的使用情况，-m 参数表示按照兆字节展示。最后两列分别表示用于IO缓存的内存数，和用于文件系统页缓存的内存数。需要注意的是，第二行 -/+ buffers/cache ，看上去缓存占用了大量内存空间。
```
### buffer 和 cache 如何区分
```
当 CPU 需要写数据到磁盘时，由于磁盘速度比较慢，所以 CPU 先把数据存进 Buffer ，然后 CPU 去执行其他任务，Buffer中的数据会定期写入磁。
当 CPU 需要从磁盘读入数据时，由于磁盘速度比较慢，可以把即将用到的数据提前存入 Cache ，CPU 直接从 Cache中 拿数据要快的多。
```
### sar
```
sar命令在这里用于查看 TCP 连接状态，其中包括：
active/s：每秒本地发起的TCP连接数，既通过connect调用创建的TCP连接；
passive/s：每秒远程发起的TCP连接数，即通过accept调用创建的TCP连接；
retrans/s：每秒TCP重传数量；
TCP 连接数可以用来判断性能问题是否由于建立了过多的连接，进一步可以判断是主动发起的连接，还是被动接受的连接。TCP 重传可能是因为网络环境恶劣，或者服务器压力。
```
### top
```
top 命令包含了前面好几个命令的检查的内容。比如系统负载情况（uptime）、系统内存使用情况（free）、系统 CPU 使用情况（vmstat）等。因此通过这个命令，可以相对全面的查看系统负载的来源。同时，top 命令支持排序，可以按照不同的列排序，方便查找出诸如内存占用最多的进程、CPU占用率最高的进程等。

但是，top 命令相对于前面一些命令，输出是一个瞬间值，如果不持续盯着，可能会错过一些线索。这时可能需要暂停 top 命令刷新，来记录和比对数据。
```
### netstat
```
netstat -lnp 系统都开启了哪些端口
netstat -an 查看网络链接情况
netstat -an | grep ESTABLISHED | wc -l 查看当前进程数
netstat -an | grep 3306 //查看端口进程
```
