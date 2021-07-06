# Linux学习笔记

**学习地址：[【小白入门 通俗易懂】2021韩顺平 一周学会Linux_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV1Sv411r7vd)**

韩老师忠实小粉丝~~~

**感谢韩老师的讲解视频，十分感谢！**

## 1. 文件和目录

### 1.1 cd 命令

- cd ~ 回到自己的家目录
- cd /home 进入 '/ home' 目录' 
- cd .. 返回上一级目录 
- cd ../.. 返回上两级目录 
- cd 进入个人的主目录 
- cd - 返回上次所在的目录 

### 1.2 pwd 命令

- pwd 显示工作路径

### 1.3 ls 命令

- ls 查看目录中的文件 
- ls -F 查看目录中的文件 
- ls -l 显示文件和目录的详细资料 
- ls -a 显示隐藏文件 
- ls *[0-9]* 显示包含数字的文件名和目录名
- ls -l >文件 列表的内容写入文件中 覆盖写
- ls -al >>文件 列表的内容追加到文件中的末尾

### 1.4 cp 命令

- cp file1 file2 复制一个文件 
- cp dir/* . 复制一个目录下的所有文件到当前工作目录 
- cp -a /tmp/dir1 . 复制一个目录到当前工作目录 
- cp -a dir1 dir2 复制一个目录 
- cp -r dir1 dir2 复制一个目录及子目录

### 1.5 mv 命令

- mv oldNameFile newNameFile 功能描述:重命名
- mv /temp/movefile /targetFolder 功能描述:移动文件

### 1.6 rm 命令

- rm -f file1 删除一个叫做 'file1' 的文件' 
- rmdir dir1 删除一个叫做 'dir1' 的目录' 
- rm -rf dir1 删除一个叫做 'dir1' 的目录并同时删除其内容 
- rm -rf dir1 dir2 同时删除两个目录及它们的内容 
- mv dir1 new_dir 重命名/移动 一个目录 

### 1.7 mkdir 命令

- mkdir dir1 创建一个叫做 'dir1' 的目录' 
- mkdir dir1 dir2 同时创建两个目录 
- mkdir -p /tmp/dir1/dir2 创建一个目录树 

### 1.8 > 和 >> 命令

输出和重定向

-  ls -l > 文件 列表的内容写入文件(覆盖写)
- cat  文件1> 文件2 将文件1的内容覆盖到文件2
- echo “内容” >> 文件 追加 

## 2. 查看文件内容

### 2.1 cat 命令

- cat file1 从第一个字节查看文件内容
- head 文件   用于显示文件的开头部分 默认是前10行内容
- head -n 5 文件 显示前5行
- tail 文件   用于显示文件的末尾部分 默认是后10行内容
- tail -f 文件 实时追踪所有文档的更新
- cat 文件1>文件2 将文件1的内容覆盖到文件2



### 2.2 find 命令

- find -name 按照指定的文件名查找模式查找文件
- find -user 用户名 查找属于指定用户名所有文件
- find -size 按照指定的文件大小查找文件



## 3. 文件权限

### 3.1 chmod 命令

- ls -lh 显示权限
- chmod ugo+rwx directory1 设置目录所有人(u),群组(g),其他人(o)以读(r,4),写(w,2),执行(x,1)
- chown 用户名 文件名  修改文件所有者
- chown newowner 文件/目录  改变所有者
- chown newowner:newgroup 文件/目录 改变所有者和所在组
- chgrp newgroup 文件/目录 改变所在组

### 3.2 组名相关命令

- groupadd 组名 创建组
- useradd -g 组名 用户名 
- chgro 组名 文件名/目录名   修改文件所在的组
- usermod -g 组名 用户名 
- usermod -d 目录名 用户名 改变该用户登录的初始目录

### 3.3 grep指令

grep 选项 查找内容 源文件

- grep 选项 查找内容 源文件
- grep -n 显示匹配行及行号
- grep -i  忽略字母大小写

| 选项 | 功能           |
| ---- | -------------- |
| -n   | 忽略匹配及行号 |
| -i   | 忽略字母大小写 |

## 4. 文件打包和解压

### 4.1 tar 命令

- tar 选项 XXX.tar.gz 打包的内容   打包目录，压缩后的文件格式.tar.gz
- tar -c 产生.tar打包文件
- tar -v 显示详细信息
- tar -f 指定压缩后的文件名
- tar -z 打包同时压缩
- tar -x 解包.tar文件
- gzip 文件 压缩文件 只能将文件压缩为*.gz文件
- gunzip 文件.gz 解压缩文件命令
- zip 选项 XXX.zip 将要压缩的内容 压缩文件和目录的命令
- unzip 选项 XXX.zip 解压缩文件 
- zip -r 递归压缩 即压缩目录
- unzip -d 目录 指定解压后文件的存放目录

## 5. 系统相关命令

### 5.1 ps 命令

- ps -a 显示当前终端的所有进程信息
- ps -u 以用户的格式显示进程信息
- ps -x 显示后台进程运行的参数

### 5.2 kill 命令

- kill 选项 进程号 通过进程号杀死进程
- kill all 进程名称  通过进程名称杀死进程 也支持通配符 
- kill -9 表示强迫进程立即停止

### 5.3 postree 命令

- pstree 选项 可以更加直观的来查看进程信息
- pstree -p 显示进程的PID
- pstree -u 显示进程的所属用户

### 5.4 systemctl 命令

- systemctl list-unit-files |服务名 查看服务器开机启动状态 ，grep可以过滤
- systemctl enable 服务名 设置服务开机启动
- systemctl disable 服务名 关闭服务开机启动
- systemctl is-enabled 服务名 查询某个服务是否自启动

### 5.5  crontab  命令

-  crontab -e 编辑crontab定时任
- crontab -l 查询crontab任务 
- crontab -r 删除当前用户所有的crontab任务

### 5.6 top 命令

- top -P 以CPU使用率排序，默认是此项
- top -M 以内存的使用率排序
- top -N 以PID排序
- top -q 退出top

### 5.7 查看进程命令

- netstat -an 查看系统网络情况 按一定顺序排列输出
- netstat -p 显示哪个进程在调用
- nestat -tunlp|grep 端口号 筛选查询

### 5.8 关机命令相关

- shutdown -h now 关闭系统
- init 0 关闭系统
- telinit 0 关闭系统
- shutdown -h hours:minutes & 按预定时间关闭系统 
- shutdown -c 取消按预定时间关闭系统 
- shutdown -r now 重启
- reboot 重启
- logout 注销 

### 5.9 firewall 命令

- 打开端口 firewall-cmd-permanent-add-port=端口号/协议
- 关闭端口 firewall-cmd-permanet-remove-port=端口号/协议
- 重新载入，才能生效 firewall-cmd --reload
- 查询端口是否开放 firewall-cmd --query-port=端口/协议

## 6. 其他命令方便查阅

> arch 显示机器的处理器架构
> uname -m 显示机器的处理器架构
> uname -r 显示正在使用的内核版本 
> dmidecode -q 显示硬件系统部件 - (SMBIOS / DMI) 
> hdparm -i /dev/hda 罗列一个磁盘的架构特性 
> hdparm -tT /dev/sda 在磁盘上执行测试性读取操作 
> cat /proc/cpuinfo 显示CPU info的信息 
> cat /proc/interrupts 显示中断 
> cat /proc/meminfo 校验内存使用 
> cat /proc/swaps 显示哪些swap被使用 
> cat /proc/version 显示内核的版本 
> cat /proc/net/dev 显示网络适配器及统计 
> cat /proc/mounts 显示已加载的文件系统 
> lspci -tv 罗列 PCI 设备 
> lsusb -tv 显示 USB 设备 
> date 显示系统日期 
> cal 2007 显示2007年的日历表 
> date 041217002007.00 设置日期和时间 - 月日时分年.秒 
> clock -w 将时间修改保存到 BIOS 
>
> locate 文件名 搜索文件
>
> which 指令 可以查看某个指令在哪个目录下 
>
> x：表示可以进入到该目录，比如cd
>
> r：表示可以ls 将目录的内容显示
>
> w： 表示可以在该目录，删除或者创建文件
>
> chkconfig -- list |grep xxx
>
> chkconfig 服务名 --list
>
> chkconfig --level 5 服务名 on/off
>
> tree 显示文件和目录由根目录开始的树形结构
>lstree 显示文件和目录由根目录开始的树形结构
> 
>ln -s file1 lnk1 创建一个指向文件或目录的软链接 
> ln file1 lnk1 创建一个指向文件或目录的物理链接 
>touch -t 0712250000 file1 修改一个文件或目录的时间戳 - (YYMMDDhhmm) 
> file file1 outputs the mime type of the file as text 
>iconv -l 列出已知的编码 
> 
>echo 输出内容到控制台
> 
>echo “内容” >>文件 追加
> 
>ln -s原文件或目录 软连接名  给原文件创建一个软链接
> 
>history 查看曾经执行过的历史命令 history 10 最近10条
> 
>！5 曾经执行的第五条命令
> 
>ls -ahl 查看文件的所有者
> 
>ps -ef 显示所有进程
> 
>ps -ef | grep atd 可以检测atd是否在运行
> 
>at 选项 时间
> 
>Ctrl +D 结束at命令的输入
> 
>lsblk 查看所有设备挂载情况
> 
>mount 设备名称 挂载目录
> 
>unmount 设备名称或挂载目录

**more 要查看的文件**


| 操作          | 功能说明                             |
| ------------- | ------------------------------------ |
| 空白键(space) | 代表向下翻一页                       |
| Enter         | 代表向下翻一行                       |
| q             | 代表立刻离开more，不再显示该文件内容 |
| Ctrl+F        | 向下滚动一屏                         |
| Ctrl+B        | 返回上一屏                           |
| =             | 输出当前行的行号                     |
| :f            | 输出文件名和当前行的行号             |

**find 指令**

find 搜索范围 选项

| 选项            | 功能                             |
| --------------- | -------------------------------- |
| -name<查询方式> | 安装指定的文件名查找模式查找文件 |
| -user<用户名>   | 按照指定用户名所有文件           |
| -size<文件大小> | 按照指定文件大小查找文件         |

**tar 指令**

tar 选项 XXX.tar.gz 打包的内容 (功能描述，打包目录，压缩后的文件格式是tar.gz)

| 选项 | 功能               |
| ---- | ------------------ |
| -c   | 产生.tar打包文件   |
| -v   | 显示详细信息       |
| -f   | 指定压缩后的文件名 |
| -z   | 打包同时压缩       |
| -x   | 解包.tar文件       |

**crontab 选项**

| -e   | 编辑crontab定时任务           |
| ---- | ----------------------------- |
| -l   | 查询crontab的任务             |
| -r   | 删除当前所有用户的crontab任务 |

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-05-18_15-12-46.png)

![Snipaste_2021-05-18_15-13-09](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-05-18_15-13-09.png)

![Snipaste_2021-05-18_15-13-15](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-05-18_15-13-15.png)

> 案例  每隔1分钟，将当前信息的日期信息追加到 /tmp/mydate 目录下

```shell
$ */**** date>> /tmp/mydate
```

> 案例2 每隔1分钟，将当前日期和日历都追加到 /home/mycal 文件中

```shell
$ vim /home/my.sh 写入内容 date>> /home/mycal 和 cal >> /home/mycal

