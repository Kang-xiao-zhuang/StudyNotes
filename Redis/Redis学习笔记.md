# Redis学习笔记

## 1.NoSQL概述

### 1.1 为什么用NoSQL

在90年代，一个网站的访问量一般不大，用单个数据库完全可以轻松应付！
在那个时候，更多的都是静态网页，动态交互类型的网站不多。
上述架构下，我们来看看数据存储的瓶颈是什么？

1. 数据量的总大小，一个机器放不下时
2. 数据的索引（B+ Tree）一个机器的内存放不下时
3. 访问量（读写混合）一个实例不能承受
如果满足了上述 1 or 3个，进化....
DAL：数据库访问层

![image-20210311174648225](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311174648225.png)

> 2、Memcached（缓存）+ MySQL + 垂直拆分

后来，随着访问量的上升，几乎大部分使用MySQL架构的网站在数据库上都开始出现了性能问题，web程序不再仅仅专注在功能上，同时也在追求性能。程序猿们开始大量使用缓存技术来缓解数据库的压力，优化数据库的结构和索引，开始比较流行的是通过文件缓存来缓解数据库压力，但是当访问量继续增大的时候，多台web机器通过文件缓存不能共享，大量的小文件缓存也带了比较高的IO压力，在这个时候，Memcached就自然的成为一个非常时尚的技术产品。

![image-20210311174736061](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311174736061.png)

> 3、MySQL主从读写分离

由于数据库的写入压力增加，Memcached只能缓解数据库的读取压力，读写集中在一个数据库上让数据库不堪重负，大部分网站开始使用主从复制技术来达到读写分离，以提高读写性能和读库的可扩展性，MySQL的master-slave模式成为这个时候的网站标配了。

![image-20210311174912875](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311174912875.png)

> 4、分表分库 + 水平拆分 + Mysql 集群

在Memcached的高速缓存，MySQL的主从复制，读写分离的基础之上，这时MySQL主库的写压力开始
出现瓶颈，而数据量的持续猛增，由于MyISAM使用表锁，在高并发下会出现严重的锁问题，大量的高
并发MySQL应用开始使用InnoDB引擎代替MyISAM。
同时，开始流行使用分表分库来缓解写压力和数据增长的扩展问题，这个时候，分表分库成了一个热门
技术，是面试的热门问题，也是业界讨论的热门技术问题。也就是在这个时候，MySQL推出了还不太稳
定的表分区，这也给技术实力一般的公司带来了希望。虽然MySQL推出了MySQL Cluster集群，但性能
也不能很好满足互联网的需求，只是在高可靠性上提供了非常大的保证。

![image-20210311175013360](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311175013360.png)

>5、MySQL 的扩展性瓶颈

MySQL数据库也经常存储一些大文本的字段，导致数据库表非常的大，在做数据库恢复的时候就导致非
常的慢，不容易快速恢复数据库，比如1000万4KB大小的文本就接近40GB的大小，如果能把这些数据
从MySQL省去，MySQL将变的非常的小，关系数据库很强大，但是它并不能很好的应付所有的应用场
景，MySQL的扩展性差（需要复杂的技术来实现），大数据下IO压力大，表结构更改困难，正是当前使
用MySQL的开发人员面临的问题。

> 6.如今的样子

![image-20210311175121803](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311175121803.png)

> 7、为什么用NoSQL？

今天我们可以通过第三方平台（如：Google，FaceBook等）可以很容易的访问和抓取数据。用户的个
人信息，社交网络，地理位置，用户生成的数据和用户操作日志已经成倍的增加、我们如果要对这些用
户数据进行挖掘，那SQL数据库已经不适合这些应用了，而NoSQL数据库的发展却能很好的处理这些大
的数据！

### 1.2 什么是NoSQL

> NoSQL

NoSQL = Not Only SQL，意思：不仅仅是SQL；
泛指非关系型的数据库，随着互联网Web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别
是超大规模和高并发的社交网络服务类型的Web2.0纯动态网站已经显得力不从心，暴露了很多难以克服
的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展，NoSQL数据库的产生就是为
了解决大规模数据集合多种数据种类带来的挑战，尤其是大数据应用难题，包括超大规模数据的存储。
（例如谷歌或Facebook每天为他们的用户收集万亿比特的数据）。这些类型的数据存储不需要固定的模
式，无需多余操作就可以横向扩展。

> NoSQL的特点

- 易拓展

NoSQL 数据库种类繁多，但是一个共同的特点都是去掉关系数据库的关系型特性。
数据之间无关系，这样就非常容易扩展，也无形之间，在架构的层面上带来了可扩展的能力。

- 大数据量高性能

NoSQL数据库都具有非常高的读写性能，尤其是在大数据量下，同样表现优秀。这得益于它的非关系
性，数据库的结构简单。
一般MySQL使用Query Cache，每次表的更新Cache就失效，是一种大力度的Cache，在针对Web2.0的
交互频繁应用，Cache性能不高，而NoSQL的Cache是记录级的，是一种细粒度的Cache，所以NoSQL
在这个层面上来说就要性能高很多了。

- 多样灵活的数据模型

NoSQL无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式，而在关系数据库里，增删
字段是一件非常麻烦的事情。如果是非常大数据量的表，增加字段简直就是噩梦。

- 传统的RDBMS VS NoSQL

> ​	**传统的关系型数据库 RDBMS**
>
> - 高度组织化结构化数据
>
> - 结构化查询语言（SQL）
>
> - 数据和关系都存储在单独的表中
>
> - 数据操纵语言，数据定义语言
>
> - 严格的一致性
>
>   **基础事务NoSQL**
>
> - 代表着不仅仅是SQL
>
> - 没有声明性查询语言
>
> - 没有预定义的模式
>
> - 键值对存储，列存储，文档存储，图形数据库
>
> - 最终一致性，而非ACID属性
>
> - 非结构化和不可预知的数据
>
> - CAP定理
>
> - 高性能，高可用性 和 可伸缩性

大数据时代的3V ： 主要是对问题的描述

- 海量 Volume
- 多样 Variety
- 实时 Velocity

互联网需求的3高 ： 主要是对程序的要求

- 高可用
- 高并发
- 高性能

### 1.3 淘宝技术分析

![image-20210311180349058](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180349058.png)

![image-20210311180408024](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180408024.png)

![image-20210311180425596](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180425596.png)

![image-20210311180441641](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180441641.png)

> **大型互联网应用（大数据，高并发，多样数据类型）的难点和解决方案**

**难点：**

- 数据类型的多样性
- 数据源多样性和变化重构
- 数据源改造而数据服务平台不需要大面积重构

**解决办法：**

![image-20210311180659495](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180659495.png)

![image-20210311180720138](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180720138.png)

![image-20210311180852111](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180852111.png)

![image-20210311180910245](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180910245.png)



![image-20210311180924551](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311180924551.png)

### 1.4 NoSQL四大分类

**KV键值：**

- 新浪
- 美团
- 阿里

**文档型数据库（bson格式较多）**

- CouchDB
- MongoDB
  - MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
  - MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

**列存储数据库**

- Cassandra, HBase
- 分布式文件系统

**图关系数据库**

- 它不是放图形的，放的是关系比如:朋友圈社交网络、广告推荐系统
- 社交网络，推荐系统，专注于构建关系图谱

> 四者对比

![image-20210311181647824](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311181647824.png)



### 1.5 CAP+BASE

#### 1.5.1BASE的概念

- A (Atomicity) 原子性

> 原子性很容易理解，也就是说事务里的所有操作要么全部做完，要么都不做，事务成功的条件是事务里的所有操作都成功，只要有一个操作失败，整个事务就失败，需要回滚。
> 比如银行转账，从A账户转100元至B账户，分为两个步骤：
> 1）从A账户取100元；
> 2）存入100元至B账户。
> 这两步要么一起完成，要么一起不完成，如果只完成第一步，第二步失败，钱会莫名其妙少了100元

- C (Consistency) 一致性

> 事物前后数据的完整性必须保持一致

- I (Isolation) 隔离性

> 所谓的独立性是指并发的事务之间不会互相影响，如果一个事务要访问的数据正在被另外一个事务修改，只要另外一个事务未提交，它所访问的数据就不受未提交事务的影响。比如现有有个交易是从A账户转100元至B账户，在这个交易还未完成的情况下，如果此时B查询自己的账户，是看不到新增加的100元的

- D (Durability) 持久性

> 持久性是指一旦事务提交后，它所做的修改将会永久的保存在数据库上，即使出现宕机也不会丢失

#### 1.5.2CAP的概念

- C : Consistency（强一致性）
-  A : Availability（可用性）
- P : Partition tolerance（分区容错性）

CAP理论就是说在分布式存储系统中，最多只能实现上面的两点。
而由于当前的网络硬件肯定会出现延迟丢包等问题，所以分区容错性是我们必须需要实现的。
所以我们只能在一致性和可用性之间进行权衡，没有NoSQL系统能同时保证这三点。
注意：分布式架构的时候必须做出取舍。
一致性和可用性之间取一个平衡。多余大多数web应用，其实并不需要强一致性

因此牺牲C换取P，这是目前分布式数据库产品的方向

**一致性与可用性的决择**
对于web2.0网站来说，关系数据库的很多主要特性却往往无用武之地
数据库事务一致性需求
很多web实时系统并不要求严格的数据库事务，对读一致性的要求很低， 有些场合对写一致性要求并不
高。允许实现最终一致性。



**数据库的写实时性和读实时性需求**
对关系数据库来说，插入一条数据之后立刻查询，是肯定可以读出来这条数据的，但是对于很多web应
用来说，并不要求这么高的实时性，比方说发一条消息之 后，过几秒乃至十几秒之后，我的订阅者才看
到这条动态是完全可以接受的。



**对复杂的SQL查询，特别是多表关联查询的需求**
任何大数据量的web系统，都非常忌讳多个大表的关联查询，以及复杂的数据分析类型的报表查询，特
别是SNS类型的网站，从需求以及产品设计角度，就避免了这种情况的产生。往往更多的只是单表的主
键查询，以及单表的简单条件分页查询，SQL的功能被极大的弱化了。



**CAP理论的核心是：**一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，
最多只能同时较好的满足两个。因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP
原则和满足 AP 原则三 大类：

- CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
- CP - 满足一致性，分区容忍必的系统，通常性能不是特别高。
- AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。



## 2.Redis概述

### 2.1Redis介绍

**Redis：REmote DIctionary Server（远程字典服务器）**

Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。Redis是 NoSQL技术阵营中的一员，它通过多种键值数据类型来适应不同场景下的存储需求，借助一些高层级的接口使其可以胜任如缓存、队列系统的不同角色

**Redis与其他key-value缓存产品有以下三个特点**

- Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的 key-value 类型的数据，同时还提供list、set、zset、hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。

**Redis的优势**

- 性能极高：Redis能读的速度是110000次/s，写的速度是81000次/s

- 丰富的数据类型：Redis支持二进制案例的Strings、Lists、Hashes、Sets及Ordered Sets数据类型操作

- 原子性：Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行

- 丰富的特性：Redis还支持publish/subscribe、通知、key 过期等等特性



官方中文网http://redis.cn/

官网https://redis.io/

### 2.2Redis在Linux环境在安装

1、下载获得 redis-6.2.1.tar.gz 后将它放到我们Linux的目录下 /opt

2、/opt 目录下，解压命令 ： tar -zxvf redis-6.2.1.tar.gz

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310200821842.png)

3、解压完成后出现文件夹：redis-5.0.7

![image-20210310201019977](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310201019977.png)

> **运行make命令时故意出现的错误解析**：
>
> 1. 安装gcc (gcc是linux下的一个编译程序，是c程序的编译工具)
>  能上网: yum install gcc-c++
>  版本测试: gcc-v
> 2. 二次make
> 3. Jemalloc/jemalloc.h: 没有那个文件或目录
>  运行 make distclean 之后再make
> 4. Redis Test（可以不用执行）

4、进入目录： cd redis-6.2.1

5、在 redis-6.2.1 目录下执行 make 命令

6、如果make完成后继续执行 make install

7、查看默认安装目录：usr/local/bin

8、拷贝设置文件

> /usr 这是一个非常重要的目录，类似于windows下的Program Files,存放用户的程序

> cd /usr/local/bin
> ls -l
>
> **在redis的解压目录下备份redis.conf**
>
> mkdir myredis
> cp redis.conf myredis  # 拷一个备份，养成良好的习惯，我们就修改这个文件
>
> **修改配置保证可以后台应用**
>
> vim redis.conf

![image-20210310201256444](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310201256444.png)

![image-20210310201323971](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310201323971.png)

![image-20210310201530209](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310201530209.png)

![image-20210310202030827](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310202030827.png)

![image-20210310202158878](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310202158878.png)

![image-20210311184758459](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311184758459.png)

- A、redis.conf配置文件中daemonize守护线程，默认是NO。
- B、daemonize是用来指定redis是否要用守护线程的方式启动。

**daemonize 设置yes或者no区别**

- daemonize:yes

  - redis采用的是单进程多线程的模式。当redis.conf中选项daemonize设置成yes时，代表开启
    守护进程模式。在该模式下，redis会在后台运行，并将进程pid号写入至redis.conf选项
    pidfile设置的文件中，此时redis将一直运行，除非手动kill该进程

