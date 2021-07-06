# RabbitMQ学习笔记

**学习地址:https://www.bilibili.com/video/BV1dX4y1V73G**

**仓库地址:https://gitee.com/zhuang-kang/rabbit-mq**

# 1，什么是中间件

一个企业可能运行多个不同业务系统，可能基于不同的操作系统，不同数据库，异构的网络环境，问题是，如何把这些信息系统结合结合成一个有机地协同工作的整体，真正实现跨平台，分布式应用，中间件便是解决之道

- **中间件：**处于操作系统和应用程序之间的软件，在中间件中必须要有一个通信中间件，即 中间件=平台＋通信，这个定义也限定只有用于分布式，同时还可以把它与支撑软件和试用软件分开来

> 1,RMI 远程调用
>
> 2，Load Balancing 负载均衡 将访问负荷分散到各个服务器中
>
> 3，Clustering 集群 用多个小服务器代替大型机
>
> 4，Threading 多线程处理
>
> 5，Transaction 事务
>
> 6，Component Life Cycle 组件的生命周期管理
>
> ......

## 1.2 为什么需要使用中间件

具体说，中间件屏蔽底层操作系统的复杂性，使程序员面对一个简单而统一的开发环境，减少程序设计的复杂性，将注意力集中在自己的业务上，不必为程序在不同软件上的移植而重复工作，减少技术负担，开发的简便，开发周期短，减少系统维护，减少费用投入



## 1.3 中间件的特点

中间件是位于平台（硬件和操作系统）和应用之间的通用服务，针对不同操作系统和硬件平台，可以有符合接口和协议的多种实现

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-14_14-43-35.png)

**特点如下：**

- 满足大量应用的需求
- 运行于多种硬件和OS平台
- 支持分布计算，提供网络，硬件和OS平台的透明性的应用或服务的交互
- 支持标准的协议
- 支持标准的接口

> 简单说，中间件有个很大的特点，脱离于具体设计目标，而具备提供普遍独立功能需求的模块。使得中间件是可替换的，如果一个系统设计中，中间件是不可替换的，不是架构，框架设计有问题，就是中间件出现问题



## 1.4 什么时候使用中间件技术

在项目的架构和重构中，使用任何技术和架构的改变我们都需要谨慎斟酌和思考，因为任何技术的融入和变化都可能人员，技术，和威本的增加，中间件的技术一般现在一些互联网公司或者项目中使用比较多，如果你仅仅还只是一个初创公司建议还是使用单体架构，最多加个缓存中间件即可，不要盲目追求新或者所谓的高性能，而追求的背后一定是业务的驱动和项目的驱动，因为一旦追求就意味着你的学习成本，公司的人员结构以及服务器成本，维护和运维的成本都会增加，所以需要谨慎选择和考虑。

但是作为一个开放人员，一定要有学习中间件技术的能力和思维，否则很容易当项目发展到一个阶段在去掌握估计或者在面试中提及，就会给自己带来不小的困扰，在当今这个时代这些技术也并不是什么新鲜的东西，如果去掌握和挖掘最关键的还是自己花时间和花精力去探讨和研究

# 2，中间件技术及架构的概述

## 2.1 什么是中间件 

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-14_14-54-46.png)

## 2.2 单体架构

在企业开发的中，大部分的初期架构都采用的是单体架构的模式进行架构，而这种架构的典型的特点:就是把所有的业务和模块，源代码，静态资源文件等都放在一个一工程中，如果其中的一个模块升级或者迭代发生一个很小变动都会重新编译和重新部看项目。这种的架构存在的问题就是:

- 耦合度太高
- 运维成本过高
- 不易维护
- 服务器成本高
- 后续复杂度增加



## 2.3 分布式架构

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-14_14-58-39.png)

何谓分布式系统呢:

> 通俗一点:就是一个请求由服务器端的多个服务(服务或者系统)协同处理完成

和单体架构不同的是，单体架构是一个请求发起ivm调度线程（确切的是tomcat线程池）分配线程Thread来处理请求直到释放，而分布式是系统是:一个请求是由多个系统共同来协同完成，jvm和环境都可能是独立。如果生活中的比喻的话，单体架构就想建设一个小房子很快就能够搞定，如果你要建设一个鸟巢者大型的建筑，你就必须是各个环节的协同和分布，这样目的也是项目发展都后期的时候要去部署和思考的问题。我们也不能看出来:分布式架构系统存在的特点和问题如下:

**存在问题**

- 学习成本高，技术栈过多
- 运维成本和服务器成本增加
- 人员成本增高
- 项目负载度上升
- 面临错误和容错性增加
- 占用服务器端口和通讯选择成本高
- 安全性的考虑和因素逼迫选择RMI/MQ相关服务器通讯

**好处**

- 服务系统的独立，占用的服务器资源减少和占用的硬件成本减少，确切的说是:可以合理的分配服务资源，不造成服务器资源的浪费
- 系统的独立维护和部署，耦合度降低，可插拔性。
- 系统的架构和技术栈的选择可以变的灵活(而不是单纯的选择java)



