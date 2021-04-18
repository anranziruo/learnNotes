### 用户和组

#### uid和gid的含义
```
每个登入的使用者至少都会取得两个 ID ,一个是使用者 ID (User ID ,简称UID)、一个是群组 ID (Group ID ,简称 GID
查看uid和gid的方法
id 名称
id Teamcity
uid=1001(Teamcity) gid=1001(Teamcity) 组=1001(Teamcity)=>输出结果
/etc/passwd
Teamcity:x:1001:1001:,,,:/home/Teamcity:/bin/bash
用户名=>密码别名=>uid:gid:用户信息栏:工作目录:bash
/etc/shadow
Teamcity:vZxbvasgdfwfasgf1sdgf
用户名=>密码
/etc/group
Teamcity:x:1001:
组名:密码别名:gid
```