- daemonize:no

  - 当daemonize选项设置成no时，当前界面将进入redis的命令行界面，exit强制退出或者关闭
    连接工具(putty,xshell等)都会导致redis进程退出。

  9、启动测试

  ```bash
  #【shell】启动redis服务
  
  [root@192 bin]# cd /usr/local/bin
  [root@192 bin]# redis-server /opt/redis-5.0.7/redis.conf
  
  redis客户端连接===> 观察地址的变化，如果连接ok,是直接连上的，redis默认端口号 6379
  
  [root@192 bin]# redis-cli -p 6379
  127.0.0.1:6379> ping
  PONG
  127.0.0.1:6379> set k1 helloworld
  OK
  127.0.0.1:6379> get k1
  "helloworld"
  
  #【shell】ps显示系统当前进程信息
  
  [root@192 myredis]# ps -ef|grep redis
  root    16005    1  0 04:45 ?     00:00:00 redis-server
  127.0.0.1:6379
  root    16031  15692  0 04:47 pts/0   00:00:00 redis-cli -p 6379
  root    16107  16076  0 04:51 pts/2   00:00:00 grep --color=auto redis
  
  #【redis】关闭连接
  
  127.0.0.1:6379> shutdown
  not connected> exit
  
  # 【shell】ps显示系统当前进程信息
  [root@192 myredis]# ps -ef|grep redis
  root    16140  16076  0 04:53 pts/2   00:00:00 grep --color=auto redis
  ```

![image-20210310203013001](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310203013001.png)

![image-20210310203227429](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310203227429.png)

![image-20210310203328902](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310203328902.png)

![image-20210310204335734](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210310204335734.png)

文件参数详解

- redis-server ：Redis服务器
- redis-cli ：Redis客户端
- redis-sentinel ：Redis哨兵端
- redis-benchmark：Redis性能测试工具
- redis-check-aof ：AOF文件修复工具
- redis-check-rdb ：RDB文件检索工具

## 3.五大数据类型

![image-20210311191512620](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210311191512620.png)

**全段翻译：**
Redis是一个开放源代码（BSD许可）的内存中数据结构存储，用作数据库，缓存和消息代理。它支持数
据结构，例如字符串，哈希，列表，集合，带范围查询的排序集合，位图，超日志，带有半径查询和流
的地理空间索引。Redis具有内置的复制，Lua脚本，LRU驱逐，事务和不同级别的磁盘持久性，并通过
Redis Sentinel和Redis Cluster自动分区提供了高可用性。

**Redis键(Key)**

```shell
# keys * 查看所有的key
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name zhuangkang
OK
127.0.0.1:6379> keys *
1) "name"
# exists key 的名字，判断某个key是否存在
127.0.0.1:6379> EXISTS name
(integer) 1
127.0.0.1:6379> EXISTS name1
(integer) 0
# move key db ---> 当前库就没有了，被移除了
127.0.0.1:6379> move name 1
(integer) 1
127.0.0.1:6379> keys *
(empty list or set)
# expire key 秒钟：为给定 key 设置生存时间，当 key 过期时(生存时间为 0 )，它会被自动删除。
# ttl key 查看还有多少秒过期，-1 表示永不过期，-2 表示已过期
127.0.0.1:6379> set name zhuangkang
OK
127.0.0.1:6379> EXPIRE name 10
(integer) 1
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
(integer) 3
127.0.0.1:6379> ttl name
(integer) 2
127.0.0.1:6379> ttl name
(integer) 1
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> keys *
(empty list or set)
# type key 查看你的key是什么类型
127.0.0.1:6379> set name zhuangkang
OK
127.0.0.1:6379> get name
"zhuangkang"
127.0.0.1:6379> type name
string
```

### 3.1 String(字符串类型)

String是redis最基本的类型，你可以理解成Memcached一模一样的类型，一个key对应一个value。
String类型是二进制安全的，意思是redis的string可以包含任何数据，比如jpg图片或者序列化的对象。
String类型是redis最基本的数据类型，一个redis中字符串value最多可以是512M。

**常用命令解析**

```shell
# ===================================================
# set、get、del、append、strlen
# ===================================================
127.0.0.1:6379> set key1 value1  # 设置值
OK
127.0.0.1:6379> get key1      # 获得key
"value1"
127.0.0.1:6379> del key1      # 删除key
(integer) 1
127.0.0.1:6379> keys *       # 查看全部的key
(empty list or set)
127.0.0.1:6379> exists key1    # 确保 key1 不存在
(integer) 0
127.0.0.1:6379> append key1 "hello"  # 对不存在的 key 进行 APPEND ，等同于 SET
key1 "hello"
(integer) 5   # 字符长度
127.0.0.1:6379> APPEND key1 "-2333"  # 对已存在的字符串进行 APPEND
(integer) 10   # 长度从 5 个字符增加到 10 个字符
127.0.0.1:6379> get key1
"hello-2333"
127.0.0.1:6379> STRLEN key1    # # 获取字符串的长度
(integer) 10 
# ===================================================
# incr、decr   一定要是数字才能进行加减，+1 和 -1。
# incrby、decrby 命令将 key 中储存的数字加上指定的增量值。
# ===================================================
127.0.0.1:6379> set views 0    # 设置浏览量为0
OK
127.0.0.1:6379> incr views     # 浏览 + 1
(integer) 1
127.0.0.1:6379> incr views     # 浏览 + 1
(integer) 2
127.0.0.1:6379> decr views     # 浏览 - 1
(integer) 1
127.0.0.1:6379> incrby views 10  # +10
(integer) 11
127.0.0.1:6379> decrby views 10  # -10
(integer) 1
# ===================================================
# range [范围]
# getrange 获取指定区间范围内的值，类似between...and的关系，从零到负一表示全部
# ===================================================
127.0.0.1:6379> set key2 abcd123456  # 设置key2的值
OK
127.0.0.1:6379> getrange key2 0 -1  # 获得全部的值
"abcd123456"
127.0.0.1:6379> getrange key2 0 2   # 截取部分字符串
"abc"
# ===================================================
# setrange 设置指定区间范围内的值，格式是setrange key值 具体值
# ===================================================
127.0.0.1:6379> get key2
"abcd123456"
127.0.0.1:6379> SETRANGE key2 1 xx  # 替换值
(integer) 10
127.0.0.1:6379> get key2
"axxd123456"
# ===================================================
# setex（set with expire）键秒值
# setnx（set if not exist）
# ===================================================
127.0.0.1:6379> setex key3 60 expire  # 设置过期时间
OK
127.0.0.1:6379> ttl key3  # 查看剩余的时间
(integer) 55
127.0.0.1:6379> setnx mykey "redis"  # 如果不存在就设置，成功返回1
(integer) 1
127.0.0.1:6379> setnx mykey "mongodb"  # 如果存在就设置，失败返回0
(integer) 0
127.0.0.1:6379> get mykey
"redis"
# ===================================================
# mset   Mset 命令用于同时设置一个或多个 key-value 对。
# mget   Mget 命令返回所有(一个或多个)给定 key 的值。
#      如果给定的 key 里面，有某个 key 不存在，那么这个 key 返回特殊值 nil 。
# msetnx  当所有 key 都成功设置，返回 1 。
#      如果所有给定 key 都设置失败(至少有一个 key 已经存在)，那么返回 0 。原子操
作
# ===================================================
127.0.0.1:6379> mset k10 v10 k11 v11 k12 v12
OK
127.0.0.1:6379> keys *
1) "k12"
2) "k11"
3) "k10"
127.0.0.1:6379> mget k10 k11 k12 k13
1) "v10"
2) "v11"
3) "v12"
4) (nil)
127.0.0.1:6379> msetnx k10 v10 k15 v15 # 原子性操作！
(integer) 0
127.0.0.1:6379> get key15
(nil)
# 传统对象缓存
set user:1 value(json数据)
# 可以用来缓存对象
mset user:1:name zhangsan user:1:age 2
mget user:1:name user:1:age
# ===================================================
# getset（先get再set）
# ===================================================
127.0.0.1:6379> getset db mongodb  # 没有旧值，返回 nil
(nil)
127.0.0.1:6379> get db
"mongodb"
127.0.0.1:6379> getset db redis   # 返回旧值 mongodb
"mongodb"
127.0.0.1:6379> get db
"redis"
```

String数据结构是简单的key-value类型，value其实不仅可以是String，也可以是数字。
常规key-value缓存应用：
常规计数：微博数，粉丝数等

### 3.2 List(集合类型)

在Redis中，List类型是按照插入顺序排序的字符串链表。和数据结构中的普通链表一样，我们可以在其头部(left)和尾部(right)添加新的元素。在插入时，如果该键并不存在，Redis将为该键创建一个新的链表。与此相反，如果链表中所有的元素均被移除，那么该键也将会被从数据库中删除。List中可以包含的最大元素数量是4294967295。从元素插入和删除的效率视角来看，如果我们是在链表的两头插入或删除元素，这将会是非常高效的操作，即使链表中已经存储了百万条记录，该操作也可以在常量时间内完成。然而需要说明的是，如果元素插入或删除操作是作用于链表中间，那将会是非常低效的。

**常用命令解析**

```shell
# ===================================================
# Lpush：将一个或多个值插入到列表头部。（左）
# rpush：将一个或多个值插入到列表尾部。（右）
# lrange：返回列表中指定区间内的元素，区间以偏移量 START 和 END 指定。
# 其中 0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。
# 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此
类推。
# ===================================================
127.0.0.1:6379> LPUSH list "one"
(integer) 1
127.0.0.1:6379> LPUSH list "two"
(integer) 2
127.0.0.1:6379> RPUSH list "right"
(integer) 3
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "one"
3) "right"
127.0.0.1:6379> Lrange list 0 1
1) "two"
2) "one"
# ===================================================
# lpop 命令用于移除并返回列表的第一个元素。当列表 key 不存在时，返回 nil 。
# rpop 移除列表的最后一个元素，返回值为移除的元素。
# ===================================================
127.0.0.1:6379> Lpop list
"two"
127.0.0.1:6379> Rpop list
"right"
127.0.0.1:6379> Lrange list 0 -1
1) "one"
# ===================================================
# Lindex，按照索引下标获得元素（-1代表最后一个，0代表是第一个）
# ===================================================
127.0.0.1:6379> Lindex list 1
(nil)
127.0.0.1:6379> Lindex list 0
"one"
127.0.0.1:6379> Lindex list -1
"one"
# ===================================================
# llen 用于返回列表的长度。
# ===================================================
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> Lpush list "one"
(integer) 1
127.0.0.1:6379> Lpush list "two"
(integer) 2
127.0.0.1:6379> Lpush list "three"
(integer) 3
127.0.0.1:6379> Llen list  # 返回列表的长度
(integer) 3
# ===================================================
# lrem key 根据参数 COUNT 的值，移除列表中与参数 VALUE 相等的元素。
# ===================================================
127.0.0.1:6379> lrem list 1 "two"
(integer) 1
127.0.0.1:6379> Lrange list 0 -1
1) "three"
2) "one"
# ===================================================
# Ltrim key 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区
间之内的元素都将被删除。
# ===================================================
127.0.0.1:6379> RPUSH mylist "hello"
(integer) 1
127.0.0.1:6379> RPUSH mylist "hello"
(integer) 2
127.0.0.1:6379> RPUSH mylist "hello2"
(integer) 3
127.0.0.1:6379> RPUSH mylist "hello3"
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2
OK
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "hello2"
# ===================================================
# rpoplpush 移除列表的最后一个元素，并将该元素添加到另一个列表并返回。
# ===================================================
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "foo"
(integer) 2
127.0.0.1:6379> rpush mylist "bar"
(integer) 3
127.0.0.1:6379> rpoplpush mylist myotherlist
"bar"
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "foo"
127.0.0.1:6379> lrange myotherlist 0 -1
1) "bar"
# ===================================================
# lset key index value 将列表 key 下标为 index 的元素的值设置为 value 。
# ===================================================
127.0.0.1:6379> exists list  # 对空列表(key 不存在)进行 LSET
(integer) 0
127.0.0.1:6379> lset list 0 item # 报错
(error) ERR no such key
127.0.0.1:6379> lpush list "value1" # 对非空列表进行 LSET
(integer) 1
127.0.0.1:6379> lrange list 0 0
1) "value1"
127.0.0.1:6379> lset list 0 "new"  # 更新值
OK
127.0.0.1:6379> lrange list 0 0
1) "new"
127.0.0.1:6379> lset list 1 "new"  # index 超出范围报错
(error) ERR index out of range
# ===================================================
# linsert key before/after pivot value 用于在列表的元素前或者后插入元素。
# 将值 value 插入到列表 key 当中，位于值 pivot 之前或之后。
# ===================================================
redis> RPUSH mylist "Hello"
(integer) 1
redis> RPUSH mylist "World"
(integer) 2
redis> LINSERT mylist BEFORE "World" "There"
(integer) 3
redis> LRANGE mylist 0 -1
1) "Hello"
2) "There"
3) "World"
```

**List总结**

- 它是一个字符串链表，left，right 都可以插入添加
- 如果键不存在，创建新的链表
- 如果键已存在，新增内容
- 如果值全移除，对应的键也就消失了
- 链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了

list就是链表，略有数据结构知识的人都应该能理解其结构。使用Lists结构，我们可以轻松地实现最新消息排行等功能。List的另一个应用就是消息队列，可以利用List的PUSH操作，将任务存在List中，然后工作线程再用POP操作将任务取出进行执行。Redis还提供了操作List中某一段的api，你可以直接查询，删除List中某一段的元素。
Redis的list是每个子元素都是String类型的双向链表，可以通过push和pop操作从列表的头部或者尾部添加或者删除元素，这样List即可以作为栈，也可以作为队列。

### 3.3 Set(集合)