# 3，基于中间件技术的架构

## 3.1 消息中间价的架构

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-14_15-04-20.png)

> 从上图看出 消息中间件是
>
> 1，利用可靠的消息传递机制进行系统直接通讯
>
> 2，通过提供可靠消息和消息的排队机制，可以在分布式系统下扩展进程间的通讯



## 3.2 消息中间件的应用场景

- 跨系统数据传递
- 高并发的流量削峰
- 数据的分发和异步处理
- 大数据分析与传递
- 分布式事务

## 3.3 常见的消息中间件

ActiveMQ、RabbitMQ、Kafka、RocketMQ等。

## 3.4 消息中间件的本质及设计

它是一种接受数据，接受请求、存储数据、发送数据等功能的技术服务。

> MQ消息队列:负责数据的传接受，存储和传递，所以性能要过于普通服务和技术。

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-14_15-12-04.png)

![Snipaste_2021-04-14_15-12-12](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-14_15-12-12.png)

## 3.5 消息中间件的核心组成部分

- 消息的协议
- 消息的持久化机制
- 消息的分发策略
- 消息的高可用
- 消息的容错机制

## 3.6 小结

其实不论选择单体架构还是分布式架构都是项目开发的一个阶段，在什么阶段选择适合的架构方式，而不能盲目追求,最后造成的后果和问题都需要自己买单。但是作为一个开发人员学习和探讨新的技术是我们每个程序开发者都应该去保持和思考的问题。当我们没办法去改变社会和世界的时候，我们为了生活和生存那就必须要迎合企业和市场的需求，发挥你的价值和所学的才能，创造价值和实现自我。



# 4，消息队列协议

## 4.1 什么是协议

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-04-58.png)

我们知道消息中间件负责数据的传递，存储，和分发消费三个部分，数据的存储和分发的过程中肯定要遵循某种约定成俗的规范，你是采用底层的TCP/IPJT, UDP协议还是其他的自己取构建等，而这些约定成俗的规范就称之为:协议。



>所谓协议是指:
>1:计算机底层操作系统和应用程序通讯时共同遵守的一组约定，只有遵循共同的约定和规范，系统和底层操作系统之间才能相互交流。
>2:和一般的网络应用程序的不同它主要负责数据的接受和传递，所以性能比较的高。
>
>3:协议对数据格式和计算机之间交换数据都必须严格遵守规范。



## 4.2 网络协议的三要素

1.语法。语法是用户数据与控制信息的结构与格式,以及数据出现的顺序。
2.语义。语义是解释控制信息每个部分的意义。它规定了需要发出何种控制信息,以及完成的动作与做出什么样的响应。3.时序。时序是对事件发生顺序的详细说明。

比如我MQ发送一个信息，是以什么数据格式发送到队列中，然后每个部分的含义是什么，发送完毕以后的执行的动作,以及消费者消费消息的动作，消费完毕的响应结果和反馈是什么，然后按照对应的执行顺序进行处理。如果你还是不理解:大家每天都在接触的http请求协议:

**1:语法:http规定了请求报文和响应报文的格式。**

**2:语义:客户端主动发起请求称之为请求。(这是一种定义，同时你发起的是post/get请求)**

**3:时序:一个请求对应一个响应。(一定先有请求在有响应，这个是时序)**

而消息中间件采用的并不是http协议，而常见的消息中间件协议有:OpenWire、AMQP、MQTT、Kafka,OpenMessage协议。
**面试题:为什么消息中间件不直接使用http协议呢?**

- 1:因为http请求报文料和响应报文头是比较复杂的，包含了cookie，数据的加密解密，状态码，响应码等附加的功能，但是对于一个消息而言，我们并不需要这么复杂，也没有这个必要性，它其实就是负责数据传递，存储，分发就行，一定要追求的是高性能。尽量简洁，快速。
- 2:大部分情况下http大部分都是短链接，在实际的交互过程中，一个请求到响应很有可能会中断，中断以后就不会就行持久化，就会造成请求的丢失。这样就不利于消息中间件的业务场景，因为消息中间件可能是一个长期的获取消息的过程，出现问题和故障要对数据或消息就行持久化等，目的是为了保证消息和数据的高可靠和稳健的运行。



## 4.3 AMQP协议

AMQP:(全称: Advanced Message Queuing Protocol)是高级消息队列协议。由摩根大通集团联合其他公司共同设计。是一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。Erlang中的实现有RabbitMQ等。

特性:
1:分布式事务支持。

2:消息的持久化支持。

3:高性能和高可靠的消息处理优势。



## 4.4 MQTT协议

MQTT协议: (Message Queueing Telemetry Transport)消息队列是IBM开放的一个即时通讯协议，物联网系统架构中的重要组成部分。

**特点:**

- 轻量

- 结构简单

- 传输快，不支持事务
- 没有持久化设计

**应用场景：**

- 适用于计算能力有限

- 低带宽