$ 给my.sh 增加执行权限 chomd u+x /home/my.sh

$ crontab -e 增加 */1**** /home/my.sh
```

> 案例3  每人凌晨2:00 将mysql数据库 testdb，备份到文件中，提示 指令为: mysqldump -u root -p 密码 数据库 > /home/db.bak

```shell
$ crontab -e
$ 0 2 *** mysqldump -u root -proot 数据库名 >home/db.bak
```



## 7. 安装相关软件

### 7.1 安装JDK

```shell
$ mkdir /opt/jdk
$ 通过xtfp上传到 /opt/jdk下
$ cd /opt/jdk
$ 解压 tar -zxvf 压缩包名
$ mkdir /usr/local/java
$ mv /opt/jkd/jdk1.8.0_261 /usr/local/java
$ 配置环境变量 vim/etc/profile
$ export JAVA_HOME=/usr/local/java/jdk1.8.0_261
$ export PATH=$JAVA_HOME/bin:$PATH
$ source /etc/profile 让新的环境变量生效
```



### 7.2安装tomcat

```shell
$ 上传文件到/opt/tomcat 并解压
$ 进入bin目录 启动tomact ./startup.sh
$ 开放端口8080 测试 ip+8080
```



### 7.3安装mysql

> 1. 新建文件夹/opt/mysql，并cd进去
>
> 2. 运行wget http://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar，下载mysql安装包
>
>  
>
> PS：centos7.6自带的类mysql数据库是mariadb，会跟mysql冲突，要先删除。
>
>  
>
> 3. 运行tar -xvf mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar 
>
> 4. 运行rpm -qa|grep mari，查询mariadb相关安装包
>
>  
>
>    5.运行rpm -e --nodeps mariadb-libs，卸载
>
> 6. 然后开始真正安装mysql，依次运行以下几条
>
>  
>
> ​	rpm -ivh mysql-community-common-5.7.26-1.el7.x86_64.rpm
>
> ​	rpm -ivh mysql-community-libs-5.7.26-1.el7.x86_64.rpm
>
> ​	rpm -ivh mysql-community-client-5.7.26-1.el7.x86_64.rpm
>
> ​	rpm -ivh mysql-community-server-5.7.26-1.el7.x86_64.rpm
>
>  
>
> 7. 运行systemctl start mysqld.service，启动mysql
>
> 8. 然后开始设置root用户密码
>
>    Mysql自动给root用户设置随机密码，运行grep "password" /var/log/mysqld.log可看到当前密码
>
>    
>
> 9. 运行mysql -u root -p，用root用户登录，提示输入密码可用上述的，可以成功登陆进入mysql命令行
>
> 
>
> 10. 设置root密码，对于个人开发环境，如果要设比较简单的密码（**生产环境服务器要设复杂密码**），可以运行
>
>     set global validate_password_policy=0; 提示密码设置策略
>
>     （validate_password_policy默认值1，）
>
> 11. set password for 'root'@'localhost' =password('hspedu100');
>
>     运行flush privileges;使密码设置生效

## 8. Linux 面试题

- 分析日志t.log(访问量),将各个ip地址截取，并统计出现次数，并按从小到大的排序

![](C:\Users\dell\Desktop\Notes\Linux\img\Snipaste_2021-05-18_15-50-02.png)

```shell
$ cat t.txt | cut -d '/' -f3 |sort|uniq -c |sort -nr
```

>  忘记mysql数据库的密码，如何找回？

- 首先，启动系统，进入开机界面，在界面中按“e”进入编辑界面。如图

![image-20210518155401846](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210518155401846.png)

- 进入编辑界面，使用键盘上的上下键把光标往下移动，找到以““Linux16”开头内容所在的行数”，在行的最后面输入：init=/bin/sh。如图

![image-20210518155436939](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210518155436939.png)

-  接着，输入完成后，直接按快捷键：Ctrl+x 进入**单用户模式**。
- 接着，在光标闪烁的位置中输入：mount -o remount,rw /（注意：各个单词间有空格），完成后按键盘的回车键（Enter）。如图

![image-20210518155507278](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210518155507278.png)

-  在新的一行最后面输入：passwd， 完成后按键盘的回车键（Enter）。输入密码，**然后再次确认密码即**可(**提示:** **密码长度最好8位以上,但不是必须的**), 密码修改成功后，会显示passwd.....的样式，说明密码修改成功

![image-20210518155541924](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210518155541924.png)

-   接着，在鼠标闪烁的位置中（最后一行中）输入：touch /.autorelabel（注意：touch与 /后面有一个空格），完成后按键盘的回车键（Enter）
-   继续在光标闪烁的位置中，输入：exec /sbin/init（注意：exec与 /后面有一个空格），完成后按键盘的回车键（Enter）,等待系统自动修改密码(**提示：这个过程时间可能有点长，耐心等待**)，完成后，系统会自动重启, **新的密码生效**了

![image-20210518155611979](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210518155611979.png)

> 统计ip访问情况，要求分析nginx访问日志(access.log)找出访问页面数量前2位的ip

```shell
$ cat access.log |awk -F " " '{print$1}'|sort|uniq -c |sort -nr |head -2
```



> 如果你是系统管理员，在进行Linux系统权限划分时，考虑哪些因素

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-05-18_15-59-59.png)

- 注意权限分离
- 权限最小原则
- 减少使用root用户 尽量用普通用户+sudo提权进行日常操作
- 重要的系统文件 比如 /etc/password /etc/shadow 日常建议使用 chattr(change attribute)锁定 需要操作再打开

> 权限操作思考题

- l)用户 tom对目录/home/test 有执行x和读r写w权限，/homeltesthelo.java是只读文件，问tom对 hellojava文件
  能读吗(ok)?能修改吗(no)?能删除吗?(ok)
- 2)用户tom对目录/homeltest 只有读写权限, home/testhellojava是只读文件,问tom对hello.java文件能读吗(no)?能
  修改吗(no)?能删除吗(no)?
- 3)用户tom对目录/home/test 只有执行权限x， /home/test/hello.java是只读文件，问 tom对 hello.java 文件能读吗(ok)?
  能修改吗(no)?能删除吗(no)?
- 4)用户tom 对目录/homeltest 只有执行和写权限，homeltesthello.java 是只读文件，问tom对 hello.java文件能读吗
  (ok)?能修改吗(no)?能删除吗(ok)?



> 列举Linux高级命令

netstat//网络状态监控    topll系统运行状态      lsblk//查看硬盘分区find
ps -aux/l查看运行进程               chkconfig/l查看服务启动状态       systemctl//管理系统服务器



> Linux查看内存、io 读写、磁盘存储、端口占用、进程查看命令是什么?(瓜子)

 ```shell
 $ top, iotop, df -lh , netstat -tunlp , ps -aux | grep关心的进程
 ```

> 使用Linux命令计算t2.txt第二列的和并输出
>
> 张三 40
>
> 李四50
>
> 王五60

```shell
$ cat t2.txt|awk -F " " '{sum+=S2} END {print sum}'
```



> 请用指令写出查找当前文件夹(/home）下所有的文本文件内容中包含有字符“cat"

```shell
$ grep -r "cat" /home |cut -d ":" -f 1
```



> 每天晚上10点30分，打包站点目录/var/spool/mail备份到/home目录下(每次备份
> 按时间生成不同的备份包比如按照年月日时分秒）

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Linux%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-05-18_16-21-30.png)



> 如何优化Linux 说出几点

> (1)对linux系统本身的优化-规则(1)不用root ,使用sudo提示权限
> (2)定时的自动更新服务时间.使用nptdate npt1.aliyun.com . 让 croud定时更新
>
> (3)配置yum源，指向国内镜像(清华，163)
>
> (4)配置合理的防火墙策略,打开必要的端口，关闭不必要的端口
>
> (5)打开最大文件数(调整文件的描述的数量)vim/etc/profile ulimit -SHn 65535
>
> (6)配置合理的监控策略
>
> (7)配置合理的系统重要文件的备份策略(
>
> 8)对安装的软件进行优化，比如 nginx ,apache
>
> (9)内核参数进行优化letc/sysctl.conf
>
> (10)锁定一些重要的系统文件chattr /etc/passwd lect/shadow /etc/inittab
>
> (11)禁用不必要的服务 setup , ntsysv