Redis中的Hash类型可以看成具有String Key和String Value的map容器。所以该类型非常适合于存储值对象的信息。如用户信息：Username、Password和Age等。每一个Hash可以存储4294967295个键值对。

**常用命令解析**

```shell
# ===================================================
# sadd 将一个或多个成员元素加入到集合中，不能重复
# smembers 返回集合中的所有的成员。
# sismember 命令判断成员元素是否是集合的成员。
# ===================================================
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "zhuangkang"
(integer) 1
127.0.0.1:6379> sadd myset "zhuangkang"
(integer) 0
127.0.0.1:6379> SMEMBERS myset
1) "zhuangkang"
2) "hello"
127.0.0.1:6379> SISMEMBER myset "hello"
(integer) 1
127.0.0.1:6379> SISMEMBER myset "world"
(integer) 0
# ===================================================
# scard，获取集合里面的元素个数
# ===================================================
127.0.0.1:6379> scard myset
(integer) 2
# ===================================================
# srem key value 用于移除集合中的一个或多个成员元素
# ===================================================
127.0.0.1:6379> srem myset "zhuangkang"
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "hello"
# ===================================================
# srandmember key 命令用于返回集合中的一个随机元素。
# ===================================================
127.0.0.1:6379> SMEMBERS myset
1) "zhuangkang"
2) "world"
3) "hello"
127.0.0.1:6379> SRANDMEMBER myset
"hello"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "world"
2) "zhuangkang"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "zhuangkang"
2) "hello"
# ===================================================
# spop key 用于移除集合中的指定 key 的一个或多个随机元素
# ===================================================
127.0.0.1:6379> SMEMBERS myset
1) "kuangshen"
2) "world"
3) "hello"
127.0.0.1:6379> spop myset
"world"
127.0.0.1:6379> spop myset
"kuangshen"
127.0.0.1:6379> spop myset
"hello"
# ===================================================
# smove SOURCE DESTINATION MEMBER
# 将指定成员 member 元素从 source 集合移动到 destination 集合。
# ===================================================
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "world"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset2 "set2"
(integer) 1
127.0.0.1:6379> smove myset myset2 "kuangshen"
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "hello"
127.0.0.1:6379> SMEMBERS myset2
1) "kuangshen"
2) "set2"
# ===================================================
- 数字集合类
 - 差集： sdiff
 - 交集： sinter
 - 并集： sunion
# ===================================================
127.0.0.1:6379> sadd key1 "a"
(integer) 1
127.0.0.1:6379> sadd key1 "b"
(integer) 1
127.0.0.1:6379> sadd key1 "c"
(integer) 1
127.0.0.1:6379> sadd key2 "c"
(integer) 1
127.0.0.1:6379> sadd key2 "d"
(integer) 1
127.0.0.1:6379> sadd key2 "e"
(integer) 1
127.0.0.1:6379> SDIFF key1 key2 # 差集
1) "a"
2) "b"
127.0.0.1:6379> SINTER key1 key2 # 交集
1) "c"
127.0.0.1:6379> SUNION key1 key2 # 并集
1) "a"
2) "b"
3) "c"
4) "e"
5) "d"
```

在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis还为集合提供了求交集、并集、差集等操作，可以非常方便的实现如共同关注、共同喜好、二度好友等功能，对上面的所有集合操作，你还可以使用不同的命令选择将结果返回给客户端还是存集到一个新的集合中。

### 3.4 Hash(集合)

**常用命令解析**

```shell
# ===================================================
# hset、hget 命令用于为哈希表中的字段赋值 。
# hmset、hmget 同时将多个field-value对设置到哈希表中。会覆盖哈希表中已存在的字段。
# hgetall 用于返回哈希表中，所有的字段和值。
# hdel  用于删除哈希表 key 中的一个或多个指定字段
# ===================================================
127.0.0.1:6379> hset myhash field1 "kuangshen"
(integer) 1
127.0.0.1:6379> hget myhash field1
"kuangshen"
127.0.0.1:6379> HMSET myhash field1 "Hello" field2 "World"
OK
127.0.0.1:6379> HGET myhash field1
"Hello"
127.0.0.1:6379> HGET myhash field2
"World"
127.0.0.1:6379> hgetall myhash
1) "field1"
2) "Hello"
3) "field2"
4) "World"
127.0.0.1:6379> HDEL myhash field1
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "World"
# ===================================================
# hlen 获取哈希表中字段的数量。
# ===================================================
127.0.0.1:6379> hlen myhash
(integer) 1
127.0.0.1:6379> HMSET myhash field1 "Hello" field2 "World"
OK
127.0.0.1:6379> hlen myhash
(integer) 2
# ===================================================
# hexists 查看哈希表的指定字段是否存在。
# ===================================================
127.0.0.1:6379> hexists myhash field1
(integer) 1
127.0.0.1:6379> hexists myhash field3
(integer) 0
# ===================================================
# hkeys 获取哈希表中的所有域（field）。
# hvals 返回哈希表所有域(field)的值。
# ===================================================
127.0.0.1:6379> HKEYS myhash
1) "field2"
2) "field1"
127.0.0.1:6379> HVALS myhash
1) "World"
2) "Hello"
# ===================================================
# hincrby 为哈希表中的字段值加上指定增量值。
# ===================================================
127.0.0.1:6379> hset myhash field 5
(integer) 1
127.0.0.1:6379> HINCRBY myhash field 1
(integer) 6
127.0.0.1:6379> HINCRBY myhash field -1
(integer) 5
127.0.0.1:6379> HINCRBY myhash field -10
(integer) -5
# ===================================================
# hsetnx 为哈希表中不存在的的字段赋值 。
# ===================================================
127.0.0.1:6379> HSETNX myhash field1 "hello"
(integer) 1  # 设置成功，返回 1 。
127.0.0.1:6379> HSETNX myhash field1 "world"
(integer) 0  # 如果给定字段已经存在，返回 0 。
127.0.0.1:6379> HGET myhash field1
"hello"
```

Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。
存储部分变更的数据，如用户信息等。

### 3.5 Zset(集合)

在Redis中，我们可以将Set类型看作为没有排序的字符串集合。Set可包含的最大元素数量是4294967295。Set类型在功能上还存在着一个非常重要的特性，即在服务器端完成多个Sets之间的聚合计算操作，如unions、intersections和differences。由于这些操作均在服务端完成，因此效率极高，而且也节省了大量的网络IO开销。

**常用命令解析**

**在set基础上，加一个score值。之前set是k1 v1 v2 v3，现在zset是 k1 score1 v1 score2 v2**

```shell
# ===================================================
# zadd  将一个或多个成员元素及其分数值加入到有序集当中。
# zrange 返回有序集中，指定区间内的成员
# ===================================================
127.0.0.1:6379> zadd myset 1 "one"
(integer) 1
127.0.0.1:6379> zadd myset 2 "two" 3 "three"
(integer) 2
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
# ===================================================
# zrangebyscore 返回有序集合中指定分数区间的成员列表。有序集成员按分数值递增(从小到大)
次序排列。
# ===================================================
127.0.0.1:6379> zadd salary 2500 xiaoming
(integer) 1
127.0.0.1:6379> zadd salary 5000 xiaohong
(integer) 1
127.0.0.1:6379> zadd salary 500 zhuangkang
(integer) 1
# Inf无穷大量+∞,同样地,-∞可以表示为-Inf。
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf # 显示整个有序集
1) "zhuangkang"
2) "xiaoming"
3) "xiaohong"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores # 递增排列
1) "zhuangkang"
2) "500"
3) "xiaoming"
4) "2500"
5) "xiaohong"
6) "5000"
127.0.0.1:6379> ZREVRANGE salary 0 -1 WITHSCORES  # 递减排列
1) "zhuangkang"
2) "5000"
3) "xiaoming"
4) "2500"
5) "zhuangkang"
6) "500"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 2500 WITHSCORES # 显示工资 <=2500
的所有成员
1) "zhuangkang"
2) "500"
3) "xiaoming"
4) "2500"
# ===================================================
# zrem 移除有序集中的一个或多个成员
# ===================================================
127.0.0.1:6379> ZRANGE salary 0 -1
1) "zhuangkang"
2) "xiaoming"
3) "xiaohong"
127.0.0.1:6379> zrem salary kuangshen
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1
1) "xiaoming"
2) "xiaohong"
# ===================================================
# zcard  命令用于计算集合中元素的数量。
# ===================================================
127.0.0.1:6379> zcard salary
(integer) 2
OK
# ===================================================
# zcount 计算有序集合中指定分数区间的成员数量。
# ===================================================
127.0.0.1:6379> zadd myset 1 "hello"
(integer) 1
127.0.0.1:6379> zadd myset 2 "world" 3 "zhuangkang"
(integer) 2
127.0.0.1:6379> ZCOUNT myset 1 3
(integer) 3
127.0.0.1:6379> ZCOUNT myset 1 2
(integer) 2
# ===================================================
# zrank 返回有序集中指定成员的排名。其中有序集成员按分数值递增(从小到大)顺序排列。
# ===================================================
127.0.0.1:6379> zadd salary 2500 xiaoming
(integer) 1
127.0.0.1:6379> zadd salary 5000 xiaohong
(integer) 1
127.0.0.1:6379> zadd salary 500 zhuangkang
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1 WITHSCORES  # 显示所有成员及其 score 值
1) "zhuangkang"
2) "500"
3) "xiaoming"
4) "2500"
5) "xiaohong"
6) "5000"
127.0.0.1:6379> zrank salary zhuangkang  # 显示 zhuangkang 的薪水排名，最少
(integer) 0
127.0.0.1:6379> zrank salary xiaohong  # 显示 xiaohong 的薪水排名，第三
(integer) 2
# ===================================================
# zrevrank 返回有序集中成员的排名。其中有序集成员按分数值递减(从大到小)排序。
# ===================================================
127.0.0.1:6379> ZREVRANK salary kuangshen # 狂神第三
(integer) 2
127.0.0.1:6379> ZREVRANK salary xiaohong  # 小红第一
(integer) 0
```

和set相比，sorted set增加了一个权重参数score，使得集合中的元素能够按score进行有序排列，比如
一个存储全班同学成绩的sorted set，其集合value可以是同学的学号，而score就可以是其考试得分，
这样在数据插入集合的时候，就已经进行了天然的排序。可以用sorted set来做带权重的队列，比如普通消息的score为1，重要消息的score为2，然后工作线程可以选择按score的倒序来获取工作任务。让重要的任务优先执行。



## 4.特殊数据类型

### 4.1 GEO地理位置

> 简介

Redis 的 GEO 特性在 Redis 3.2 版本中推出， 这个功能可以将用户给定的地理位置信息储存起来， 并对这些信息进行操作。来实现诸如附近位置、摇一摇这类依赖于地理位置信息的功能。geo的数据类型为**zset**。
GEO 的数据结构总共有六个常用命令：**geoadd、geopos、geodist、georadius、georadiusbymember、gethash**

官方文档：https://www.redis.net.cn/order/3685.html

> geoadd

**解析**：

```bash
# 语法
geoadd key longitude latitude member ...
# 将给定的空间元素(纬度、经度、名字)添加到指定的键里面。
# 这些数据会以有序集he的形式被储存在键里面，从而使得georadius和georadiusbymember这样的
命令可以在之后通过位置查询取得这些元素。
# geoadd命令以标准的x,y格式接受参数,所以用户必须先输入经度,然后再输入纬度。
# geoadd能够记录的坐标是有限的:非常接近两极的区域无法被索引。
# 有效的经度介于-180-180度之间，有效的纬度介于-85.05112878 度至 85.05112878 度之间。，
当用户尝试输入一个超出范围的经度或者纬度时,geoadd命令将返回一个错误。
```

**测试模拟真实经纬度(数据来源：百度)**

```bash
127.0.0.1:6379> geoadd china:city 116.23 40.22 北京
(integer) 1
127.0.0.1:6379> geoadd china:city 121.48 31.40 上海 113.88 22.55 深圳 120.21
30.20 杭州
(integer) 3
127.0.0.1:6379> geoadd china:city 106.54 29.40 重庆 108.93 34.23 西安 114.02
30.58 武汉
(integer) 3
```

> geopos

**解析：**

```bash
# 语法
geopos key member [member...]
#从key里返回所有给定位置元素的位置（经度和纬度）
```

**测试：**

```bash
127.0.0.1:6379> geopos china:city 北京
1) 1) "116.23000055551528931"
 2) "40.2200010338739844"
127.0.0.1:6379> geopos china:city 上海 重庆
1) 1) "121.48000091314315796"
 2) "31.40000025319353938"
2) 1) "106.54000014066696167"
 2) "29.39999880018641676"
127.0.0.1:6379> geopos china:city 新疆
1) (nil)
```



> geodist

**解析：**

```bash
# 语法
geodist key member1 member2 [unit]
# 返回两个给定位置之间的距离，如果两个位置之间的其中一个不存在,那么命令返回空值。
# 指定单位的参数unit必须是以下单位的其中一个：
#  m表示单位为米
#  km表示单位为千米
#  mi表示单位为英里
#  ft表示单位为英尺
#  如果用户没有显式地指定单位参数,那么geodist默认使用米作为单位。
#geodist命令在计算距离时会假设地球为完美的球形,在极限情况下,这一假设最大会造成0.5%的误
差。
```

**测试：**

```bash
127.0.0.1:6379> geodist china:city 北京 上海
"1088785.4302"
127.0.0.1:6379> geodist china:city 北京 上海 km
"1088.7854"
127.0.0.1:6379> geodist china:city 重庆 北京 km
"1491.6716"
```



> georadius

**解析：**