- 网络不稳定的场景。支持者:



## 4.5 OpenMessage协议

是近几年由阿里、雅r虎和滴滴出行、Stremalio等公司共同参与创立的分布式消息中间件、流处理等领域的应用开发标准。
特点:

1∶结构简单

2∶解析速度快

3:支持事务和持久化设计。



## 4.6 Kafaka协议

Kafka协议是基于TCP/IP的二进制协议。消想内部是通过长度来分割，由一些基本数据类型组成特点是:
1:结构的单一

2:解析速度快

3:无事务支持

4:有持久化设计



## 4.7 小结

协议:是在tcp/ip协议基础之上构建的一种约定成俗的规范和机制、它的主要目的可以让客户端（应用程序java，go)进行沟通和通讯。并且这种协议下规范必须具有持久性，高可用，高可靠的性能。



# 5，消息分发策略

**MQ消息队列有如下几个角色**

1:生产者

2:存储消息

3∶消费者
那么生产者生成消息以后，MQ进行存储，消费者是如何获取消息的呢?一般获取数据的方式无外乎推(push）或者拉(pull)两种方式，典型的git就有推拉机制，我们发送的http请求就是一种典型的拉取数据库数据返回的过程。而消息队列MQ是一种推送的过程，而这些推机制会适用到很多的业务场景也有很多对应推机制策略。

## 5.2 场景分析1

![]()![Snipaste_2021-04-15_21-18-03](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-18-03.png)

> 比如我在APP上下了一个订单，我们的系统和服务很多，我们如何得知这个消息被那个系统或者那些服务或者系统进行消费，那这个时候就需要一个分发的策略。这就需要消费策略。或者称之为消费的方法论。

## 5.2 场景分析2

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-20-37.png)

> 在发送消息的过程中可能会出现异常，或者网络的抖动，故障等等因为造成消息的无法消费，比如用户在下订单，消费MQ接受，订单系统出现故障，导致用户支付失败，那么这个时候就需要消息中间件就必须支持消息重试机制策略。也就是支持:出现问题和故障的情况下，消息不丢失还可以进行重发。



## 5.3 消息分发策略的机制和对比

|          | **ActiveMQ** | **RabbitMQ** | **Kafaka** | **RocketMQ** |
| -------- | ------------ | ------------ | ---------- | ------------ |
| 发布订阅 | 支持         | 支持         | 支持       | 支持         |
| 轮询发布 | 支持         | 支持         | 支持       | /            |
| 公平分发 | /            | 支持         | 支持       | /            |
| 重发     | 支持         | 支持         | /          | 支持         |
| 消息拉取 | /            | 支持         | 支持       | 支持         |



# 6，消息队列高可靠和高可用

所谓高可用：是指产品在规定的条件和规定的时刻或时间内处于可执行规定功能状态的能力，当业务量增加时，请求也过大，一台消息中间件服务器的会触及硬件 比如CPU 内存 磁盘 的极限，一台消息服务器无法满足需求，消息中间件必须支持集群部署，来达到高可用



## 6.1 集群模式1- Master-slave 主从共享数据的部署方式

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-25-24.png)

> 生产者将消费发送到Master节点,所有的都连接这个消息队列共享这块数据区域，Master节点负责写入，一旦Master挂掉，slave节点继续服务，形成高可用



## 6.2 集群模式2 Master-slave主从同步部署方式

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-25-57.png)

> 这种模式写入消息同样在Master主节点上，但是主节点会同步数据到slave节点形成副本，和zookeeper或者redis主从机制很类同，可以达到负载均衡的效果，如果消费者有多个这样可以去不同的节点进行消费，因为消费的拷贝和同步会回占用很大的带宽和网络资源，rabbitmq中有使用



## 6.3 集群模式3 多主集同步部署模式

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-34-05.png)

> 和上面差不多的模式



## 6.4 集群模式4 多主集群转发部署模式

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-35-51.png)

> 解释:如果你插入的数据是broker-1中，元数据信息会存储数据的相关描述和记录存放的位置(队列)。
> 它会对描述信息也就是元数据信息就行同步，如果消费者在broker-2中进行消费，发现自己几点没有对应的消息，可以从对应的元数据信息中去查询，然后返回对应的消息信息，场景:比如买火车票或者黄牛买演唱会门票，比如第一个黄牛有顾客说要买的演唱会门票，但是没有但是他会去联系其他的黄牛询问，如果有就返回。



## 6.5 集群模式5 Mater-slave 与Broker-cluster组合的方案

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-37-31.png)

> 解释:实现多主多从的热备机制来完成消息的高可用以及数据的热备机制，在生产规模达到一定的阶段的时候，这种使用的频率比较高。
> 这么集群模式，具体在后续的课程中会进行一个分析和讲解。他们的最终目的都是为保证︰消息服务器不会挂掉，出现了故障依然可以抱着消息服务继续使用。

**三句话:**
1:要么消息共享

2:要么消息同步

3:要么元数据共享



## 6.6 什么是高可靠机制

