## Docker学习笔记

- 镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含
  运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

### 1.Docker镜像加载原理

- UnionFS（联合文件系统）

> UnionFS（联合文件系统）：**Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统**，
> 它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系
> 统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基
> 础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。
> 特性：**一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件**
> **系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录**

- Docker镜像加载原理

>docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
>bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启
>动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是
>一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已
>由bootfs转交给内核，此时系统也会卸载bootfs。
>rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标
>准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

- 分层镜像

```shell
docker pull redis  #拉取redis
```

![image-20210306152500153](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306152500153.png)

```shell
docker image inspect redis:latest # 查看镜像分层的方式
```

![image-20210306152556075](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306152556075.png)

- 为什么Docker镜像采用分层的结构

> 资源共享  比如有多个镜像都从相同的Base镜像构建而来，那么宿主机
> 只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服
> 务了，而且镜像的每一层都可以被共享。

#### 理解

所有的 Docker 镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之
上，创建新的镜像层。
举一个简单的例子，假如基于 Ubuntu Linux 16.04 创建一个新的镜像，这就是新镜像的第一层；如果
在该镜像中添加 Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就
会创建第三个镜像层。
该镜像当前已经包含 3 个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![image-20210306152903863](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306152903863.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了
一个简单的例子，每个镜像层包含 3 个文件，而镜像包含了来自两个镜像层的 6 个文件。

![image-20210306152919748](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306152919748.png)

上图中的镜像层跟之前图中的略有区别，主要目的是便于展示文件。
下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有 6 个文件，这是因为最上层中的文件
7 是文件 5 的一个更新版本。

![image-20210306152937874](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306152937874.png)

这种情况下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新
镜像层添加到镜像当中。
Docker 通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统
一的文件系统。
Linux 上可用的存储引擎有 AUFS、Overlay2、Device Mapper、Btrfs 以及 ZFS。顾名思义，每种存储
引擎都基于 Linux 中对应的文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点。
Docker 在 Windows 上仅支持 windowsfilter 一种存储引擎，该引擎基于 NTFS 文件系统之上实现了分
层和 CoW[1]。
下图展示了与系统显示相同的三层镜像。所有镜像层堆叠并合并，对外提供统一的视图。

![image-20210306153005448](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306153005448.png)

- 特点

Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部！
这一层就是我们通常说的容器层，容器之下的都叫镜像层！

### 2.镜像Commit



```shell 
docker commit # 从容器创建一个新的镜像
# 语法
docker commit -m="提交的描述信息" -a="作者" 容器id 要创建的目标镜像名:[标签名]
```

#### 测试

```shell
# 1、从Docker Hub 下载tomcat镜像到本地并运行 -it 交互终端 -p 端口映射
docker run -it -p 8080:8080 tomcat

# 注意：坑爹：docker启动官方tomcat镜像的容器，发现404是因为使用了加速器，而加速器里的tomcat的webapps下没有root等文件！
# 下载tomcat官方镜像，就是这个镜像（阿里云里的tomcat的webapps下没有任何文件）
# 进入tomcat查看cd到webapps下发现全部空的，反而有个webapps.dist里有对应文件，cp -r到webapps下！

#2.进入到tomcat目录中
docker exec -it 容器ID /bin/bash
/usr/local/tomcat # ce webapps/
/usr/local/tomcat/webapps # ls -l # 查看是否存在 docs文件夹
/usr/local/tomcat/webapps # curl localhost:8080/docs/  # 可以看到 docs 返回的
内容
/usr/local/tomcat/webapps # rm -rf docs # 删除它
/usr/local/tomcat/webapps # curl localhost:8080/docs/  # 再次访问返回404
# 3、当前运行的tomcat实例就是一个没有docs的容器，我们使用它为模板commit一个没有docs的
tomcat新镜像， tomcat02

docker ps -l  # 查看容器的id

[root@bogon ~]# docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
46e66e5d6c69        docker.io/tomcat    "catalina.sh run"   10 minutes ago      Up 10 minutes       8080/tcp            focused_khorana
[root@bogon ~]# docker commit -a="zhuangkang" -m="no tomcat docs" 46e66e5d6c69 tomcat02:1.1
sha256:a054127ff9bcfbbfad569cfc1cf52a20987774320ba060e4989f412cb7650f60
[root@bogon ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
tomcat02            1.1                 a054127ff9bc        About a minute ago   672 MB

# 4、这个时候，我们的镜像都是可以使用的，大家可以启动原来的tomcat，和我们新的tomcat02来
测试看看！
[root@kuangshen ~]# docker run -it -p 8080:8080 tomcat02:1.1

测试 LinuxIP:8080 访问成功！
```

![image-20210306154214974](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306154214974.png)

![image-20210306155234112](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306155234112.png)

![image-20210306155710543](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306155710543.png)

![image-20210306160248850](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306160248850.png)

![image-20210306160727354](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306160727354.png)

## 2.容器数据卷

 ### 2.1docker理念回顾

将应用和运行的环境打包形成容器运行，运行可以伴随着容器，但是我们对于数据的要求，是希望能够
持久化的！
就好比，你安装一个MySQL，结果你把容器删了，就相当于删库跑路了，这TM也太扯了吧！
所以我们希望容器之间有可能可以共享数据，Docker容器产生的数据，如果不通过docker commit 生成
新的镜像，使得数据作为镜像的一部分保存下来，那么当容器删除后，数据自然也就没有了！这样是行
不通的！
为了能保存数据在Docker中我们就可以使用卷！让数据挂载到我们本地！这样数据就不会因为容器删除
而丢失了！

### 2.2作用

作用：
卷就是目录或者文件，存在一个或者多个容器中，由docker挂载到容器，但不属于联合文件系统，因此
能够绕过 Union File System ， 提供一些用于持续存储或共享数据的特性：
卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂
载的数据卷。

### 2.3特点

1、数据卷可在容器之间共享或重用数据
2、卷中的更改可以直接生效
3、数据卷中的更改不会包含在镜像的更新中
4、数据卷的生命周期一直持续到没有容器使用它为止
**所以：总结一句话： 就是容器的持久化，以及容器间的继承和数据共享！**

## 3.使用数据卷

- 方式一：容器中 直接用命令来添加

挂载

```shel
# 命令
docker run -it -v 宿主机绝对路径目录:容器内目录 镜像名
# 测试
[root@root~]# docker run -it -v /home/ceshi:/home centos /bin/bash
```

查看数据卷是否挂载成功

```shell
docker inspect 容器ID
```

测试容器和宿主机之间数据是否共享！

测试容器停止退出后，主机修改数据是否会同步！
1. 停止容器
2. 在宿主机上修改文件，增加些内容
3. 启动刚才停止的容器
4. 然后查看对应的文件，发现数据依旧同步！ok

## 4.使用docker安装mysql

```shell
# 1.搜索镜像
docker search mysql
# 2.拉取镜像
docker pull mysql:5.7
# 3启动容器 -e 环境变量！
# 注意： mysql的数据应该不丢失！先体验下 -v 挂载卷！
docker run -d -p 3310:3306 -v
/home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e
MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
# 4、使用本地的sqlyog连接测试一下 3310
# 5、查看本地的 /home/mysql 目录 pwd 
# 6、删除mysql容器
docker rm -f mysql01
```

## 5.匿名和具名挂载

以nginx为例！

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx
# 匿名挂载的缺点，就是不好维护，通常使用命令 docker volume维护
docker volume ls
# 具名挂载
-v 卷名:/容器内路径
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx nginx
# 查看挂载的路径
docker volume inspect nginxconfig

# 怎么判断挂载的是卷名而不是本机目录名？
不是/开始就是卷名，是/开始就是目录名
# 改变文件的读写权限
# ro: readonly
# rw: readwrite
# 指定容器对我们挂载出来的内容的读写权限
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx:rw nginx
```

## 6.DockerFile

微服务打包成镜像，任何装了Docker的地方，都可以下载使用，极其的方便。
流程：开发应用=>DockerFile=>打包为镜像=>上传到仓库（私有仓库，公有仓库）=> 下载镜像 => 启动
运行。

### 6.1概念

dockerfile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。
构建步骤：
1、编写DockerFile文件
2、docker build 构建镜像
3、docker run

### 6.2DockerFile构建过程

#### 6.2.1基础知识

1、每条保留字指令都必须为大写字母且后面要跟随至少一个参数
2、指令按照从上到下，顺序执行
3、# 表示注释
4、每条指令都会创建一个新的镜像层，并对镜像进行提交

#### 6.2.2流程

1、docker从基础镜像运行一个容器
2、执行一条指令并对容器做出修改
3、执行类似 docker commit 的操作提交一个新的镜像层
4、Docker再基于刚提交的镜像运行一个新容器
5、执行dockerfile中的下一条指令直到所有指令都执行完成！

#### 6.2.3说明

从应用软件的角度来看，DockerFile，docker镜像与docker容器分别代表软件的三个不同阶段。
DockerFile 是软件的原材料 （代码）
Docker 镜像则是软件的交付品 （.apk）
Docker 容器则是软件的运行状态 （客户下载安装执行）
DockerFile 面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可！

DockerFile：需要定义一个DockerFile，DockerFile定义了进程需要的一切东西。DockerFile涉及的内容
包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进
程和内核进程（当引用进行需要和系统服务和内核进程打交道，这时需要考虑如何设计 namespace的权
限控制）等等。
Docker镜像：在DockerFile 定义了一个文件之后，Docker build 时会产生一个Docker镜像，当运行
Docker 镜像时，会真正开始提供服务；
Docker容器：容器是直接提供服务的。

## 7.DockerFile指令

#### 7.1关键字

```shell
FROM     # 基础镜像，当前新镜像是基于哪个镜像的
MAINTAINER  # 镜像维护者的姓名混合邮箱地址
RUN      # 容器构建时需要运行的命令
EXPOSE    # 当前容器对外保留出的端口
WORKDIR    # 指定在创建容器后，终端默认登录的进来工作目录，一个落脚点
ENV      # 用来在构建镜像过程中设置环境变量
ADD      # 将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包
COPY     # 类似ADD，拷贝文件和目录到镜像中！
VOLUME    # 容器数据卷，用于数据保存和持久化工作
CMD      # 指定一个容器启动时要运行的命令，dockerFile中可以有多个CMD指令，但只有最
后一个生效！
ENTRYPOINT  # 指定一个容器启动时要运行的命令！和CMD一样
ONBUILD    # 当构建一个被继承的DockerFile时运行命令，父镜像在被子镜像继承后，父镜像的
ONBUILD被触发
```

![image-20210306162408373](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306162408373.png)

### 7.2自定义一个centos

#### 7.2.1.编写DockerFile

![image-20210306162536380](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306162536380.png)

目的：使我们自己的镜像具备如下：登陆后的默认路径、vim编辑器、查看网络配置ifconfig支持
准备编写DockerFlie文件

```shell
# 1.创建一个目录
mkdir dockerfile-test
# 2.编写文件
vim mydockerfile-test

FROM centos
MAINTAINER zhuangkang<2247830091@qq.com>
ENV MYPATH /usr/local # 默认路径
WORKDIR $MYPATH  # 工作路径
RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80
CMD echo $MYPATH
CMD echo "----------end--------"
CMD /bin/bash

# 通过cat命令查看
cat mydockerfile-test
```

#### 7.2.2.构建

docker build -f dockerfile地址 -t 新镜像名字:TAG .

```shell
docker build -f mydockerfile-centos -t mycentos:0.1
.

# 看到下面就是成功！
Successfully built 18888023317c
Successfully tagged mycentos:0.1
```

#### 7.2.3. 运行

```shell
docker run -it 新镜像名字:TAG
```

#### 7.2.4.列出镜像地的变更历史

```shell
docker history 镜像名
```

## 8.Docker网络

首先清空所有容器和镜像

```shell
docker rm -f $(docker ps -a -q)       # 删除所有容器
docker rmi -f $(docker images -qa)      # 删除全部镜像
```

![image-20210306163506426](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306163506426.png)

实际场景中，我们开发了很多微服务项目，那些微服务项目都要连接数据库，需要指定数据库的url地
址，通过ip。但是我们用Docker管理的话，假设数据库出问题了，我们重新启动运行一个，这个时候数
据库的地址就会发生变化，docker会给每个容器都分配一个ip，且容器和容器之间是可以互相访问的。
我们可以测试下容器之间能不能ping通过：

```shell
[root@bogon ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
12: eth0@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe11:2/64 scope link 
       valid_lft forever preferred_lft forever
[root@bogon ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.089 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.048 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.060 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.047 ms
64 bytes from 172.17.0.2: icmp_seq=5 ttl=64 time=0.045 ms

```

#### 8.1原理 

- 1、每一个安装了Docker的linux主机都有一个docker0的虚拟网卡。这是个桥接网卡，使用了veth-pair
  技术！

  ![image-20210306164405468](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306164255054.png)

![image-20210306163525959](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306163525959.png)

![image-20210306163607386](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306163607386.png)

- 2.每启动一个容器，linux主机就会多了一个虚拟网卡。

```shell
docker run -d -P --name tomcat02 tomcat	# 再启动一个tomcat2

docker exec -it tomcat02 ip addr # 查看ip

# 观察现象：
# tomcat --- linux主机 vethc8584ea@if122 ---- 容器内 eth0@if123
# tomcat --- linux主机 veth021eeea@if124 ---- 容器内 eth0@if125
```

- 3.网络模型图

![image-20210306164643127](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306164643127.png)

- 4.结论

- tomcat1和tomcat2共用一个路由器。是的，他们使用的一个，就是docker0。任何一个容器启动
  默认都是docker0网络。
  docker默认会给容器分配一个可用ip。

- Docker使用Linux桥接，在宿主机虚拟一个Docker容器网桥(docker0)，Docker启动一个容器时会根据
  Docker网桥的网段分配给容器一个IP地址，称为Container-IP，同时Docker网桥是每个容器的默认网
  关。因为在同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的Container-IP直接
  通信。

### 8.2所有网络模式

![image-20210306164928025](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/image-20210306164928025.png)

## 9.部署Redis集群

```shell
# 创建网卡
docker network create redis --subnet 172.38.0.0/16
# 通过脚本创建六个redis配置
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server
/etc/redis/redis.conf; \
# 启动6个容器
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server
/etc/redis/redis.conf
docker run -p 6376:6379 -p 16376:16379 --name redis-6 \
-v /mydata/redis/node-6/data:/data \
-v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.16 redis:5.0.9-alpine3.11 redis-server
/etc/redis/redis.conf
# 进入一个redis，注意这里是 sh命令
docker exec -it redis-1 /bin/sh
# 创建集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379
172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --
cluster-replicas 1
# 连接集群
redis-cli -c
# 查看集群信息
cluster info
# 查看节点
cluster nodes
# set a b
# 停止到存值的容器
# 然后再次get a，发现依旧可以获取值
# 查看节点，发现高可用完全没问题
```

## 10.IDEA整合Redis

- 1.创建一个SpringBoot项目
- 2.创建一个HelloController

```java
@RestController
public class HelloController {
 @GetMapping("/hello")
 public String hello(){
   return "hello,world";
 }
}
```

- 3.启动测试，确认端口没有冲突
- 4.用Maven打成jar包
- 打包镜像
- 1.在项目下编写 Dockerfile 文件，将打包好的jar包拷贝到Dockerfile同级目录

```shell
FROM java:8
# 服务器只有dockerfile和jar在同级目录
COPY *.jar /app.jar
CMD ["--server.port=8080"]
# 指定容器内要暴露的端口
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```

- 2.将Dockerfile 和 项目的 jar 包上传到linux服务器上，构建运行

```shell
# 构建镜像
docker build -t idea-ks .
# 查看镜像
docker images
# 运行
docker run -d -P --name idea-ks idea-ks

docker ps
curl localhost:32779
curl localhost:32779/hello
```

## 11.常用命令总结

![](https://gitee.com/zhuang-kang/note-picture/raw/master/%E5%9B%BE%E7%89%87/20200526095411.png)

```shell
$ attach    Attach to a running container                 # 当前 shell 下 attach 连接指定运行镜像

$ build     Build an image from a Dockerfile              # 通过 Dockerfile 定制镜像

$ commit    Create a new image from a container changes   # 提交当前容器为新的镜像

$ cp        Copy files/folders from the containers filesystem to the host path   #从容器中拷贝指定文件或者目录到宿主机中

$ create    Create a new container                        # 创建一个新的容器，同 run，但不启动容器

$ diff      Inspect changes on a container's filesystem   # 查看 docker 容器变化

$ events    Get real time events from the server          # 从 docker 服务获取容器实时事件

$ exec      Run a command in an existing container        # 在已存在的容器上运行命令

$ export    Stream the contents of a container as a tar archive   # 导出容器的内容流作为一个 tar 归档文件[对应 import ]

$ history   Show the history of an image                  # 展示一个镜像形成历史

$ images    List images                                   # 列出系统当前镜像

$ import    Create a new filesystem image from the contents of a tarball # 从tar包中的内容创建一个新的文件系统映像[对应export]

$ info      Display system-wide information               # 显示系统相关信息

$ inspect   Return low-level information on a container   # 查看容器详细信息

$ kill      Kill a running container                      # kill 指定 docker 容器

$ load      Load an image from a tar archive              # 从一个 tar 包中加载一个镜像[对应 save]

$ login     Register or Login to the docker registry server    # 注册或者登陆一个 docker 源服务器

$ logout    Log out from a Docker registry server          # 从当前 Docker registry 退出

$ logs      Fetch the logs of a container                 # 输出当前容器日志信息

$ port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # 查看映射端口对应的容器内部源端口

$ pause     Pause all processes within a container        # 暂停容器

$ ps        List containers                               # 列出容器列表

$ pull      Pull an image or a repository from the docker registry server   # 从docker镜像源服务器拉取指定镜像或者库镜像

$ push      Push an image or a repository to the docker registry server   # 推送指定镜像或者库镜像至docker源服务器

$ restart   Restart a running container                   # 重启运行的容器

$ rm        Remove one or more containers                 # 移除一个或者多个容器

$ rmi       Remove one or more images             # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]

$ run       Run a command in a new container              # 创建一个新的容器并运行一个命令

$ save      Save an image to a tar archive                # 保存一个镜像为一个 tar 包[对应 load]

$ search    Search for an image on the Docker Hub         # 在 docker hub 中搜索镜像

$ start     Start a stopped containers                    # 启动容器

$ stop      Stop a running containers                     # 停止容器

$ tag       Tag an image into a repository                # 给源中镜像打标签

$ top       Lookup the running processes of a container   # 查看容器中运行的进程信息

$ unpause   Unpause a paused container                    # 取消暂停容器

$ version   Show the docker version information           # 查看 docker 版本号

$ wait      Block until a container stops, then print its exit code   # 截取容器停止时的退出状态值
```