```bash
# 语法
georadius key longitude latitude radius m|km|ft|mi [withcoord][withdist][withhash][asc|desc][count count]
# 以给定的经纬度为中心， 找出某一半径内的元素
```

**测试：重新连接 redis-cli，增加参数 --raw ，可以强制输出中文，不然会乱码**

```bash
[root@zhuangkang bin]# redis-cli --raw -p 6379
# 在 china:city 中寻找坐标 100 30 半径为 1000km 的城市
127.0.0.1:6379> georadius china:city 100 30 1000 km
重庆
西安
# withdist 返回位置名称和中心距离
127.0.0.1:6379> georadius china:city 100 30 1000 km withdist
重庆
635.2850
西安
963.3171
# withcoord 返回位置名称和经纬度
127.0.0.1:6379> georadius china:city 100 30 1000 km withcoord
重庆
106.54000014066696167
29.39999880018641676
西安
108.92999857664108276
34.23000121926852302
# withdist withcoord 返回位置名称 距离 和经纬度 count 限定寻找个数
127.0.0.1:6379> georadius china:city 100 30 1000 km withcoord withdist count
1
重庆
635.2850
106.54000014066696167
29.39999880018641676
127.0.0.1:6379> georadius china:city 100 30 1000 km withcoord withdist count
2
重庆
635.2850
106.54000014066696167
29.39999880018641676
西安
963.3171
108.92999857664108276
34.23000121926852302
```



> georadiusbymember

**解析：**

```bash
# 语法
georadiusbymember key member radius m|km|ft|mi [withcoord][withdist][withhash][asc|desc][count count]
# 找出位于指定范围内的元素，中心点是由给定的位置元素决定
```