所谓高可用是指:是指系统可以无故障低持续运行，比如一个系统突然崩溃，报错，异常等等并不影响线上业务的正常运行，出错的几率极低，就称之为:高可靠。

在高并发的业务场景中，如果不能保证系统的高可靠，那造成的隐患和损失是非常严重的。

如何保证中间件消息的可靠性呢?可以从两个方面考虑:
1:消息的传输:通过协议来保证系统间数据解析的正确性。

2:消息的存储可靠:通过持久化来保证消息的可靠性。



## 7，RabbitMQ安装

**参考官网：https://www.rabbitmq.com/**



##  7.1 Docker安装

一个命令搞定

```shell
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

然后通过IP地址+15672端口访问即可

用户名和密码都是guest



# 8，RabbitMQ角色分类

## 8.1 none

- 不能访问management plugin



## 8.2 management:查看自己相关节点信息

- 列出自己可以通过AMQP登入的虚拟机

- 查看自己的虚拟机节点 virtual hosts的queues,exchanges和bindings信息
- 查看和关闭自己的channels和connections



## 8.3 Policymaker

- 包含management所有权限

- 查看和创建和删除自己的virtual hosts所属的policies和parameters信息



## 8.4 Monitoring

- 包含management所有权限
- 罗列出所有virtual hosts，包括不能登录的virtual hosts
- 查看其他用户的connections和channels信息
- 查看节点级别的数据如clustering和memory使用情况
- 查看所有的virtual hosts的全局统计信息



## 8.5  Administrator

- 最高权限
- 可以创建和删除virtual hosts·可以查看，创建和删除users
- 查看创建permisssions



# 9，入门案例 简单模式

**具体流程**

- 生产者发送消息
  1. 生产者创建连接（Connection），开启一个信道（Channel），连接到RabbitMQ Broker；
  2. 声明队列并设置属性；如是否排它，是否持久化，是否自动删除；
  3. 将路由键（空字符串）与队列绑定起来；
  4. 发送消息至RabbitMQ Broker；
  5. 关闭信道；
  6. 关闭连接；
- 消费者接收消息
  1. 消费者创建连接（Connection），开启一个信道（Channel），连接到RabbitMQ Broker
  2. 向Broker 请求消费相应队列中的消息，设置相应的回调函数；
  3. 等待Broker回应闭关投递响应队列中的消息，消费者接收消息；
  4. 确认（ack，自动确认）接收到的消息；
  5. RabbitMQ从队列中删除相应已经被确认的消息；
  6. 关闭信道；
  7. 关闭连接；

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_15-31-15.png)

先导入依赖 创建Maven工程

```xml
<dependencies>
        <dependency>
            <groupId>com.rabbitmq</groupId>
            <artifactId>amqp-client</artifactId>
            <version>5.11.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-amqp</artifactId>
            <version>2.3.6</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-rabbit</artifactId>
            <version>2.3.6</version>
        </dependency>
```

**编写Producer类**

```java
package com.zhuang.rabbitmq.simple;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            String queueName = "queue1";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */
            channel.queueDeclare(queueName, false, false, false, null);
            String message = "hello kang xiao zhuang";
            //发送消息给队列
            channel.basicPublish("", queueName, null, message.getBytes());

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

**编写Consumer类**

```java
package com.zhuang.rabbitmq.simple;

import com.rabbitmq.client.*;
import com.sun.org.apache.bcel.internal.generic.NEW;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer
 * @Description 用一句话描述类的作用
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Consumer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

            channel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("收到的消息是" + new String(delivery.getBody(), "UTF-8"));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.in.read();
            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

运行即可！

查看Web端控制器

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_16-45-31.png)

**先启动生产者会发现消息，再启动消费者 控制台打印出消费的消息**



# 10，AMQP协议

AMQP，即Advanced Message Queuing Protocol，一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。Erlang中的实现有RabbitMQ等



## 10.1 AMQP生产者流转过程

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-58-26.png)

## 10.2 AMQP消费者流转过程

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-59-12.png)

# 11，RabbitMQ核心功能部分

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_22-02-02.png)

## 11.1 核心概念

**Server:**又称Broker ,接受客户端的连接，实现AMQP实体服务。安装rabbitmq-serverConnection:连接，应用程序与Broker的网络连接TCP/IP/三次握手和四次挥手

**Channel :**网络信道，几乎所有的操作都在Channel中进行，Channel是进k%息读写的通道，客户端可以建立对各Channel，每个Channel代表一个会话任务。

**Message :**消息:服务与应用程序之间传送的数据，由Properties和body组成，Properties可是对消息进行修饰，比如消息的优先级，延迟等高级特性，Body则就是消息体的内容。

**Virtual Host：**虚拟地址，用于进行逻辑隔离，最上层的消息路由，一个虚拟主机理由可以有若千个Exhange和Queueu，同一个虚拟主机里面不能有相同名字的Exchange

**Exchange:**交换机，接受消息，根据路由键发送消息到绑定的队列。(==不具备消息存储的能力==)Bindings : Exchange和Queue之问的虚拟连接，binding中可以保护多个routing key.

**Routing key :**是一个路由规则，虚拟机可以用它来确定如何路由一个特定消息。

**Queue :**队列:也成为Message Queue,消息队列，保存消息并将它们转发给消费者。



## 11.2 核心架构

![]()![Snipaste_2021-04-15_22-04-54](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_22-04-54.png)



## 11.3 运行流程

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_22-05-59.png)



## 11.4 工作模式分类

### 11.4.1 简单模式 simple

- 参考入门案例

### 11.4.2 工作模式 work

- 类型 ：无
- 特点：分发机制

### 11.4.3 发布订阅模式 fanout

- 类型：fanout
- 特点：发布订阅模式，是一种广播机制，没有路由key

### 11.4.4 路由模式

- 类型 ：direct
- 特点：有routing-key 的匹配模式

### 11.4.5 主题模式

- 类型：topic
- 特点：模糊的routing-key的匹配模式



# 12，Fanout模式

生产者

```java
package com.zhuang.rabbitmq.routing;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

        //    String queueName = "queue1";

            String message = "hello fanout！！！";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */
            //准备交换机
            String exchangeName="fanout_exchange";

            //定义路由Key
            String routeKey="";
            //定义交换机类型
            String type="fanout";
          //  channel.queueDeclare(queueName, false, false, false, null);

            //发送消息给队列
            channel.basicPublish(exchangeName, routeKey, null, message.getBytes());

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

消费者

```java
package com.zhuang.rabbitmq.routing;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer
 * @Description 用一句话描述类的作用
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Consumer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

            channel.basicConsume("queue2", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("收到的消息是" + new String(delivery.getBody(), "UTF-8"));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.in.read();
            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



# 13，Direct模式

生产者

```java
package com.zhuang.rabbitmq.direct;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 消费者direct
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

        //    String queueName = "queue1";

            String message = "hello direct !!!";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */
            //准备交换机
            String exchangeName="direct_exchange";

            //定义路由Key
            String routeKey="";
            //定义交换机类型
            String type="direct";
          //  channel.queueDeclare(queueName, false, false, false, null);

            //发送消息给队列
            channel.basicPublish(exchangeName, routeKey, null, message.getBytes());

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

消费者

```java
package com.zhuang.rabbitmq.direct;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer
 * @Description direct消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Consumer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

            channel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("收到的消息是" + new String(delivery.getBody(), "UTF-8"));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.in.read();
            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



# 14，Topic模式

生产者

```java
package com.zhuang.rabbitmq.topics;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 生产者主题模式
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

        //    String queueName = "queue1";

            String message = "hello topics !!!";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */
            //准备交换机
            String exchangeName="topic_exchange";

            //定义路由Key
            String routeKey="com.order.user.test.xxxx";
            //定义交换机类型
            String type="topic";
          //  channel.queueDeclare(queueName, false, false, false, null);

            //发送消息给队列
            channel.basicPublish(exchangeName, routeKey, null, message.getBytes());

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

消费者

```java
package com.zhuang.rabbitmq.topics;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer
 * @Description 主题模式的消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Consumer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息

            channel.basicConsume("queue3", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("收到的消息是" + new String(delivery.getBody(), "UTF-8"));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.in.read();
            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_16-45-37.png)

![Snipaste_2021-04-15_16-46-06](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_16-46-06.png)

![Snipaste_2021-04-15_16-49-47](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_16-49-47.png)

![Snipaste_2021-04-15_16-50-18](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_16-50-18.png)

**以上图片仅供参考，需要自己创建交换机，指定路由key**



# 15，代码创建交换机

上面都是用web客户端创建的，现在用代码的方式创建交换机和定义队列！

```java
package com.zhuang.rabbitmq.all;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();

            //声明交换机名称
            String exchangeName="all_message_exchange";
            //交换机类型
            String exchangeType="direct";
            //定义一个交换机
            channel.exchangeDeclare(exchangeName,exchangeType,true);

            //声明队列
            channel.queueDeclare("queue5",true,false,false,null);
            channel.queueDeclare("queue6",true,false,false,null);
            channel.queueDeclare("queue7",true,false,false,null);

            //绑定队列和交换机的关系
            channel.queueBind("queue5",exchangeName,"order");
            channel.queueBind("queue6",exchangeName,"order");
            channel.queueBind("queue7",exchangeName,"order");
          //  String queueName = "queue1";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */
            String message = "hello kang xiao zhuang";
            //发送消息给队列
            channel.basicPublish(exchangeName,"order", null, message.getBytes());

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

我们可以看到web端创建了交换机和队列，关系也绑定上去了

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_13-19-13.png)

![Snipaste_2021-04-17_13-19-56](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_13-19-56.png)

![Snipaste_2021-04-17_13-20-20](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_13-20-20.png)

消费者代码