**测试：**

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER china:city 北京 1000 km
北京
西安
127.0.0.1:6379> GEORADIUSBYMEMBER china:city 上海 400 km
杭州
上海
```



> geohash

**解析：**

```bash
# 语法
geohash key member [member...]
# Redis使用geohash将二维经纬度转换为一维字符串，字符串越长表示位置更精确,两个字符串越相似表示距离越近。
```



### 4.2 HyperLogLog 

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。
Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积
非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。
在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基
数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。
HyperLogLog则是一种算法，它提供了不精确的去重计数方案。
举个栗子：假如我要统计网页的UV（浏览用户数量，一天内同一个用户多次访问只能算一次），传统的
解决方案是使用Set来保存用户id，然后统计Set中的元素数量来获取页面UV。但这种方案只能承载少量
用户，一旦用户数量大起来就需要消耗大量的空间来存储用户id。我的目的是统计用户数量而不是保存
用户，这简直是个吃力不讨好的方案！而使用Redis的HyperLogLog最多需要12k就可以统计大量的用户
数，尽管它大概有0.81%的错误率，但对于统计UV这种不需要很精确的数据是可以忽略不计的。

> **什么是基数**

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。
基数估计就是在误差可接受的范围内，快速计算基数。

**常用命令：**

```bash
[PFADD key element [element ...]  # 添加指定元素到 HyperLogLog 中。
[PFCOUNT key [key ...] # 返回给定 HyperLogLog 的基数估算值。
[PFMERGE destkey sourcekey[sourcekey ...] # 将多个 HyperLogLog 合并为一个 HyperLogLog，并集计算
```

**测试**

```bash
127.0.0.1:6379> PFADD mykey a b c d e f g h i j
1
127.0.0.1:6379> PFCOUNT mykey
10
127.0.0.1:6379> PFADD mykey2 i j z x c v b n m
1
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2
OK
127.0.0.1:6379> PFCOUNT mykey3
15
```

## 5.Redis配置文件解析

### 5.1 熟悉基本配置

![image-20210312202909307](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312202909307.png)

1、配置大小单位，开头定义了一些基本的度量单位，只支持bytes，不支持bit
2、对 大小写 不敏感

> INCLUDE

![image-20210312202936947](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312202936947.png)

> NETWORK网络配置

```bash
bind 127.0.0.1    # 绑定的ip
protected-mode yes  # 保护模式
port 6379      # 默认端口
```

> GENERAL

```bash
daemonize yes  # 默认情况下，Redis不作为守护进程运行。需要开启的话，改为 yes
supervised no  # 可通过upstart和systemd管理Redis守护进程
pidfile /var/run/redis_6379.pid  # 以后台进程方式运行redis，则需要指定pid 文件
loglevel notice # 日志级别。可选项有：
# debug（记录大量日志信息，适用于开发、测试阶段）； 
        # verbose（较多日志信息）； 
        # notice（适量日志信息，使用于生产环境）；
        # warning（仅有部分重要、关键信息才会被记录）。
logfile ""    # 日志文件的位置，当指定为空字符串时，为标准输出
databases 16   # 设置数据库的数目。默认的数据库是DB 0
always-show-logo yes  # 是否总是显示logo
```

> SNAPSHOPTING 快照

```bash
# 900秒（15分钟）内至少1个key值改变（则进行数据库保存--持久化）
save 900 1
# 300秒（5分钟）内至少10个key值改变（则进行数据库保存--持久化）
save 300 10
# 60秒（1分钟）内至少10000个key值改变（则进行数据库保存--持久化）
save 60 10000
stop-writes-on-bgsave-error yes  # 持久化出现错误后，是否依然进行继续进行工作
rdbcompression yes  # 使用压缩rdb文件 yes：压缩，但是需要一些cpu的消耗。no：不压
缩，需要更多的磁盘空间
rdbchecksum yes    # 是否校验rdb文件，更有利于文件的容错性，但是在保存rdb文件的时
候，会有大概10%的性能损耗
dbfilename dump.rdb  # dbfilenamerdb文件名称
dir ./   # dir 数据目录，数据库的写入会在这个目录。rdb、aof文件也会写在这个目录
```

> SECURITY安全

```bash
# 启动redis
# 连接客户端
# 获得和设置密码
config get requirepass
config set requirepass "123456"
#测试ping，发现需要验证
127.0.0.1:6379> ping
NOAUTH Authentication required.
# 验证
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> ping
PONG
```

> 限制

```bash
maxclients 10000  # 设置能连上redis的最大客户端连接数量
maxmemory <bytes>  # redis配置的最大内存容量
maxmemory-policy noeviction  # maxmemory-policy 内存达到上限的处理策略
    #volatile-lru：利用LRU算法移除设置过过期时间的key。
    #volatile-random：随机移除设置过过期时间的key。
    #volatile-ttl：移除即将过期的key，根据最近过期时间来删除（辅以TTL）
    #allkeys-lru：利用LRU算法移除任何key。
    #allkeys-random：随机移除任何key。
    #noeviction：不移除任何key，只是返回一个写错误。
```

> append only模式

```bash
appendonly no  # 是否以append only模式作为持久化方式，默认使用的是rdb方式持久化，这种
方式在许多应用中已经足够用了
appendfilename "appendonly.aof"  # appendfilename AOF 文件名称
appendfsync everysec  # appendfsync aof持久化策略的配置
  # no表示不执行fsync，由操作系统保证数据同步到磁盘，速度最快。
  # always表示每次写入都执行fsync，以保证数据同步到磁盘。
  # everysec表示每秒执行一次fsync，可能会导致丢失这1s数据。
```



### 5.2 常见配置介绍

```bash
#1、Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
	daemonize no 
#2、当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
	pidfile /var/run/redis.pid	
#3、指定Redis监听端口，默认端口为6379
	port 6379
#4、绑定的主机地址
	bind 127.0.0.1
#5、当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
	timeout300
#6、指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
	loglevel verbose
#7、日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会给/dev/null
	logfile stdout
#8、设置数据库的数量，默认数据库为0，可以使用SELECT 命令在连接上指定数据库id
	databases 16
#9、指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
	save
	Redis默认配置文件中提供了三个条件：
	save 900 1
	save 300 10
	save 60 10000
	分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改
#10、指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
	rdbcompression yes
#11、指定本地数据库文件名，默认值为dump.rdb
	dbfilename dump.rdb
#12、指定本地数据库存放目录
	dir ./
#13、设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
	slaveof
#14、当master服务设置了密码保护时，slav服务连接master的密码
	masterauth
#15、设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH 命令提供密码，默认关闭
	requirepass foobared
#16、设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。
	maxclients 128
#17、指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
	maxmemory
#18、指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。
	appendonly no
#19、指定更新日志文件名，默认为appendonly.aof
	appendfilename appendonly.aof	
#20、指定更新日志条件，共有3个可选值：
	no：表示等操作系统进行数据缓存同步到磁盘（快）
	always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
	everysec：表示每秒同步一次（折衷，默认值）
	appendfsync everysec
#21、指定是否启用虚拟内存机制，默认值为no
	vm-enabled no
#22、虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
	vm-swap-file /tmp/redis.swap
#23、将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0
	vm-max-memory 0
#24、Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的
	vm-page-size 32
#25、设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，在磁盘上每8个pages将消耗1byte的内存。
	vm-pages 134217728
#26、设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4
	vm-max-threads 4
#27、设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
	glueoutputbuf yes
#28、指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法	
	hash-max-zipmap-entries 64
	hash-max-zipmap-value 512
#29、指定是否激活重置哈希，默认为开启
	activerehashing yes
#30、指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
	include /path/to/local.conf
```



## 6.删除策略

### 6.1 Redis中的数据特征

-  Redis是一种内存级数据库，所有数据均存放在内存中，内存中的数据可以通过TTL指令获取其状态

  -  XX ：具有时效性的数据
  - -1 ：永久有效的数据
  -  -2 ：已经过期的数据 或 被删除的数据 或 未定义的数据

  

### 6.2 数据删除策略

![image-20210312210918949](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312210918949.png) 

**目标：**在内存占用与CPU占用之间寻找一种平衡，顾此失彼都会造成整体redis性能的下降，甚至引发服务器宕机或
内存泄露

#### 6.2.1 定时删除

- 创建一个定时器，当KEY设置过期时间，且过期时间到达时，有定时器立即执行对键的删除
- **优点：节约内存，快速释放不必要的内存占用**

- 缺点：CPU压力大，占用CPU，影响Redis服务器响应时间
- 总结：用处理器性能换取存储空间 （拿时间换空间）

![image-20210312211240858](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312211240858.png)

#### 6.2.2 惰性删除策略

- 数据到达过期时间，不做处理。等下次访问该数据时
  - 如果未过期，返回数据
  -  发现已过期，删除，返回不存在

- **优点：**CPU性能，发现必须删除的时候才删除
- 缺点：内存压力大，出现长期占用内存的数据
- 总结：用存储空间换取处理器性能expireIfNeeded()(拿时间换空间）

![image-20210312211500020](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312211500020.png)

#### 6.2.3 定期删除策略

- 周期性轮询redis库中的时效性数据，采用随机抽取的策略，利用过期数据占比的方式控制删除频度
- 特点1：CPU性能占用设置有峰值，检测频度可自定义设置
- 特点2：内存压力不是很大，长期占用内存的冷数据会被持续清理
- 总结：周期性抽查存储空间（随机抽查，重点抽查）

![image-20210312211646091](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312211646091.png)

## 7.Redis的持久化

Redis 是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中
的数据库状态也会消失。所以 Redis 提供了持久化功能！

持久化过程保存什么？

> **将当前数据状态进行保存，快照形式，存储数据结果，存储格式简单，关注点在数据**
>
>  **将数据的操作过程进行保存，日志形式，存储操作过程，存储格式复杂，关注点在数据的操作过程**

### 7.1.1RDB

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快
照文件直接读到内存里。

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程
都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。
这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那
RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。

**RDB启动方式 —— save指令工作原理**

```bash
#手动执行一次保存操作
	save

#说明：设置本地数据库文件名，默认值为 dump.rdb
#经验：通常设置为dump-端口号.rdb
	dbfilename dump.rdb

#说明：设置存储.rdb文件的路径
#经验：通常设置成存储空间较大的目录中，目录名称data
	dir
	
#说明：设置存储至本地数据库时是否压缩数据，默认为 yes，采用 LZF 压缩
#经验：通常默认为开启状态，如果设置为no，可以节省 CPU 运行时间，但会使存储的文件变大（巨大）
	rdbcompression yes
	
#说明：设置是否进行RDB文件格式校验，该校验过程在写文件和读文件过程均进行
#经验：通常默认为开启状态，如果设置为no，可以节约读写性过程约10%时间消耗，但是存储一定的数据损坏风险	
	 rdbchecksum yes
```

![image-20210312212649363](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312212649363.png)



![image-20210312212709577](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312212709577.png)

**RDB启动方式 —— bgsave指令**

```bash
#手动启动后台保存操作，但不是立即执行
	bgsave
	
#说明：后台存储过程中如果出现错误现象，是否停止保存操作
#经验：通常默认为开启状态
    dbfilename dump.rdb
	dir
 	rdbcompression yes
 	rdbchecksum yes
 	stop-writes-on-bgsave-error yes
	
```



![image-20210312212940908](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312212940908.png)

**RDB启动方式 ——save配置**

```bash
#满足限定时间范围内key的变化数量达到指定数量即进行持久化
参数
	second：监控时间范围
	changes：监控key的变化量
	$ save second changes
```

![image-20210312213134359](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312213134359.png)



![image-20210312213202725](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312213202725.png)

**RDB优点：**

- RDB是一个紧凑压缩的二进制文件，存储效率较高
-  RDB内部存储的是redis在某个时间点的数据快照，非常适合用于数据备份，全量复制等场景
-  RDB恢复数据的速度要比AOF快很多
-  应用：服务器中每X小时执行bgsave备份，并将RDB文件拷贝到远程机器中，用于灾难恢复。

**RDB缺点：**

-  RDB方式无论是执行指令还是利用配置，无法做到实时持久化，具有较大的可能性丢失数据
- bgsave指令每次运行要执行fork操作创建子进程，要牺牲掉一些性能
-  Redis的众多版本中未进行RDB文件格式的版本统一，有可能出现各版本服务之间数据格式无法兼容现象



### 7.1.2 AOF

**RDB存储的弊端**

- 存储数据较大，效率较低
- 快照思想，每次读写全部数据，数据多时，效率低
- 大数据下的IO性能低
- 基于Fork创建子进程，内存额外消耗
- 宕机带来的数据丢失风险

**解决思路**

- 不全写数据，仅仅记录部分数据
- 降低区分数据是否改变的难度，改变记录数据为记录操作过程
- 对所有操作进行记录，排除丢失数据的风险

AOF概念

-  AOF(append only file)持久化：以独立日志的方式记录每次写命令，重启时再重新执行AOF文件中命令
  达到恢复数据的目的。与RDB相比可以简单描述为改记录数据为记录数据产生的过程

- AOF的主要作用是解决了数据持久化的实时性，目前已经是Redis持久化的主流方式

![image-20210312213813782](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312213813782.png)



**AOF写数据三种策略(appendfsync)**

- always(每次）
  - 每次写入操作均同步到AOF文件中，**数据零误差，性能较低**，不建议使用。
- everysec（每秒）
  - 每秒将缓冲区中的指令同步到AOF文件中，数据准确性较高，性能较高，建议使用，也是默认配置
  - 在系统突然宕机的情况下丢失1秒内的数据
- no(系统控制)
  - 由操作系统控制每次同步到AOF文件的周期，整体过程不可控



**AOF功能开启**

```bash
#是否开启AOF持久化功能，默认为不开启状态
	appendonly yes|no
#AOF写数据策略
	appendfsync always|everysec|no
```

**AOF相关配置**

```bash
#AOF持久化文件名，默认文件名未appendonly.aof，建议配置为appendonly-端口号.aof
	appendfilename filename
#AOF持久化文件保存路径，与RDB持久化文件保持一致即可
	dir
```

**AOF重写**

随着命令不断写入AOF，文件会越来越大，为了解决这个问题，Redis引入了AOF重写机制压缩文件体积。AOF文件重
写是将Redis进程内的数据转化为写命令同步到新AOF文件的过程。简单说就是将对同一个数据的若干个条命令执行结
果转化成最终结果数据对应的指令进行记录

**AOF重写作用**

- 降低磁盘占用量，提高磁盘利用率
-  提高持久化效率，降低持久化写时间，提高IO性能
-  降低数据恢复用时，提高数据恢复效率

**AOF重写方式**

```bash
# 手动重写

 bgrewriteaof
# 自动重写

auto-aof-rewrite-min-size size
auto-aof-rewrite-percentage percentage
```

![image-20210312214538566](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312214538566.png)

![image-20210312214607099](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312214607099.png)

![image-20210312214623014](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312214623014.png)

![image-20210312214648792](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312214648792.png)

![image-20210312214737258](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312214737258.png)

**RDB与AOF的选择之惑**

- **对数据非常敏感，建议使用默认的AOF持久化方案**
  - AOF持久化策略使用everysecond，每秒钟fsync一次。该策略redis仍可以保持很 好的处理性能，当出
    现问题时，最多丢失0-1秒内的数据。
  - 注意：由于AOF文件存储体积较大，且恢复速度较慢

- **数据呈现阶段有效性，建议使用RDB持久化方案**
  -  数据可以良好的做到阶段内无丢失（该阶段是开发者或运维人员手工维护的），且恢复速度较快，阶段
    点数据恢复通常采用RDB方案
  -  注意：利用RDB实现紧凑的数据持久化会使Redis降的很低

- **综合比对**
  - RDB与AOF的选择实际上是在做一种权衡，每种都有利有弊
  -  如不能承受数分钟以内的数据丢失，对业务数据非常敏感，选用AOF
  -  如能承受数分钟以内的数据丢失，且追求大数据集的恢复速度，选用RDB
  - 灾难恢复选用RDB
  - 双保险策略，同时开启 RDB 和 AOF，重启后，Redis优先使用 AOF 来恢复数据，降低丢失数据的量

### 7.1.3 持久化应用场景

![image-20210312215304344](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312215304344.png)

## 8.Redis事务

**Redis事务的概念：**
Redis 事务的本质是一组命令的集合。事务支持一次执行多个命令，一个事务中所有命令都会被序列
化。在事务执行过程，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事
务执行命令序列中。
总结说：redis事务就是一次性、顺序性、排他性的执行一个队列中的一系列命令。
**Redis事务没有隔离级别的概念：**
批量操作在发送 EXEC 命令前被放入队列缓存，并不会被实际执行！
**Redis不保证原子性：**
Redis中，单条命令是原子性执行的，但事务不保证原子性，且没有回滚。事务中任意命令执行失败，其
余的命令仍会被执行。

**Redis事务的三个阶段：**

- 开始事务
- 命令入队
- 执行事务

**Redis事务相关命令**

```bash
watch key1 key2 ...  #监视一或多个key,如果在事务执行之前，被监视的key被其他命令改动，则
事务被打断 （ 类似乐观锁 ）
multi # 标记一个事务块的开始（ queued ）
exec # 执行所有事务块的命令 （ 一旦执行exec后，之前加的监控锁都会被取消掉 ）
discard # 取消事务，放弃事务块中的所有命令
unwatch # 取消watch对所有key的监控
```

正常执行

![image-20210312215653001](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312215653001.png)

**放弃事务**

![image-20210312215711723](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312215711723.png)

**存在错误**

![image-20210312215734036](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312215734036.png)

**出现语法性错误**

![image-20210312215810395](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312215810395.png)

**悲观锁：**
悲观锁(Pessimistic Lock),顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿到这个数据就会block直到它拿到锁。传统的关系型数据库里面就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在操作之前先上锁。
**乐观锁：**
乐观锁(Optimistic Lock),顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁。但是在更新的时候会判断一下再此期间别人有没有去更新这个数据，可以使用版本号等机制，乐观锁适用于多读的应用类型，这样可以提高吞吐量，乐观锁策略：提交版本必须大于记录当前版本才能执行更新。

- 1、初始化信用卡可用余额和欠额

```bash
127.0.0.1:6379> set balance 100
OK
127.0.0.1:6379> set debt 0
OK
```

- 2、使用watch检测balance，事务期间balance数据未变动，事务执行成功

```bash
127.0.0.1:6379> watch balance
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> decrby balance 20
QUEUED
127.0.0.1:6379> incrby debt 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
```

- 3、使用watch检测balance，事务期间balance数据变动，事务执行失败！

```bash
# 窗口一
127.0.0.1:6379> watch balance
OK
127.0.0.1:6379> MULTI  # 执行完毕后，执行窗口二代码测试
OK
127.0.0.1:6379> decrby balance 20
QUEUED
127.0.0.1:6379> incrby debt 20
QUEUED
127.0.0.1:6379> exec  # 修改失败！
(nil)
# 窗口二
127.0.0.1:6379> get balance
"80"
127.0.0.1:6379> set balance 200
OK
# 窗口一：出现问题后放弃监视，然后重来！
127.0.0.1:6379> UNWATCH  # 放弃监视
OK
127.0.0.1:6379> watch balance
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> decrby balance 20
QUEUED
127.0.0.1:6379> incrby debt 20
QUEUED
127.0.0.1:6379> exec  # 成功！
1) (integer) 180
2) (integer) 40
```

**说明：**
一但执行 EXEC 开启事务的执行后，无论事务使用执行成功， WARCH 对变量的监控都将被取消。故当事务执行失败后，需重新执行WATCH命令对变量进行监控，并开启新的事务进行操作。
**小结**
watch指令类似于乐观锁，在事务提交时，如果watch监控的多个KEY中任何KEY的值已经被其他客户端
更改，则使用EXEC执行事务时，事务队列将不会被执行，同时返回Nullmulti-bulk应答以通知调用者事务执行失败。



## 9.Redis发布订阅

**Redis 发布订阅(pub/sub)是一种消息通信模式**：发送者(pub)发送消息，订阅者(sub)接收消息。
Redis 客户端可以订阅任意数量的频道。

![image-20210312220231575](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312220231575.png)

![image-20210312220244949](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312220244949.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端

![image-20210312220305354](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312220305354.png)

**常用命令**

![image-20210312220332321](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210312220332321.png)

以下实例演示了发布订阅是如何工作的。在我们实例中我们创建了订阅频道名为 redisChat:

```bash
redis 127.0.0.1:6379> SUBSCRIBE redisChat
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "redisChat"
3) (integer) 1
```

现在，我们先重新开启个 redis 客户端，然后在同一个频道 redisChat 发布两次消息，订阅者就能接收到消息。

```bash
redis 127.0.0.1:6379> PUBLISH redisChat "Hello,Redis"
(integer) 1
redis 127.0.0.1:6379> PUBLISH redisChat "Hello，123"
(integer) 1
# 订阅者的客户端会显示如下消息
1) "message"
2) "redisChat"
3) "Hello,Redis"
1) "message"
2) "redisChat"
3) "Hello，123"
```

**原理解析**

Redis是使用C实现的，通过分析 Redis 源码里的 pubsub.c 文件，了解发布和订阅机制的底层实现，籍
此加深对 Redis 的理解。
Redis 通过 PUBLISH 、SUBSCRIBE 和 PSUBSCRIBE 等命令实现发布和订阅功能。

- 通过 SUBSCRIBE 命令订阅某频道后，redis-server 里维护了一个字典，字典的键就是一个个 channel
  ，而字典的值则是一个链表，链表中保存了所有订阅这个 channel 的客户端。SUBSCRIBE 命令的关
  键，就是将客户端添加到给定 channel 的订阅链表中。

- 通过 PUBLISH 命令向订阅者发送消息，redis-server 会使用给定的频道作为键，在它所维护的 channel
  字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。
  Pub/Sub 从字面上理解就是发布（Publish）与订阅（Subscribe），在Redis中，你可以设定对某一个
  key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应
  的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能。

**使用场景**

Pub/Sub构建实时消息系统
Redis的Pub/Sub系统可以构建实时的消息系统
比如很多用Pub/Sub构建的实时聊天系统的例子



## 10.Redis主从复制

**概念**
主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower)；数据的复制是单向的，只能由主节点到从节点。

Master以写为主，Slave 以读为主。

默认情况下，每台Redis服务器都是主节点；且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。



主从复制的作用主要包括：
**1、数据冗余：**主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。

2、故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。

**3、负载均衡：**在主从复制的基础上，配合读写分离，可以由主节点提供写服务，

由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；

尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。

**4、高可用基石：**除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是
Redis高可用的基础。



**特征：一个master可以拥有多个slave，一个slave只对应一个master**

**职责：**

- master
  - 写数据
  - 执行写操作时，将出现变化的数据自动同步到slave
  - 读数据

- slave
  - 读数据
  - 写数据（禁止）

![image-20210313203215765](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313203215765.png)

主从复制过程3个阶段

- 建立连接阶段
- 数据同步阶段
- 命令传播阶段

![image-20210313203317194](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313203317194.png)

### **主从复制工作流程**

#### 阶段一:建立连接阶段

 **建立slave到master的连接，使master能够识别slave，并保存slave端口号**

步骤

> 步骤1：设置master的地址和端口，保存master信息
> 步骤2：建立socket连接
> 步骤3：发送ping命令（定时器任务）
> 步骤4：身份验证
> 步骤5：发送slave端口信息

![image-20210313203608886](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313203608886.png)



**主从连接（slave连接master）**

```shell
# 方式一：客户端发送命令
$ slaveof <masterip> <masterport>
# 方式二：启动服务器参数
$ redis-server -slaveof <masterip> <masterport>
#方式三：服务器配置
$ slaveof <masterip> <masterport>
```

**主从断开连接**

```shell
# 客户端发送命令 slave断开连接后，不会删除已有数据，只是不再接受master发送的数据
slaveof no one 