```java
package com.zhuang.rabbitmq.all;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.ArrayList;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer
 * @Description 代码创建交换机的消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Consumer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            channel.basicConsume("queue5", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("收到的消息是" + new String(delivery.getBody(), "UTF-8"));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.in.read();
            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

代码都是大同小异，主要是看用什么方式创建和定义交换机，队列



# 16，Work模式-轮询模式

当有多个消费者时，消息会被哪个消费者消费呢，主要有两种模式，如何均衡消费者消费的信息多少

1，轮询模式的分发：一个消费者一条，按均分配

2，公平分发：根据消费者的消费能力进行公平分发，处理快处理的多，慢处理的少，按劳分配！



**实战模拟**

```java
package com.zhuang.rabbitmq.work.lunxun;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 消费者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            String queueName = "queue1";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */
            channel.queueDeclare(queueName, true, false, false, null);
            for (int i = 0; i < 20; i++) {
                String message = "hello kang xiao zhuang-->轮询模式"+i;
                //发送消息给队列
                channel.basicPublish("", queueName, null, message.getBytes());
            }

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

两个消费者 同时运行看结果

```java
package com.zhuang.rabbitmq.work.lunxun;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer1
 * @Description 轮询模式的案例 消费者1
 * @Date 2021/4/17 13:33
 * @Created by dell
 */

public class Consumer1 {

    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            channel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("Consumer1号收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.out.println("Consumer1号开始接收消息");
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

第二个

```java
package com.zhuang.rabbitmq.work.lunxun;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer2
 * @Description 轮询模式的案例 消费者2
 * @Date 2021/4/17 13:33
 * @Created by dell
 */