```

**授权访问**

```shell
# master客户端发送命令设置密码
$ requirepass <password>
# master配置文件设置密码
$ config set requirepass <password>
$ config get requirepass
# slave客户端发送命令设置密码
$ auth <password>
# slave配置文件设置密码
$ masterauth <password>
# slave启动服务器设置密码
$ redis-server –a <password>
```

#### 阶段二:数据同步阶段工作流程

- 在slave初次连接master后，复制master中的所有数据到slave

- 将slave的数据库状态更新成master当前的数据库状态

**步骤：**

> 步骤1：请求同步数据
> 步骤2：创建RDB同步数据
> 步骤3：恢复RDB同步数据
> 步骤4：请求部分同步数据
> 步骤5：恢复部分同步数据

![image-20210313204536575](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313204536575.png)



#### 阶段三：命令传播阶段

-  当master数据库状态被修改后，导致主从服务器数据库状态不一致，需要让主从数据同步到一致的状态，同步的动作称为命令传播
-  master将接收到的数据变更命令发送给slave，slave接收命令后执行命令

 **命令传播阶段的部分复制**

- **命令传播阶段出现断网现象**
  - 网络闪断闪连  **忽略**
  - 短时间网络中断   **部分复制**
  - 长时间网络中断   全量复制

- **部分复制的三个核心要素**
  - 服务器的运行id
  - 主服务器的复制积压缓冲区
  - 主从服务器的赋值偏移量



**服务器运行ID（runid）**

服务器运行ID是每一台服务器每次运行的身份识别码，一台服务器多次运行可以生成多个运行id

- 作用：运行id被用于在服务器间进行传输，识别身份如果想两次操作均对同一台服务器进行，必须每次操作携带对应的运行id，用于对方识别

- 实现方式：运行id在每台服务器启动时自动生成的，master在首次连接slave时，会将自己的运行ID发送给slave，slave保存此ID，通过**info Server**命令，可以查看节点的runid





**复制缓冲区**

复制缓冲区，又名复制积压缓冲区，**是一个先进先出（FIFO）的队列，用于存储服务器执行过的命令**，每次传播命令，master都会将传播的命令记录下来，并存储在复制缓冲区

![image-20210313205206305](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313205206305.png)





**主从服务器复制偏移量（offset）**

- **概念：**一个数字，描述复制缓冲区中的指令字节位置

- **分类**：
  - master复制偏移量：记录发送给所有slave的指令字节对应的位置（多个）
  -  slave复制偏移量：记录slave接收master发送过来的指令字节对应的位置（一个）

- **数据来源：**
  - master端：发送一次记录一次
  - slave端：接收一次记录一次

- **作用：**同步信息，比对master与slave的差异，当slave断线后，恢复数据使用

![image-20210313205648993](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313205648993.png)



**心跳机制**
进入命令传播阶段候，master与slave间需要进行信息交换，使用心跳机制进行维护，实现双方连接保持在线

- **master心跳：**
  - 指令：PING
  -  周期：由repl-ping-slave-period决定，默认10秒
  -  作用：判断slave是否在线
  -  查询：INFO replication 获取slave最后一次连接时间间隔，lag项维持在0或1视为正常

- **slave心跳任务**:
  - 指令：REPLCONF ACK {offset}
  -  周期：1秒
  -  作用1：汇报slave自己的复制偏移量，获取最新的数据变更指令
  -  作用2：判断master是否在线

![image-20210313205954713](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313205954713.png)



### 主从复制常见问题

#### 频繁的全量复制

伴随着系统的运行，master的数据量会越来越大，一旦master重启，runid将发生变化，会导致全部slave的全量复制操作

**内部优化调整方案：**

1. master内部创建master_replid变量，使用runid相同的策略生成，长度41位，并发送给所有slave
2. 在master关闭时执行命令 shutdown save，进行RDB持久化,将runid与offset保存到RDB文件中
- **epl-id repl-offset**
- **通过redis-check-rdb命令可以查看该信息**
3. master重启后加载RDB文件，恢复数据
    重启后，将RDB文件中保存的repl-id与repl-offset加载到内存中
  - **master_repl_id = repl master_repl_offset = repl-offset**
  - **通过info命令可以查看该信息**
4. 作用：本机保存上次runid，重启后恢复该值，使所有slave认为还是之前的master 



#### 频繁的网络中断（1）

- **问题现象**：master的CPU占用过高 或 slave频繁断开连接
- **问题原因**
  -  slave每1秒发送REPLCONF ACK命令到master
  - 当slave接到了慢查询时（keys * ，hgetall等），会大量占用CPU性能
  -  master每1秒调用复制定时函数replicationCron()，比对slave发现长时间没有进行响应

- **最终结果**
  - master各种资源（输出缓冲区、带宽、连接等）被严重占用
- **解决方案**
  - 通过设置合理的超时时间，确认是否释放slave （repl-timeout）
    该参数定义了超时时间的阈值（默认60秒），超过该值，释放slave



#### 频繁的网络中断（2）

**问题现象**：slave与master连接断开

- **问题原因**
  - master发送ping指令频度比较低
  - master设定超时时间较短
  - ping网络中存在丢包
- **解决方案：**
  - 提高ping指令的频度（repl-ping-slave-period）

**超时间repl-time的时间至少是ping指令频度的5-10倍，否则slave容易判定超时**



#### 数据不一致

问题现象：多个slave获取相同数据不同步

- **问题原因**
  - 网络信息不同步，发送数据有延迟
- **解决方案**
  - 优化主从间的网络环境
  - 监控从节点延迟（通过offset）判断，如果slave延迟过大，暂时屏蔽程序对slave的数据访问(slave-serve-stale-data  yes|no)



## 11.哨兵模式

哨兵(sentinel) 是一个分布式系统，用于对主从结构中的每台服务器进行监控，当出现故障时通过投票机制选择新的master并将所有slave连接到新的master。

![image-20210313211546132](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313211546132.png)



**哨兵的作用**

- **监控**
  不断的检查master和slave是否正常运行。master存活检测、master与slave运行情况检测

  

- 通知（提醒）当被监控的服务器出现问题时，向其他（哨兵间，客户端）发送通知。

  

- **自动故障转移**
  断开master与slave连接，选取一个slave作为master，将其他slave连接到新的master，并告知客户端新的服务器地址

**注意：哨兵也是一台redis服务器，只是不提供数据服务通常哨兵配置数量为单数**



**启用哨兵模式**

配置哨兵

- 配置一拖二的主从结构
- 配置三个哨兵(配置相同，端口不同)
- 启动哨兵

> redis-sentinel sentinel- 端口号 .conf

![image-20210313211842733](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313211842733.png)



**哨兵工作原路**

主从切换三个阶段

- 监控
- 通知
- 故障转移



**阶段一：监控阶段**

![image-20210313212005678](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313212005678.png)

- 用于同步各个节点的状态信息

  - 获取各个sentine的状态(是否在线)
  - 获取master状态
    - master属性
      - runid
      - role:master

  - 获取所有slave的状态
    - slave属性
      - runid
      - role:master
      - offset
      - mater_host.master_port

![image-20210313212233484](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313212233484.png)

![image-20210313212243342](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313212243342.png)

![image-20210313212257746](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313212257746.png)

![image-20210313212313110](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313212313110.png)

**阶段三：故障转移阶段**

- 服务器列表中挑选备选master
  - 在线的
  - 响应慢的
  - 与原master断开时间久的
  - 优先原则
    - 优先级
    - offset
    - runid

- 发送指令 sentinel
  -  向新的master发送slaveof no one
  -  向其他slave发送slaveof 新masterIP端口

- 监控
  - 同步信息

- 通知
  - 保持联通

- 故障转移
  - 发现问题
  - 竞选负责人
  - 优选心master
  - 新master上任，其他slave切换master，原master作为slave故障回复后连接



## 12.企业级解决方案

### 缓存预热

问题：

- 请求数量较高
- 主从之间数据吞吐量较大，数据同步操作频度高

**解决方案**

**前置准备工作：**

1. 日常例行统计数据访问记录，统计访问频度较高的热点数据
2. 利用LRU数据删除策略，构建数据留存队列

**准备工作：**

1. 将统计结果中的数据分类，根据级别，redis优先加载级别较高的热点数据
2. 利用分布式多服务器同时进行数据读取，提速数据加载过程
3. 热点数据主从同时预热

**实施：**

1. 使用脚本程序固定触发数据预热过程
2. 如果条件允许，使用了CDN（内容分发网络），效果会更好

 **总结：
缓存预热就是系统启动前，提前将相关的缓存数据直接加载到缓存系统。避免在用户请求的时候，先查询数据库，然后再将数据缓
存的问题！用户直接查询事先被预热的缓存数据！**



### 缓存雪崩

**数据库服务器崩溃（1）**

1. 系统平稳运行过程中，忽然数据库连接量激增
2. 应用服务器无法及时处理请求
3. 大量408，500错误页面出现
4. 客户反复刷新页面获取数据
5. 数据库崩溃
6. 应用服务器崩溃
7. 重启应用服务器无效
8. Redis服务器崩溃
9. Redis集群崩溃
10. 重启数据库后再次被瞬间流量放倒



**问题排查**

1. 在一个较短的时间内，缓存中较多的key集中过期
2. 此周期内请求访问过期的数据，redis未命中，redis向数据库获取数据
3. 数据库同时接收到大量的请求无法及时处理
4. Redis大量请求被积压，开始出现超时现象
5. 数据库流量激增，数据库崩溃
6. 重启后仍然面对缓存中无数据可用
7. Redis服务器资源被严重占用，Redis服务器崩溃
8. Redis集群呈现崩塌，集群瓦解
9. 应用服务器无法及时得到数据响应请求，来自客户端的请求数量越来越多，应用服务器崩溃
10. 应用服务器，redis，数据库全部重启，效果不理想

**问题**

- 短时间范围内
- 大量key集中过期

**解决方案**

1. 更多的页面静态化处理
2. 构建多级缓存架构
Nginx缓存+redis缓存+ehcache缓存
3. 检测Mysql严重耗时业务进行优化
对数据库的瓶颈排查：例如超时查询、耗时较高事务等
4. 灾难预警机制
监控redis服务器性能指标
- CPU占用、CPU使用率
-  内存容量
-  查询平均响应时间
-  线程数
5. 限流、降级
短时间范围内牺牲一些客户体验，限制一部分请求访问，降低应用服务器压力，待业务低速运转后再逐步放开访问

**总结
缓存雪崩就是瞬间过期数据量太大，导致对数据库服务器造成压力。如能够有效避免过期时间集中，可以有效解决雪崩现象的出现
（约40%），配合其他策略一起使用，并监控服务器的运行数据，根据运行记录做快速调整。**

![image-20210313213728118](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313213728118.png)



### 缓存击穿

**数据库服务器崩溃（2）**

1. 系统平稳运行过程中
2. 数据库连接量瞬间激增
3. Redis服务器无大量key过期
4. Redis内存平稳，无波动
5. Redis服务器CPU正常
6. 数据库崩溃

**问题**

- Redis中某个key过期，该key访问量巨大
- 多个数据请求从服务器压到Redis后，均未命中
- Redis在短时间内发起了大量对数据库中同一数据的访问

**分析**

- 单个key高热数据
- key过期

**解决方案（术）**

1. 预先设定
以电商为例，每个商家根据店铺等级，指定若干款主打商品，在购物节期间，加大此类信息key的过期时长
注意：购物节不仅仅指当天，以及后续若干天，访问峰值呈现逐渐降低的趋势
2. 现场调整
监控访问量，对自然流量激增的数据延长过期时间或设置为永久性key
3. 后台刷新数据
启动定时任务，高峰期来临之前，刷新数据有效期，确保不丢失
4. 二级缓存
设置不同的失效时间，保障不会被同时淘汰就行
5. 加锁
分布式锁，防止被击穿，但是要注意也是性能瓶颈，慎重！

**总结
缓存击穿就是单个高热数据过期的瞬间，数据访问量较大，未命中redis后，发起了大量对同一数据的数据库访问，导致对数据库服
务器造成压力。应对策略应该在业务数据分析与预防方面进行，配合运行监控测试与即时调整策略，毕竟单个key的过期监控难度
较高，配合雪崩处理策略即可。**



### 缓存穿透

**数据库服务器崩溃（3）**

1. 系统平稳运行过程中
2. 应用服务器流量随时间增量较大
3. Redis服务器命中率随时间逐步降低
4. Redis内存平稳，内存无压力
5. Redis服务器CPU占用激增
6. 数据库服务器压力激增
7. 数据库崩溃

**问题**

- Redis中出现大面积未命中
- 出现非正常URL访问

**分析**

- 获取的数据在数据库中也不存在，数据库查询未得到相应的数据
- Redis获取到null数据未进行持久化，直接返回
- 出现黑客攻击服务器

**解决方案（术）**

1. 缓存null
对查询结果为null的数据进行缓存（长期使用，定期清理），设定短时限，例如30-60秒，最高5分钟
2. 白名单策略
- 提前预热各种分类数据i d对应的bitmaps，id作为bitmaps的offset，相当于设置了数据白名单。当加载正常数据时，放
  行，加载异常数据时直接拦截（效率偏低）
-  使用布隆过滤器（有关布隆过滤器的命中问题对当前状况可以忽略）
3. 实施监控
实时监控redis命中率（业务正常范围时，通常会有一个波动值）与null数据的占比
- 非活动时段波动：通常检测3-5倍，超过5倍纳入重点排查对象
- 活动时段波动：通常检测10-50倍，超过50倍纳入重点排查对象
  根据倍数不同，启动不同的排查流程。然后使用黑名单进行防控（运营）
4. key加密
    问题出现后，临时启动防灾业务key，对key进行业务层传输加密服务，设定校验程序，过来的key校验
    例如每天随机分配60个加密串，挑选2到3个，混淆到页面数据id中，发现访问key不满足规则，驳回数据访问

**总结
缓存击穿访问了不存在的数据，跳过了合法数据的redis数据缓存阶段，每次访问数据库，导致对数据库服务器造成压力。通常此类
数据的出现量是一个较低的值，当出现此类情况以毒攻毒，并及时报警。应对策略应该在临时预案防范方面多做文章。
无论是黑名单还是白名单，都是对整体系统的压力，警报解除后尽快移除。**



### 性能指标监控

**监控指标**

- 性能指标：Performance
-  内存指标：Memory
-  基本活动指标：Basic activity
-  持久性指标：Persistence
-  错误指标：Error



- **性能指标：Performance**

| Name                      | Description             |
| ------------------------- | ----------------------- |
| latency                   | Redis响应一个请求的时间 |
| instantaneous_ops_per_sec | 平均每秒处理请求总数    |
| hit rate(calculated)      | 缓存命中率              |



- **内存指标：Memory**

| Name                    | Description                                 |
| ----------------------- | ------------------------------------------- |
| used_memory             | 已使用内存                                  |
| mem_fragmentation_ratio | 内存碎片率                                  |
| evicted_keys            | 由于最大内存限制被移的key数量               |
| blocked_clients         | 由于BLPOP,BRPOP,or BRPOPLPUSH而阻塞的客户端 |



- **基本活动指标：Basic activity**

| Name                       | Description                |
| -------------------------- | -------------------------- |
| connected_clients          | 客户端连接数               |
| connected_slaves           | Slave数量                  |
| master_last_io_seconds_ago | 最近一次主从交互之后的秒数 |
| keyspace                   | 数据库中的key值总数        |



- **持久性指标：Persistence**

| Name                        | Description                        |
| --------------------------- | ---------------------------------- |
| rdb_last_save_time          | 最后一次持久化保存到磁盘的时间戳   |
| rdb_changes_since_last_save | 自最后一次持久化以来的数据库更改数 |



- **错误指标：Error**

| Name                           | Description                           |
| ------------------------------ | ------------------------------------- |
| rejected_connections           | 由于达到maxclient限制而被拒绝的连接数 |
| keyspace_misses                | key值查找失败（没有命中）次数         |
| master_link_down_since_seconds | 主从断开的持续时间(以秒为单位)        |



监控方式

- 工具
  - Cloud Insight Redis
  - Prometheus
  - Redis-stat
  - Redis-faina
  - RedisLive
  - zabbix

- 命令
  - benchmark
  - redis cli
    - monitor
    - showlog

```shell
	$ redis-benchmark [-h ] [-p ] [-c ] [-n <requests]> [-k ]
  # 打印服务器调试信息
	$ monitor
	
	$ showlong [operator]
  # get ：获取慢查询日志
  # len ：获取慢查询日志条目数
  # reset ：重置慢查询日志
  # slowlog-log-slower-than 1000 #设置慢查询的时间下线，单位：微妙
  # slowlog-max-len 100 #设置慢查询命令对应的日志显示长度，单位：命令数
```

![image-20210313215951231](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313215951231.png)



## 13.Jedis

**Jedis是Redis官方推荐的Java连接开发工具。要在Java开发中使用好Redis中间件，必须对Jedis熟悉才能写成漂亮的代码**

![image-20210313220249208](https://gitee.com/zhuang-kang/note-picture/raw/master/Redis%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210313220249208.png)

**测试**

1、新建一个普通的Maven项目
2、导入redis的依赖！

```xml
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
  <version>3.2.0</version>
</dependency>
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>1.2.58</version>
</dependency>
```

3、编写测试代码

```java
public class Ping {
  public static void main(String[] args) {
    Jedis jedis = new Jedis("127.0.0.1",6379);
    System.out.println("连接成功");
    //查看服务是否运行
    System.out.println("服务正在运行: "+jedis.ping());
 }
}
```

4、启动redis服务
5、启动测试，结果

> 连接成功
> 服务正在运行: PONG

**对key常用命令**

```java
public class TestKey {
  public static void main(String[] args) {
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    System.out.println("清空数据："+jedis.flushDB());
    System.out.println("判断某个键是否存在："+jedis.exists("username"));
    System.out.println("新增<'username','kuangshen'>的键值
对："+jedis.set("username", "kuangshen"));
    System.out.println("新增<'password','password'>的键值
对："+jedis.set("password", "password"));
    System.out.print("系统中所有的键如下：");
    Set<String> keys = jedis.keys("*");
    System.out.println(keys);
    System.out.println("删除键password:"+jedis.del("password"));
    System.out.println("判断键password是否存
在："+jedis.exists("password"));
    System.out.println("查看键username所存储的值的类
型："+jedis.type("username"));
    System.out.println("随机返回key空间的一个："+jedis.randomKey());
    System.out.println("重命名key："+jedis.rename("username","name"));
    System.out.println("取出改后的name："+jedis.get("name"));
    System.out.println("按索引查询："+jedis.select(0));
    System.out.println("删除当前选择数据库中的所有key："+jedis.flushDB());
    System.out.println("返回当前数据库中key的数目："+jedis.dbSize());
    System.out.println("删除所有数据库中的所有key："+jedis.flushAll());
 }
}
```

**对String操作的命令**

```java
public class TestString {
  public static void main(String[] args) {
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    jedis.flushDB();
    System.out.println("===========增加数据===========");
    System.out.println(jedis.set("key1","value1"));
    System.out.println(jedis.set("key2","value2"));
    System.out.println(jedis.set("key3", "value3"));
      System.out.println("删除键key2:"+jedis.del("key2"));
    System.out.println("获取键key2:"+jedis.get("key2"));
    System.out.println("修改key1:"+jedis.set("key1", "value1Changed"));
    System.out.println("获取key1的值："+jedis.get("key1"));
    System.out.println("在key3后面加入值："+jedis.append("key3", "End"));
    System.out.println("key3的值："+jedis.get("key3"));
    System.out.println("增加多个键值对："+jedis.mset("key01","value01","key02","value02","key03","value03"));
    System.out.println("获取多个键值对："+jedis.mget("key01","key02","key03"));
    System.out.println("获取多个键值对："+jedis.mget("key01","key02","key03","key04"));
    System.out.println("删除多个键值对："+jedis.del("key01","key02"));
    System.out.println("获取多个键值对："+jedis.mget("key01","key02","key03"));
    jedis.flushDB();
    System.out.println("===========新增键值对防止覆盖原先值==============");
    System.out.println(jedis.setnx("key1", "value1"));
    System.out.println(jedis.setnx("key2", "value2"));
    System.out.println(jedis.setnx("key2", "value2-new"));
    System.out.println(jedis.get("key1"));
    System.out.println(jedis.get("key2"));
    System.out.println("===========新增键值对并设置有效时间=============");
    System.out.println(jedis.setex("key3", 2, "value3"));
    System.out.println(jedis.get("key3"));
    try {
      TimeUnit.SECONDS.sleep(3);
   } catch (InterruptedException e) {
      e.printStackTrace();
   }
    System.out.println(jedis.get("key3"));
    System.out.println("===========获取原值，更新为新值==========");
    System.out.println(jedis.getSet("key2", "key2GetSet"));
    System.out.println(jedis.get("key2"));
    System.out.println("获得key2的值的字串："+jedis.getrange("key2", 2,4));
 }
}
```

**对List操作命令**

```java
public class TestList {
  public static void main(String[] args) {
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    jedis.flushDB();
    System.out.println("===========添加一个list===========");
    jedis.lpush("collections", "ArrayList", "Vector", "Stack",
"HashMap", "WeakHashMap", "LinkedHashMap");
    jedis.lpush("collections", "HashSet");
    jedis.lpush("collections", "TreeSet");
    jedis.lpush("collections", "TreeMap");
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
      //-1代表倒数第一个元素，-2代表倒数第二个元素,end为-1表示查询全部
      System.out.println("collections区间0-3的元素："+jedis.lrange("collections",0,3));
    System.out.println("===============================");
    // 删除列表指定的值 ，第二个参数为删除的个数（有重复时），后add进去的值先被删，类似于出栈
    System.out.println("删除指定元素个数："+jedis.lrem("collections", 2,"HashMap"));
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
    System.out.println("删除下表0-3区间之外的元素："+jedis.ltrim("collections", 0, 3));
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
    System.out.println("collections列表出栈（左端）："+jedis.lpop("collections"));
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
    System.out.println("collections添加元素，从列表右端，与lpush相对应："+jedis.rpush("collections", "EnumMap"));
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
    System.out.println("collections列表出栈（右端）："+jedis.rpop("collections"));
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
    System.out.println("修改collections指定下标1的内容："+jedis.lset("collections", 1, "LinkedArrayList"));
    System.out.println("collections的内容："+jedis.lrange("collections",0, -1));
    System.out.println("===============================");
    System.out.println("collections的长度："+jedis.llen("collections"));
    System.out.println("获取collections下标为2的元素："+jedis.lindex("collections", 2));
    System.out.println("===============================");
    jedis.lpush("sortedList", "3","6","2","0","7","4");
    System.out.println("sortedList排序前："+jedis.lrange("sortedList", 0,-1));
    System.out.println(jedis.sort("sortedList"));
    System.out.println("sortedList排序后："+jedis.lrange("sortedList", 0,-1));
 }
}
```

**对Set的操作命令**

```java
public class TestSet {
  public static void main(String[] args) {
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    jedis.flushDB();
    System.out.println("============向集合中添加元素（不重复）============");
    System.out.println(jedis.sadd("eleSet","e1","e2","e4","e3","e0","e8","e7","e5"));
    System.out.println(jedis.sadd("eleSet", "e6"));
    System.out.println(jedis.sadd("eleSet", "e6"));
    System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
    System.out.println("删除一个元素e0："+jedis.srem("eleSet", "e0"));
    System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
    System.out.println("删除两个元素e7和e6："+jedis.srem("eleSet","e7","e6"));
    System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
    System.out.println("随机的移除集合中的一个元素："+jedis.spop("eleSet"));
    System.out.println("随机的移除集合中的一个元素："+jedis.spop("eleSet"));
    System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
    System.out.println("eleSet中包含元素的个数："+jedis.scard("eleSet"));
    System.out.println("e3是否在eleSet中："+jedis.sismember("eleSet","e3"));
    System.out.println("e1是否在eleSet中："+jedis.sismember("eleSet","e1"));
    System.out.println("e1是否在eleSet中："+jedis.sismember("eleSet","e5"));
    System.out.println("=================================");
    System.out.println(jedis.sadd("eleSet1","e1","e2","e4","e3","e0","e8","e7","e5"));
    System.out.println(jedis.sadd("eleSet2","e1","e2","e4","e3","e0","e8"));
    System.out.println("将eleSet1中删除e1并存入eleSet3中："+jedis.smove("eleSet1", "eleSet3", "e1"));
      //移到集合元素
    System.out.println("将eleSet1中删除e2并存入eleSet3中："+jedis.smove("eleSet1", "eleSet3", "e2"));
    System.out.println("eleSet1中的元素："+jedis.smembers("eleSet1"));
    System.out.println("eleSet3中的元素："+jedis.smembers("eleSet3"));
    System.out.println("============集合运算=================");
    System.out.println("eleSet1中的元素："+jedis.smembers("eleSet1"));
    System.out.println("eleSet2中的元素："+jedis.smembers("eleSet2"));
    System.out.println("eleSet1和eleSet2的交集:"+jedis.sinter("eleSet1","eleSet2"));
    System.out.println("eleSet1和eleSet2的并集:"+jedis.sunion("eleSet1","eleSet2"));
    System.out.println("eleSet1和eleSet2的差集:"+jedis.sdiff("eleSet1","eleSet2"));//eleSet1中有，eleSet2中没有
    jedis.sinterstore("eleSet4","eleSet1","eleSet2");//求交集并将交集保存到dstkey的集合
    System.out.println("eleSet4中的元素："+jedis.smembers("eleSet4"));
 }
}
```

**对Hash的操作命令**

```java
public class TestHash {
  public static void main(String[] args) {
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    jedis.flushDB();
    Map<String,String> map = new HashMap<>();
    map.put("key1","value1");
    map.put("key2","value2");
    map.put("key3","value3");
    map.put("key4","value4");
    //添加名称为hash（key）的hash元素
    jedis.hmset("hash",map);
    //向名称为hash的hash中添加key为key5，value为value5元素
    jedis.hset("hash", "key5", "value5");
    System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));//return Map<String,String>
    System.out.println("散列hash的所有键为："+jedis.hkeys("hash"));//returnSet<String>
    System.out.println("散列hash的所有值为："+jedis.hvals("hash"));//returnList<String>
    System.out.println("将key6保存的值加上一个整数，如果key6不存在则添加key6："+jedis.hincrBy("hash", "key6", 6));
    System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));
    System.out.println("将key6保存的值加上一个整数，如果key6不存在则添加key6："+jedis.hincrBy("hash", "key6", 3));
    System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));
    System.out.println("删除一个或者多个键值对："+jedis.hdel("hash","key2"));
    System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));
    System.out.println("散列hash中键值对的个数："+jedis.hlen("hash"));
    System.out.println("判断hash中是否存在key2："+jedis.hexists("hash","key2"));
    System.out.println("判断hash中是否存在key3："+jedis.hexists("hash","key3"));
    System.out.println("获取hash中的值："+jedis.hmget("hash","key3"));
    System.out.println("获取hash中的值："+jedis.hmget("hash","key3","key4"));
 }
}
```

**事务**

```java
public class TestMulti {
  public static void main(String[] args) {
    //创建客户端连接服务端，redis服务端需要被开启
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    jedis.flushDB();
    JSONObject jsonObject = new JSONObject();
    jsonObject.put("hello", "world");
    jsonObject.put("name", "java");
    //开启事务
    Transaction multi = jedis.multi();
    String result = jsonObject.toJSONString();
    try{
      //向redis存入一条数据
      multi.set("json", result);
      //再存入一条数据
      multi.set("json2", result);
      //这里引发了异常，用0作为被除数
      int i = 100/0;
      //如果没有引发异常，执行进入队列的命令
        multi.exec();
   }catch(Exception e){
      e.printStackTrace();
      //如果出现异常，回滚
      multi.discard();
   }finally{
      System.out.println(jedis.get("json"));
      System.out.println(jedis.get("json2"));
      //最终关闭客户端
      jedis.close();
   }
 }
}
```



## 13.SpringBoot整合Redis

在SpringBoot中一般使用RedisTemplate提供的方法来操作Redis。那么使用SpringBoot整合Redis需要那些步骤呢。
**1、 JedisPoolConfig (这个是配置连接池)**

**2、 RedisConnectionFactory 这个是配置连接信息，这里的RedisConnectionFactory是一个接口**，我们需要使用它的实现类，在SpringD Data Redis方案中提供了以下四种工厂模型：

- JredisConnectionFactory
- JedisConnectionFactory
- LettuceConnectionFactory
- SrpConnectionFactory

**3、 RedisTemplate 基本操作** 

导入依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

yml配置

```yml
spring:
 redis:
  host: 127.0.0.1
  port: 6379
  password: *****
  jedis:
   pool:
    max-active: 8
    max-wait: -1ms
    max-idle: 500
    min-idle: 0
  lettuce:
   shutdown-timeout: 0ms