public class Consumer2 {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            channel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("Consumer2号收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.out.println("Consumer2号开始接收消息");
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_13-49-05.png)

![Snipaste_2021-04-17_13-48-58](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_13-48-58.png)

**总结 ：不会因为服务器的性能而影响消费的多少，都是对等！**



# 17，Work模式-公平模式

改成手动应答机制，指标设置参数

```java
package com.zhuang.rabbitmq.work.fair;


import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Producer
 * @Description 生产者
 * @Date 2021/4/15 15:38
 * @Created by dell
 */

public class Producer {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            String queueName = "queue1";
            /*
            队列名称
            是否要持久化
            排他性
            是否自动删除
            携带附属参数
             */

            channel.queueDeclare(queueName, false, false, false, null);
            for (int i = 0; i < 20; i++) {
                String message = "hello kang xiao zhuang-->公平模式"+i;
                //发送消息给队列
                channel.basicPublish("", queueName, null, message.getBytes());
            }

            System.out.println("消息发送成功");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

```java
package com.zhuang.rabbitmq.work.fair;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer1
 * @Description 公平模式的案例 消费者1
 * @Date 2021/4/17 13:33
 * @Created by dell
 */

public class Consumer1 {

    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            //设定指标的值
            channel.basicQos(1);
            Channel finalChannel = channel;
            channel.basicConsume("queue1", false, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    try {
                        System.out.println("Consumer1号收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                        Thread.sleep(3000);
                        finalChannel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.out.println("Consumer1号开始接收消息");
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

```java
package com.zhuang.rabbitmq.work.fair;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Classname Consumer2
 * @Description 公平模式的案例 消费者2
 * @Date 2021/4/17 13:33
 * @Created by dell
 */

public class Consumer2 {
    public static void main(String[] args) {
        // 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.152.130");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");
        //创建Connection
        Connection connection = null;
        Channel channel = null;
        try {
            connection = connectionFactory.newConnection("生产者");
            //通过连接获得通道Channel
            channel = connection.createChannel();
            //通过交换机创建 声明队列 绑定关系 路由key 发送消息 和接收消息
            //设定指标的值
            channel.basicQos(1);
            Channel finalChannel = channel;
            channel.basicConsume("queue1", false, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    try {
                        System.out.println("Consumer2号收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                        Thread.sleep(100);
                        finalChannel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    System.out.println("接受失败...");
                }
            });
            System.out.println("Consumer2号开始接收消息");
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        } finally {
            if (channel != null && channel.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



# 18，RabbitMQ应用场景

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_14-26-09.png)

![Snipaste_2021-04-17_14-27-07](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_14-27-07.png)

![Snipaste_2021-04-17_14-28-53](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_14-28-53.png)

**好处：**

- 完全解耦，用MQ建立桥接
- 有独立的线程池和运行模型
- 出现了消息可能会丢失，MQ有持久化功能
- 如何保证消息的可靠性，死信队列和消息转移功能
- 如果服务器承载不了，你需要自己去写高可用，HA镜像模型高可用

按照上面约定，用户的响应时间相当于是订单信息的写入数据库的时间，也就是50毫秒，注册邮件，发送消息写入消息队列后，直接返回，因此写入消息队列的速度很快，基本可以忽略，架构改变后，比串行提高3倍，并行提高2倍

![Snipaste_2021-04-17_14-30-05](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_14-30-05.png)

![Snipaste_2021-04-17_14-30-21](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_14-30-21.png)

场景

- 流量的削峰
- 分布式事务的可靠消费和可靠生产
- 索引，缓存，静态化处理的数据同步
- 流量监控
- 日志监控
- 下单，订单分发，抢票



# 19，SpringBoot整合RabbitMQ

在Spring项目中，可以使用Spring-Rabbit去操作RabbitMQ
https://github.com/spring-projects/spring-amqp

尤其是在spring boot项目中只需要引入对应的amqp启动器依赖即可，方便的使用RabbitTemplate发送消息，使用注解接收消息。



*一般在开发过程中*：

**生产者工程：**

1. application.yml文件配置RabbitMQ相关信息；
2. 在生产者工程中编写配置类，用于创建交换机和队列，并进行绑定

3. 注入RabbitTemplate对象，通过RabbitTemplate对象发送消息到交换机



**消费者工程：**

1. application.yml文件配置RabbitMQ相关信息

2. 创建消息处理类，用于接收队列中的消息并进行处理

新建一个模块 导入相关依赖

```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-rabbit-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

编写yml文件

```yml
server:
  port: 8080

#rabbitmq服务
spring:
  rabbitmq:
    username: admin
    password: admin
    virtual-host: /
    host: 192.168.152.130
    port: 5672
```



创建业务层

```java
package com.zhuang.springboot_rabbitmq.service;

import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.UUID;

/**
 * @Classname OrderService
 * @Description 业务层
 * @Date 2021/4/17 14:46
 * @Created by dell
 */
@Service
public class OrderService {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    //模拟用户下单
    public void makeOrder(String userid,String productid,int num) {
        //根据商品id查询库存是否充足
        String orderId = UUID.randomUUID().toString();
        //保存订单
        System.out.println("订单生产成功"+orderId);

        //通过MQ来完成消息的分发
        String exchangeName="fanout_order_exchange";
        String routingKey="";
        rabbitTemplate.convertAndSend(exchangeName,routingKey,orderId);
    }
}
```

编写配置类 定义交换机和队列

```java
package com.zhuang.springboot_rabbitmq.config;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.FanoutExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


/**
 * @Classname RabbitMQConfiguration
 * @Description rabbitmq的配置类
 * @Date 2021/4/17 14:53
 * @Created by dell
 */
@Configuration
public class RabbitMQConfiguration {

    //声明注册fanout模式的交换机
    @Bean
    public FanoutExchange fanoutExchange() {
        return new FanoutExchange("fanout_order_exchange", true, false);
    }

    //声明队列sms.fanout.queue email.fanout.queue duanxin.fanout.queue
    @Bean
    public Queue smsQueue() {
        return new Queue("sms.fanout.queue", true);
    }

    @Bean
    public Queue duanxinQueue() {
        return new Queue("duanxin.fanout.queue", true);
    }

    @Bean
    public Queue emailQueue() {
        return new Queue("email.fanout.queue", true);
    }

    //完成绑定关系
    @Bean
    public Binding smsBinding() {
        return BindingBuilder.bind(smsQueue()).to(fanoutExchange());
    }

    @Bean
    public Binding duanxinBinding() {
        return BindingBuilder.bind(duanxinQueue()).to(fanoutExchange());
    }

    @Bean
    public Binding emailBinding() {
        return BindingBuilder.bind(emailQueue()).to(fanoutExchange());
    }
}
```

创建三个消费者 类上添加注解即可

```java
package com.zhuang.springboot_rabbitmq.service.fanout;

import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Classname FanoutDuanXinConsumer
 * @Description FanoutDuanXinConsumer
 * @Date 2021/4/17 15:06
 * @Created by dell
 */
@Component
@RabbitListener(queues = {"duanxin.fanout.queue"})
public class FanoutDuanXinConsumer {

    @RabbitHandler
    public void receiveMessage(String message){
        System.out.println("dunaxin fanout --- 接受到了订单消息是：->"+ message);
    }
}
```

```java
package com.zhuang.springboot_rabbitmq.service.fanout;

import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Classname FanoutEmailConsumer
 * @Description FanoutEmailConsumer
 * @Date 2021/4/17 15:06
 * @Created by dell
 */
@Component
@RabbitListener(queues = {"email.fanout.queue"})
public class FanoutEmailConsumer {
    @RabbitHandler
    public void receiveMessage(String message){
        System.out.println("email fanout --- 接受到了订单消息是：->"+ message);
    }
}
```

```java
package com.zhuang.springboot_rabbitmq.service.fanout;

import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Classname FanoutSMSConsumer
 * @Description FanoutSMSConsumer
 * @Date 2021/4/17 15:06
 * @Created by dell
 */
@Component
@RabbitListener(queues = {"sms.fanout.queue"})
public class FanoutSMSConsumer {
    @RabbitHandler
    public void receiveMessage(String message){
        System.out.println("sms fanout --- 接受到了订单消息是：->"+ message);
    }
}
```

运行测试

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-17_15-13-10.png)

**看到消息被创建而且被消费**



# 20，RabbitMQ模式总结

RabbitMQ工作模式：
**1、简单模式 HelloWorld**
一个生产者、一个消费者，不需要设置交换机（使用默认的交换机）

**2、工作队列模式 Work Queue**
一个生产者、多个消费者（竞争关系），不需要设置交换机（使用默认的交换机）

**3、发布订阅模式 Publish/subscribe**
需要设置类型为fanout的交换机，并且交换机和队列进行绑定，当发送消息到交换机后，交换机会将消息发送到绑定的队列

**4、路由模式 Routing**
需要设置类型为direct的交换机，交换机和队列进行绑定，并且指定routing key，当发送消息到交换机后，交换机会根据routing key将消息发送到对应的队列

**5、通配符模式 Topic**
需要设置类型为topic的交换机，交换机和队列进行绑定，并且指定通配符方式的routing key，当发送消息到交换机后，交换机会根据routing key将消息发送到对应的队列



# 21，RabbitMQ高级特性

## 21.1 消息的可靠投递

在使用 RabbitMQ 的时候，作为消息发送方希望杜绝任何消息丢失或者投递失败场景。RabbitMQ 为我们提供了两种方式用来控制消息的投递可靠性模式。

- confirm 确认模式

- return 退回模式



rabbitmq 整个消息投递的路径为：

producer--->rabbitmq broker--->exchange--->queue--->consumer

- 消息从 producer 到 exchange 则会返回一个 confirmCallback 。

- 消息从 exchange-->queue 投递失败则会返回一个 returnCallback 。

我们将利用这两个 callback 控制消息的可靠性投递



## 21.2 Consumer Ack

ack指Acknowledge，确认。 表示消费端收到消息后的确认方式。

有三种确认方式：

- 自动确认：acknowledge="none"

- 手动确认：acknowledge="manual"

- 根据异常情况确认：acknowledge="auto"）



其中自动确认是指，当消息一旦被Consumer接收到，则自动确认收到，并将相应 message 从 RabbitMQ 的消息缓存中移除。但是在实际业务处理中，很可能消息接收到，业务处理出现异常，那么该消息就会丢失。如果设置了手动确认方式，则需要在业务处理成功后，调用channel.basicAck()，手动签收，如果出现异常，则调用channel.basicNack()方法，让其自动重新发送消息。



## 21.3 TTL

- TTL 全称 Time To Live（存活时间/过期时间）。

- 当消息到达存活时间后，还没有被消费，会被自动清除。

- RabbitMQ可以对消息设置过期时间，也可以对整个队列（Queue）设置过期时间

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-18_13-52-32.png)

**小结**

- 设置队列过期时间使用参数：x-message-ttl，单位：ms(毫秒)，会对整个队列消息统一过期。

- 设置消息过期时间使用参数：expiration。单位：ms(毫秒)，当该消息在队列头部时（消费时），会单独判断这一消息是否过期。

- 如果两者都进行了设置，以时间短的为准。



## 21.4 死信队列

死信队列，英文缩写：DLX 。Dead Letter Exchange（死信交换机），当消息成为Dead message后，可以被重新发送到另一个交换机，这个交换机就是DLX。

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-18_13-54-09.png)

**消息成为死信的三种情况：**

1. 队列消息长度到达限制；

2. 消费者拒接消费消息，basicNack/basicReject,并且不把消息重新放入原目标队列,requeue=false；

3. 原队列存在消息过期设置，消息到达超时时间未被消费；



**队列绑定死信交换机：**

给队列设置参数： x-dead-letter-exchange 和 x-dead-letter-routing-key

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-18_13-55-53.png)



## 21.5 日志监控

配置详解见

https://www.rabbitmq.com/configure.html

### 21.5.1 **rabbitmqctl管理和监控**

```shell
查看队列
# rabbitmqctl list_queues

查看exchanges
# rabbitmqctl list_exchanges

查看用户
# rabbitmqctl list_users

查看连接
# rabbitmqctl list_connections

查看消费者信息
# rabbitmqctl list_consumers

查看环境变量
# rabbitmqctl environment

查看未被确认的队列
# rabbitmqctl list_queues  name messages_unacknowledged

查看单个队列的内存使用
# rabbitmqctl list_queues name memory

查看准备就绪的队列
# rabbitmqctl list_queues name messages_ready
```



# 22，RabbitMQ应用问题

1. 消息可靠性保障

- 消息补偿机制

2. 消息幂等性保障

- 乐观锁解决方案



## 22.1 **消息可靠性保障**

**消息补偿**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-18_13-59-14.png)

## 22.2 消息幂等性保障

幂等性指一次和多次请求某一个资源，对于资源本身应该具有同样的结果。也就是说，其任意多次执行对资源本身所产生的影响均与一次执行的影响相同。

在MQ中指，消费多条相同的消息，得到与消费该消息一次相同的结果。

**乐观锁机制**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-18_14-00-32.png)

# 23，RabbitMQ集群搭建 

此章节后续补上！