```

编写测试类

```java
@SpringBootTest
class SpringbootRedisApplicationTests {
  @Autowired
  private RedisTemplate<String,String> redisTemplate;
  @Test
  void contextLoads() {
    redisTemplate.opsForValue().set("myKey","myValue");
    System.out.println(redisTemplate.opsForValue().get("myKey"));
 }
}
```

**封装工具类**

分析 RedisAutoConfiguration 自动配置类

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class,JedisConnectionConfiguration.class })
    public class RedisAutoConfiguration {
    	@Bean
    	@ConditionalOnMissingBean(name = "redisTemplate")
    	public RedisTemplate<Object, Object>redisTemplate(RedisConnectionFactory redisConnectionFactory)
    	throws UnknownHostException {
    		RedisTemplate<Object, Object> template = new RedisTemplate<>();
    		template.setConnectionFactory(redisConnectionFactory);
    		return template;
    }
    	@Bean
   	 	@ConditionalOnMissingBean
    	public StringRedisTemplate stringRedisTemplate(RedisConnectionFactoryredisConnectionFactory)
   		 throws UnknownHostException {
    		StringRedisTemplate template = new StringRedisTemplate();
    		template.setConnectionFactory(redisConnectionFactory);
    		return template;
    }
    }
```

通过源码可以看出，SpringBoot自动帮我们在容器中生成了一个**RedisTemplate**和一个**StringRedisTemplate**。
但是，这个RedisTemplate的泛型是<Object,Object>，写代码不方便，**需要写好多类型转换的代码**；我们需要一个泛型为<String,Object>形式的RedisTemplate。并且，这个RedisTemplate没有设置数据存在Redis时，key及value的序列化方式。
看到这个@ConditionalOnMissingBean注解后，就知道如果Spring容器中有了RedisTemplate对象了，这个自动配置的RedisTemplate不会实例化。因此我们可以直接自己写个配置类，**配置RedisTemplate**

**RedisTemplate**

```java
@Configuration
public class RedisConfig {
 @Bean
 @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
   RedisTemplate<String, Object> template = new RedisTemplate<String,Object>();
   template.setConnectionFactory(factory);
   Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
   ObjectMapper om = new ObjectMapper();
   om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
   om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
   jackson2JsonRedisSerializer.setObjectMapper(om);
   StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
  
   // key采用String的序列化方式
   template.setKeySerializer(stringRedisSerializer);
   // hash的key也采用String的序列化方式
   template.setHashKeySerializer(stringRedisSerializer);
   // value序列化方式采用jackson
   template.setValueSerializer(jackson2JsonRedisSerializer);
   // hash的value序列化方式采用jackson
   template.setHashValueSerializer(jackson2JsonRedisSerializer);
   template.afterPropertiesSet();
  
   return template;
}
}
```

**写一个Redis工具类（直接用RedisTemplate操作Redis，需要很多行代码，因此直接封装好一个RedisUtils，这样写代码更方便点。这个RedisUtils交给Spring容器实例化，使用时直接注解注入。）**

```java
@Component
public final class RedisUtil {
  @Autowired
  private RedisTemplate<String, Object> redisTemplate;
 
  // =============================common============================
  /**
  * 指定缓存失效时间
  * @param key 键
  * @param time 时间(秒)
  */
  public boolean expire(String key, long time) {
    try {
        if (time > 0) {
        redisTemplate.expire(key, time, TimeUnit.SECONDS);
     }
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 根据key 获取过期时间
  * @param key 键 不能为null
  * @return 时间(秒) 返回0代表为永久有效
  */
  public long getExpire(String key) {
    return redisTemplate.getExpire(key, TimeUnit.SECONDS);
 }
  /**
  * 判断key是否存在
  * @param key 键
  * @return true 存在 false不存在
  */
  public boolean hasKey(String key) {
    try {
      return redisTemplate.hasKey(key);
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 删除缓存
  * @param key 可以传一个值 或多个
  */
  @SuppressWarnings("unchecked")
  public void del(String... key) {
    if (key != null && key.length > 0) {
      if (key.length == 1) {
        redisTemplate.delete(key[0]);
     } else {
        redisTemplate.delete(CollectionUtils.arrayToList(key));
     }
   }
 }
    // ============================String=============================
  /**
  * 普通缓存获取
  * @param key 键
  * @return 值
  */
    public Object get(String key) {
    return key == null ? null : redisTemplate.opsForValue().get(key);
 }
 
  /**
  * 普通缓存放入
  * @param key  键
  * @param value 值
  * @return true成功 false失败
  */
  public boolean set(String key, Object value) {
    try {
      redisTemplate.opsForValue().set(key, value);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 普通缓存放入并设置时间
  * @param key  键
  * @param value 值
  * @param time 时间(秒) time要大于0 如果time小于等于0 将设置无限期
  * @return true成功 false 失败
  */
  public boolean set(String key, Object value, long time) {
    try {
      if (time > 0) {
        redisTemplate.opsForValue().set(key, value, time,TimeUnit.SECONDS);
     } else {
        set(key, value);
     }
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 递增
  * @param key  键
  * @param delta 要增加几(大于0)
  */
  public long incr(String key, long delta) {
    if (delta < 0) {
      throw new RuntimeException("递增因子必须大于0");
   }
    return redisTemplate.opsForValue().increment(key, delta);
 }
    /**
  * 递减
  * @param key  键
  * @param delta 要减少几(小于0)
  */
  public long decr(String key, long delta) {
    if (delta < 0) {
      throw new RuntimeException("递减因子必须大于0");
   }
    return redisTemplate.opsForValue().increment(key, -delta);
 }
  // ================================Map=================================
  /**
  * HashGet
  * @param key 键 不能为null
  * @param item 项 不能为null
  */
  public Object hget(String key, String item) {
    return redisTemplate.opsForHash().get(key, item);
 }
 
  /**
  * 获取hashKey对应的所有键值
  * @param key 键
  * @return 对应的多个键值
  */
  public Map<Object, Object> hmget(String key) {
    return redisTemplate.opsForHash().entries(key);
 }
 
  /**
  * HashSet
  * @param key 键
  * @param map 对应多个键值
  */
  public boolean hmset(String key, Map<String, Object> map) {
    try {
      redisTemplate.opsForHash().putAll(key, map);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * HashSet 并设置时间
  * @param key 键
  * @param map 对应多个键值
  * @param time 时间(秒)
  * @return true成功 false失败
  */
  public boolean hmset(String key, Map<String, Object> map, long time) {
      try {
      redisTemplate.opsForHash().putAll(key, map);
      if (time > 0) {
        expire(key, time);
     }
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 向一张hash表中放入数据,如果不存在将创建
  *
  * @param key  键
  * @param item 项
  * @param value 值
  * @return true 成功 false失败
  */
  public boolean hset(String key, String item, Object value) {
    try {
      redisTemplate.opsForHash().put(key, item, value);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 向一张hash表中放入数据,如果不存在将创建
  *
  * @param key  键
  * @param item 项
  * @param value 值
  * @param time 时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
  * @return true 成功 false失败
  */
  public boolean hset(String key, String item, Object value, long time) {
    try {
      redisTemplate.opsForHash().put(key, item, value);
      if (time > 0) {
        expire(key, time);
     }
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 删除hash表中的值
  *
  * @param key 键 不能为null
  * @param item 项 可以使多个 不能为null
  */
  public void hdel(String key, Object... item) {
    redisTemplate.opsForHash().delete(key, item);
 }
  /**
  * 判断hash表中是否有该项的值
  *
  * @param key 键 不能为null
  * @param item 项 不能为null
  * @return true 存在 false不存在
  */
  public boolean hHasKey(String key, String item) {
    return redisTemplate.opsForHash().hasKey(key, item);
 }
  /**
  * hash递增 如果不存在,就会创建一个 并把新增后的值返回
  *
  * @param key 键
  * @param item 项
  * @param by  要增加几(大于0)
  */
  public double hincr(String key, String item, double by) {
    return redisTemplate.opsForHash().increment(key, item, by);
 }
  /**
  * hash递减
  *
  * @param key 键
  * @param item 项
  * @param by  要减少记(小于0)
  */
  public double hdecr(String key, String item, double by) {
    return redisTemplate.opsForHash().increment(key, item, -by);
 }
  // ============================set=============================
  /**
  * 根据key获取Set中的所有值
  * @param key 键
  */
  public Set<Object> sGet(String key) {
    try {
      return redisTemplate.opsForSet().members(key);
   } catch (Exception e) {
      e.printStackTrace();
      return null;
   }
 }
    /**
  * 根据value从一个set中查询,是否存在
  *
  * @param key  键
  * @param value 值
  * @return true 存在 false不存在
  */
  public boolean sHasKey(String key, Object value) {
    try {
      return redisTemplate.opsForSet().isMember(key, value);
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 将数据放入set缓存
  *
  * @param key  键
  * @param values 值 可以是多个
  * @return 成功个数
  */
  public long sSet(String key, Object... values) {
    try {
      return redisTemplate.opsForSet().add(key, values);
   } catch (Exception e) {
      e.printStackTrace();
      return 0;
   }
 }
  /**
  * 将set数据放入缓存
  *
  * @param key  键
  * @param time  时间(秒)
  * @param values 值 可以是多个
  * @return 成功个数
  */
  public long sSetAndTime(String key, long time, Object... values) {
    try {
      Long count = redisTemplate.opsForSet().add(key, values);
      if (time > 0)
        expire(key, time);
      return count;
   } catch (Exception e) {
      e.printStackTrace();
      return 0;
   }
 }
  /**
  * 获取set缓存的长度
  *
  * @param key 键
  */
  public long sGetSetSize(String key) {
    try {
      return redisTemplate.opsForSet().size(key);
   } catch (Exception e) {
      e.printStackTrace();
      return 0;
   }
 }
  /**
  * 移除值为value的
  *
  * @param key  键
  * @param values 值 可以是多个
  * @return 移除的个数
  */
  public long setRemove(String key, Object... values) {
    try {
      Long count = redisTemplate.opsForSet().remove(key, values);
      return count;
   } catch (Exception e) {
      e.printStackTrace();
      return 0;
   }
 }
  // ===============================list=================================
 
  /**
  * 获取list缓存的内容
  *
  * @param key  键
  * @param start 开始
  * @param end  结束 0 到 -1代表所有值
  */
  public List<Object> lGet(String key, long start, long end) {
    try {
      return redisTemplate.opsForList().range(key, start, end);
   } catch (Exception e) {
      e.printStackTrace();
      return null;
   }
 }
  /**
  * 获取list缓存的长度
  *
  * @param key 键
  */
  public long lGetListSize(String key) {
    try {
      return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
      e.printStackTrace();
      return 0;
   }
 }
  /**
  * 通过索引 获取list中的值
  *
  * @param key  键
  * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0
时，-1，表尾，-2倒数第二个元素，依次类推
  */
  public Object lGetIndex(String key, long index) {
    try {
      return redisTemplate.opsForList().index(key, index);
   } catch (Exception e) {
      e.printStackTrace();
      return null;
   }
 }
  /**
  * 将list放入缓存
  *
  * @param key  键
  * @param value 值
  */
  public boolean lSet(String key, Object value) {
    try {
      redisTemplate.opsForList().rightPush(key, value);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 将list放入缓存
  * @param key  键
  * @param value 值
  * @param time 时间(秒)
  */
  public boolean lSet(String key, Object value, long time) {
    try {
      redisTemplate.opsForList().rightPush(key, value);
      if (time > 0)
        expire(key, time);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
      }
  /**
  * 将list放入缓存
  *
  * @param key  键
  * @param value 值
  * @return
  */
  public boolean lSet(String key, List<Object> value) {
    try {
      redisTemplate.opsForList().rightPushAll(key, value);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 将list放入缓存
  *
  * @param key  键
  * @param value 值
  * @param time 时间(秒)
  * @return
  */
  public boolean lSet(String key, List<Object> value, long time) {
    try {
      redisTemplate.opsForList().rightPushAll(key, value);
      if (time > 0)
        expire(key, time);
      return true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
   }
 }
  /**
  * 根据索引修改list中的某条数据
  *
  * @param key  键
  * @param index 索引
  * @param value 值
  * @return
  */
  public boolean lUpdateIndex(String key, long index, Object value) {
    try {
      redisTemplate.opsForList().set(key, index, value);
      ren true;
   } catch (Exception e) {
      e.printStackTrace();
      return false;
        }
 }
  /**
  * 移除N个值为value
  *
  * @param key  键
  * @param count 移除多少个
  * @param value 值
  * @return 移除的个数
  */
  public long lRemove(String key, long count, Object value) {
    try {
      Long remove = redisTemplate.opsForList().remove(key, count,value);
      return remove;
   } catch (Exception e) {
      e.printStackTrace();
      return 0;
   }
	 }
}
```


 **学习地址https://www.bilibili.com/video/BV1oW411u75R?from=search&seid=3220241811845891398** 