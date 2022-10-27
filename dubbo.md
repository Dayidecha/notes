# Dubbo



## 一、基础知识

### 互联网架构演变趋势

框架：ORM - MVC - PRC -SOA

单一应用架构 - RPC架构 - SOA - 微服务

+ 单一应用架构：

  优点：部署简单（打包成一个jar包或war包、项目易于管理

  缺点：团队协作困难、测试成本高、可伸缩性差、可靠性差、迭代困难

+ RPC架构：

  特点：应用直接调用服务，服务之间是隔离的

  缺点：服务过多，管理成本高。服务治理，服务容错，ip暴露

  

+ SOA架构：面向幕府架构
  + ESB(Enterparise Service Bus):企业服务总线，服务中介
  + ESB包含功能：负载均衡，流量控制，加密处理，服务监控，异常处理，监控告急等等。
+ 微服务架构
  + 微服务是一种架构风格。一个大型的复杂软件应用，由一个或多个微服务组成。系统中的各个微服务可被独立部署，各个微服务之间是松耦合的。每个微服务只关注于完成一件任务。微服务就是一个轻量级的服务治理方案，对比SOA架构，使用注册中心代替ESB服务总线。注册中心更轻量级，代表技术：`springcloud`,`dubbo`等等
  + 架构风格：项目的一种设计模式，常见的有：
    + 客户端与服务端的：如QQ
    + 基于组件模型的架构：如EJB
    + 分层架构（单体架构）：如MVC
    + 面向服务架构：ESB,ZOOKEEPER，eruka(springcloud)，nacos(springcloud alibaba)
  + 特点：服务内部高内聚，服务之间松耦合
  + 优点：测试容易、可伸缩性强、团队协作灵活，系统迭代容易
  + 缺点：运维成本高、接口兼容多版本、分布式系统复杂、**分布式事务**

### RPC基于RMI的简单实现

**RPC基本介绍**

remote procedure call：远程过程调用

RPC两个核心：通信效率、序列化效率

**RPC框架**

`JAVA RMI` `Dubbo` `Thrift` `grpc`等

**RPC与HTTP、TPC/UDP、Socket的区别**

Socket：是在应用程序层面上对TCP/IP协议的封装和应用。其实是一个调用接口，方便程序员使用TCP/IP协议栈而已。程序员通过socket来使用tcp/ip协议。但是socket并不是一定要使用tcp/ip协议，Socket编程接口在设计的时候，就希望也能适应其他的网络协议。
RPC：是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。所以RPC的实现可以通过不同的协议去实现比如可以使http、RMI等。

**为什么需要RPC**

论复杂度，RPC框架肯定是高于简单的HTTP接口的。但毋庸置疑，HTTP接口由于受限于HTTP协议，需要带HTTP请求头,导致传输起来效率或者说安全性不如RPC。

http接口是在接口不多、系统与系统交互较少的情况下，解决信息初期常使用的一种通信手段;优点就是简单、直接、开发方便。利用现成的http协议进行传输。但是如果是一个大型的网站，内部子系统较多、接口非常多的情况下，RPC框架的好处就显示出来了，首先就是长链接，不必每次通信都要像http一样去3次握手什么的，减少了网络开销(这个问题在http2.0已经被解决不再算是问题了);其次就是RPC框架一般都有注册中心，有丰富的监控管理;发布、下线接口、动态扩展等，对调用方来说是无感知、统一化的操作。第三个来说就是安全性。最后就是流行的服务化架构、服务化治理，RPC框架是一个强力的支撑。

RPC是一种概念，http也是RPC实现方式的一种



### Dubbo介绍

Dubbo是由阿里巴巴开源的一个高性能、基于Java开源的远程调用框架。正如在许多RPC系统中一样，Dubbo是基于定义服务的概念，指定可以通过参数和返回类型远程调用的方法。在服务器端，服务器实现这个接口，并运行一个Dubbo服务器来处理客户端调用。在客户端，客户机有一个存根，它提供与服务器相同的方法。

Dubbo提供三个核心功能:基于接口的远程调用、容错和负载均衡，以及服务的自动注册与发现。Dubbo框架广泛的在阿里巴巴内部使用，以及京东、当当、去哪儿、考拉等都在使用。

## 二、dubbo配置

1.引入依赖



provider方：

2.application中开启dubbo

```java
@SpringBootApplication
//开启dubbo
@EnableDubboConfiguration
public class DubboProviderApplication {
    public static void main(String[] args){
        ConfigurableApplicationContext run = SpringApplication.run(DubboProviderApplication.class,args);
    }
}

```



3.在application.yml中配置dubbo

```yml
server:
  port: 8001
spring:
  application:
    name: dubbo
  dubbo:
    #开启dubbo
    server: true
    #应用信息
    application:
      name: provider
    #注册中心地址
    registry:
      address: multicast://224.5.6.7:1234
    protocol:
      #协议名
      name: dubbo
      #协议端口
      port: 20880
    #扫描包位置
    scan: com.rmi.provider.service
```



4.service中暴露服务

```java
@Service(interfaceClass = UserServicel.class)
```





consumer方：

1.配置

```yaml
server:
  port: 8002
spring:
  application:
    name: dubbo
  dubbo:
    #应用信息
    application:
      name: consumer
    #注册中心地址
    registry:
      address: multicast://224.5.6.7:1234
    protocol:
      #协议名
      name: dubbo
      #协议端口
      port: 20880
    #扫描包位置
    scan: com.rmi.consumer
```



2.消费

```java
@Component
public class UserInit implements CommandLineRunner {

    @Reference(interfaceClass = UserServicel.class)
    private UserServicel userServicel;
    @Override
    public void run(String... args) throws Exception {
        System.out.println(userServicel.selectUserById(2));

    }
}
```



### dubbo-admin

+ 从注册中心获取所有提供者、消费者进行配置管理
+ 路由规则、动态配置、服务降级、访问控制、权重调整】负载均衡等管理功能



**安装**

https://github.com/apache/dubbo-admin/tree/0.2.0下载

**修改配置文件**

dubbo-admin-0.2.0\dubbo-admin-server\src\main\resources

```properties
# centers in dubbo2.7
admin.registry.address=zookeeper://101.34.9.213:2181
admin.config-center=zookeeper://101.34.9.213:2181
admin.metadata-report.address=zookeeper://101.34.9.213:2181

```

**构建mvn项目**

到根目录执行`mvn clean package`

构建完去`C:\Users\y\Desktop\dubbo-admin-develop\dubbo-admin-distribution\target`

执行`java -jar dubbo-admin-0.3.0-SNAPSHOT.jar`

到`C:\Users\y\Desktop\dubbo-admin-develop\dubbo-admin-ui`执行

`npm run dev`

## 三、高可用







## 四、dubbo原理





## 五、Dubbo高级特性

### 序列化

序列化的对象要实现`Serializable`接口

dubbo已经将序列化和反序列化的过程内部封装了



### 地址缓存

注册中心挂了，服务是否可以正常访问？

+ 可以，因为dubbo服务消费者第一次调用时，会将服务提供方的地址缓存到本地，在以后调用则不会访问注册中心
+ 当服务提供者地址发生变化时，注册中心会通知服务消费者。



### 超时与重试

+ 服务消费者在调用服务提供者的时候发生了阻塞、等待等情形。

+ 在某个峰值时刻，大量请求同时请求服务消费者，造成线程大量堆积，势必会造成雪崩。

+ dubbo利用超时机制，设置一个超时时间，在这个时间段内，无法完成服务访问，自动断开连接，

  dubbo使用`timeout`设置超时时间，默认为1000ms。

+ 建议配置在服务提供者上面`@Service(timeout = 3000,retries = 0)`

+ 如果网络抖动，一次请求会失败，Dubbo用重试机制避免这个问题,用`retries`来设置,默认为2

### 多版本

+ 灰度发布：当出现新功能时，会让一部分用户先使用新功能，用户反馈没问题时，再将所有的用户迁移到新功能。

+ dubbo使用version属性来设置和调用同一个接口的不同版本,`@Service(version = "v1.0")`

  消费方`@Reference(version = "v1.0")`

### 负载均衡策略

`@Service(weight = 100)`

+ Random:按权重随机`@Reference(loadbalance = “random”)`
+ RoundRobin:按权重轮询
+ LeastActive:最少活跃调用次数
+ ConsistentHash:一致性Hash，相同参数的请求总是发到同一提供者

### 集群容错

集群容错模式：

+ Failover Cluster:失败重试。默认值，当失败时，重试其他服务器，默认重试2次，使用`retries`配置，一般用于读操作`@Reference(cluster = "failover")`
+ FailFast Cluster:快速失败，只发起一次调用，失败立即报错，通常用于写操作。
+ FailSafe Cluster:安全失败，出现异常时，直接忽略。返回一个空结果，通常用于日志操作。
+ Failback Cluster:失败自动回复，后台记录失败请求，定时重发。
+ Forking Cluster:并行调用多个服务器，只要一个成功即可返回。
+ Broadcast Cluster:广播调用所有提供者，逐个调用，任意一台报错则报错。

### 服务降级

服务降级方式

`@Reference(mock = "force:return null")`

+ mock = force:return null 表示消费方对该服务的方式调用都直接返回null,不发起远程调用。用来屏蔽不重要的服务不可用时对调用方的影响。
+ mock = fail:return null 表示消费方对该服务的方法调用在失败后，返回null值，不抛出异常。用来容忍不重要的服务在不稳定时对调用方的影响。

# ZooKeeper

zookeeper是一个分布式的，开源的分布式应用程序协调服务，从设计模式的角度来理解就是观察者设计模式的分布式管理架构（发布订阅）

功能：为一个分布式应用服务提供一致性服务，包括：统一命名服务，统一配置管理，统一集群管理，服务节点动态上下线，负载均衡等。

简而言之：分布式应用可以在zookeeper上注册即可实现集群中的主从管理模式。



## 1.zookeeper中的内部原理

### 1.1. 选举机制

半数机制：集群中半数以上的机器alive则集群可用，所有zookeeper更适合安装在奇数台机器上，并且不同于其他框架的主从机制，zookeeper并没有在配置中节点leader和follower的具体对象，而是启动时通过内部选举产生一个leader。

​	Server一共有三种状态：

+ `LOOKING`：server不知道谁leader，选举中
+ `LEADING`
+ `FOLLOWING`

总结：过半当leader，后来的当follower，如5台机器，第三台是leader



### 2.2. paxos算法

paxos是一个基于消息传递的一致性算法



小岛（Island)一一 ZK Server Cluster

议员（Senator)一一 ZK Server

提议（Proposal)一一 Znode Change(Create/Delete/SetData..)

提议编号(PID)一一 Zxid(Zookeeper Transaction id)

正式法令（PID)一一所有ZNode及其数据



## 2. zookeeper快速入门



安装

```shell
wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.5.9/zookeeper-3.5.9.tar.gz
wget https://mirrors.huaweicloud.com/zookeeper/zookeeper-3.5.9.tar.gz
tar -zxvf zookeeper-3.4.13.tar.gz -C /usr/local/zookeeper
```



配置

```shell
#zookeeper启动默认会找zoo.cfg
cp zoo_sample.cfg zoo.cfg

cd ..
mkdir data
mkdir log

vim zoo.cfg
dataDir=/urs/local/....刚才新建的目录
logDir=...
```



启动

```shell
cd bin/ 
./zkServer.sh start
#查看状态
./zkServer.sh status
##Mode:standalone

#停止服务
./zkServer.sh stop
```



java中改application.yml

```yaml
address: zookeeper://192.168.10.110:2181
```



# RabbitMQ



**MQ简介**

在计算机科学中，MQ是一种进程间通信或同一进程的不同线程之间通信的方式。消息队列提供了异步通信协议，每一个伫列中的记录包含详细的说明数据，包括发生时间，输入的设备种类，以及特定的输入参数，也就是说：消息的发送者和接收者不需要同时和消息队列交互，消息会保存在队列中，直到接受者取回它。

**实现**

当前使用较多的有：`RabbitMQ`、`RocketMQ`、`Kafka`、`Redis`、`Mysql`

**特点**

MQ是消费者-生产者模型的一个典型代表,和JMS类似。

MQ是遵循AMQP协议的具体实现和产品

> AMQP，即Advanced Message Queuing Protocol，一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可以传递消息，并不收客户端、中间件不同产品，不同的开发语言等条件限制。



> JMS，即java消息服务应用程序接口，是一个Java平台中关于面向消息中间件的API，用户在两个应用程序之间，或者分布式系统中发送消息，进行异步通信，绝大数的MOM提供商都提供了对JMS的支持，常见的消息队列，大部分都实现了JMS API，如`Redis`、`ActiveMQ`和`RabbitMQ`等。

**优点：**

异步处理、应用解耦、流量削峰



**使用场景：**

当不需要立即获得结果，但并发量又需要进行控制的时候





## 安装

```shell
vim /etc/yum.repos.d/rabbitmq-erlang.repo
```



```
[rabbitmq_erlang]
name=rabbitmq_erlang
baseurl=https://packagecloud.io/rabbitmq/erlang/el/7/$basearch
repo_gpgcheck=1
gpgcheck=0
enab1ed=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[rabbitmq_erlang-source]
name=rabbitmq_erlang-source
baseurl=https://packagec1oud.io/rabbitmq/erlang/el/7/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300



[rabbitmq_erlang]
name=rabbitmq_erlang
baseurl=https://packagecloud.io/rabbitmq/erlang/el/7/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
 
[rabbitmq_erlang-source]
name=rabbitmq_erlang-source
baseurl=https://packagecloud.io/rabbitmq/erlang/el/7/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
```



然后清除yum缓存，重新创建缓存

```shell
yum clean all
yum makecache
```



> 上面的忽略



1.下载erlang包~

2.解压

```shell
tar -xvf otp_src_22.0.tar.gz
```

3.安装运行环境

```shell
yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel
yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel unixODBC-devel
```

4.进入目录并设置安装规则

```shell
cd otp_src_22.0

./configure --prefix=/usr/local/erlang --with-ssl --enable-threads --enable-smp-support --enable-kernel-poll --enable-hipe --without-javac
```

5.安装

```shell
make 
```

6.配置环境变量

```shell
export ERL_PATH=/usr/local/erlang/bin
export PATH=$ERL_PATH:$PATH

source /etc/profile  使配置生效,在shell中使用(临时有效)
```



安装rabbitMQ

```shell
wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.7.12/rabbitmq-server-3.7.12-1.el7.noarch.rpm
wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.8.2/rabbitmq-server-3.8.2-1.el7.noarch.rpm
wget www.rabbitmq.com/releases/rabbitmq-server/v3.7.12/rabbitmq-server-3.7.12-1.el7.noarch.rpm
或
yum install -y rabbitmq-server-3.7.12-1.el7.noarch.rpm
```



```shell
安装Ui插件
rabbitmq-plugins enable rabbitmq_management

systemctl start rabbitmq-server.service
```



> docker安装了了

### **rabbitmq忘记控制台密码**



**第一步：进入docker容器**

`docker exec -it myrabbit bash`

**第二步：添加用户**

`rabbitmqctl add_user user 123456 `

**第三步：赋予user权限和角色**

`rabbitmqctl set_permissions -p / user ".*" ".*" ".*"`

`rabbitmqctl set_user_tags user administrator`

**第四步：查看当前用户列表**

`rabbitmqctl  list_users  `

**第五步：修改密码**

`rabbitmqctl  change_password  Username  'Newpassword'`

## 使用

添加依赖

```xml
    <dependency>
      <groupId>com.rabbitmq</groupId>
      <artifactId>amqp-client</artifactId>
      <version>5.6.0</version>
    </dependency>
```

```java
/*channel.queueDeclare声明队列
第一个参数queue:队列名称
第二个参数durable:是否持久化
第三个参数Exclusive:排他队列，如果一个队列被声明为排他队列，该队列仅对首次声明它的连接可见,并在连接断开时自动删除。

这里需要注意三点:

1．排他队列是基于连接可见的，同一连接的不同通道是可以同时访问同一个连接创建的排
他队列的。

2．"首次"，如果一个连接已经声明了一个排他队列，其他连接是不允许建立同名的排他队
列的,这个与普通队列不同。

3．即使该队列是持久化的，一旦连接关闭或者客户端退出，该排他队列都会被自动删除
的。

这种队列适用于只限于一个客户端发送读取消息的应用场景。
第四个参数Auto-delete:自动删除，如果该队列没有任何订阅的消费者的话，该队列会被自动删
除。

这种队列适用于临时队列。
*/
   
//创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("101.34.9.213");
        factory.setPort(5672);
        factory.setUsername("shop1");
        factory.setPassword("shop_");
        factory.setVirtualHost("/");
        try (
                //通过工厂建立连接
                Connection connection = factory.newConnection();
                //获取通道
             Channel channel = connection.createChannel()) {

            channel.queueDeclare(QUEUE_NAME, false, false, false, null);
            String message = "Hello World!";
            channel.basicPublish("", QUEUE_NAME, null, message.getBytes(StandardCharsets.UTF_8));
            System.out.println(" [x] Sent '" + message + "'");
        }
```





消费者

```java
/**
 * 工作队列-公平模式
 */
public class Recv02 {

    //private final static String QUEUE_NAME = "work_rr";
	private final static String QUEUE_NAME = "work_fair";
    
    public static void main(String[] argv) throws Exception {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("101.34.9.213");
        factory.setPort(5672);
        factory.setUsername("shop1");
        factory.setPassword("shop_");
        factory.setVirtualHost("/");
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();

        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        System.out.println(" [*] Waiting for messages. To exit press CTRL+C");
        /*
        限制rabbitmq只发不超过一条的信息给同一个消费者
        当消息处理完毕后，有了反馈，才会进行第二次发送
         */
        int perferchCount = 1;
        channel.basicQos(perferchCount);
        //获取消息
        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };
        //监听队列
        channel.basicConsume(QUEUE_NAME, false, deliverCallback, consumerTag -> { });
    }
}
```



工作模式：

+ 轮询：`work_rr`
+ 公平：`work_fair`

发布/订阅模式：`exchange_fanout`

路由模式：`exchange_direct`

主题模式：`*`匹配一个单词,`#`匹配0多个单词

# Docker

**为什么选择Docker?**

+ 更高效的利用系统资源

  由于容器不需要进行硬件虚拟以及运行完整的操作系统等额外开销，Docker对系统资源利用率更高

  

+ 更快速的启动时间

  传统的虚拟机技术启动应用服务往往需要数分钟，而Docker容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。

+ —致的运行环境

  开发过程中一个常见的问题是环境一致性问题。由于开发环境、测试环境、生产环境不一致，导致有些bug并未在开发过程中被发现。而Docker的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性，从而不会再出现「这段代码在我机器上没问题啊」这类问题。

+ 持续交付和部署

  对开发和运维(DevOps）人员来说，最希望的就是一次创建或配置，可以在任意地方正常运行。
  使用Docker可以通过定制应用镜像来实现持续集成、持续交付、部署。开发人员可以通过Dockerfile来进行镜像构建，并结合持续集成(Continuous Integration)系统进行集成测试，而运维人员则可以直接在生产环境中快速部署该镜像，甚至结合持续部署(Continuous Delivery/Deployment)系统进行自动部署。

  而且使用Dockerfile使镜像构建透明化，不仅仅开发团队可以理解应用运行环境，也方便运维团队理解应用运行所需条件，帮助更好的生产环境中部署该镜像。

+ 更轻松的迁移

  由于Docker确保了执行环境的一致性，使得应用的迁移更加容易。Docker 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。因此用户可以很轻易的将在一个平台上运行的应用，迁移到另一个平台上，而不用担心运行环境的变化导致应用无法正常运行的情况。

+ 更轻松的维护和扩展

  Docker使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单，基于基础镜像进一步扩展镜像也变得非常简单。此外，Docker团队同各个开源项目团队一起维护了一大批高质量的官方镜像，既可以直接在生产环境使用，又可以作为基础进一步定制，大大的降低了应用服务的镜像制作成本



## Docker组件

**Docker服务器与客户端**

Docker是一个客户端-服务器(C/S）架构程序。Docker客户端只需要向Docker服务器或者守护进程发出请求,服务器或者守护进程将完成所有工作并返回结果。Docker提供了一个命令行工具Docker以及一整套RESTful API。你可以在同一台宿主机上运行Docker守护进程和客户端，也可以从本地的Docker客户端连接到运行在另一台宿主机上的远程Docker守护进程。

**Docker镜像与容器**

镜像是构建Docker的基石。用户基于镜像来运行自己的容器。镜像也是Docker生命周期中的"构建"部分。镜像是基于联合文件系统的一种层式结构，由一系列指令一步一步构建出来。例如:

+ 添加一个文件;
+ 执行一个命令;

+ 打开一个窗口。

也可以将镜像当作容器的“源代码"”。镜像体积很小，非常“便携”，易于分享、存储和更新。

Docker可以帮助你构建和部署容器，你只需要把自己的应用程序或者服务打包放进容器即可。容器是基于镜像启动起来的，容器中可以运行一个或多个进程。我们可以认为，镜像是Docker生命周期中的构建或者打包阶段，而容器则是启动或者执行阶段。容器基于镜像启动，一旦容器启动完成后，我们就可以登录到容器中安装自己需要的软件或者服务。

所以Docker容器就是：

+ 一个镜像格式
+ 一系列标准操作
+ 一个执行环境



**Registry（注册中心）**

Docker用Registry来保存用户构建的镜像。Registry分为公共和私有两种。Docker公司运营公共的Registry叫做Docker Hub。用户可以在Docker Hub注册账号，分享并保存自己的镜像(说明:在Docker Hub下载镜像巨慢，可以自己构建私有的Registry) 。



## 安装Docker

1.更新yum

``yum update`



2.安装软件包 yum-util提供yum-config-manager功能，另外两个是devicemapper的驱动依赖

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```



3.设置yum源为阿里云

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



4.安装docker

```shell
yum install -y docker-ce
```



5.查看docker 版本

```shell
docker -v
```



6.设置ustc镜像

ustc是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.04版本的时候就在用。ustc的docker镜像加速器速度很快。ustc docker mirror的优势之一就是不需要注册，是真正的公共服务。

```shell
mkdir -p /etc/docker
vim /etc/docker/daemon.json
```

```json
//加入
{
"registry-mirrors " : ["https : / /docker.mirrors.ustc.edu.cn"]
}
```



## 使用docker

**启动docker**

```shell
systemctl start docker
systemctl status docker
systemctl status docker
systemctl restart docker

systemctl enable docker #开机默认启动docker
```



**常用命令**

```shell
docker info
docker --help
```

### **镜像相关命令**

**查看镜像**

```shell
docker images
```

+ `REPOSITORY` 镜像名称
+ `TAG` 标签
+ `IMAGE ID` 镜像ID
+ `CREATED` 镜像创建日期
+ `SIZE` 镜像大小

这些镜像都是存储在Docker宿主机的`/var/lib.docker`目录下



**搜索镜像**

```shelll
docker search name
```

+ `NAME`
+ `DESCRIPTION`
+ `STARS`
+ `OFFICIAL`
+ `AUTOmATED` 自动构建，表示该镜像由Docker Hub自动构建流程创建的

**拉取镜像**

```
docker pull name:7 #版本
```

**删除镜像**

```
docker rmi 镜像id
```

删除所有镜像

```
docker rmi `docker images -q`
```



### 容器相关命令

**查看正在运行的容器**

```
docker ps
```

**查看所有容器**

```
docker ps -a
```

**查看最好一次运行的容器**

```
docker ps -l
```

**查看停止运行的容器**

```
docker ps -f status=exited
```



**创建和启动容器**

```
docker run
```

`-i` 表示运行容器

`-t` 表示容器启动后进入其命令行，即分配一个伪终端

`--name` 为创建的容器命名

`-v` 表示目录映射关系（前者是宿主的战绩目录，后者是映射到宿主主机上的目录

`-d`  在run后加-d会创建一个守护式容器在后台运行

`-p` 表示端口映射，前者是宿主主机端口，后者是容器内映射端口



> 加了 **-d** 参数默认不会进入容器，想要进入容器需要使用指令 **docker exec**（下面会介绍到）。

**交互方式创建容器**

```
docker run -itd --name redis-test -p 6379:6379 redis
docker exec -it redis-test /bin/bash
```

**停止容器**

```
docker stop 容器id
```

**把宿主机的文件拷贝进容器里/反着类似**

```
docker cp 文件 容器id:/xxx/xxx
```

```
docker cp 容器name:/xxx/xxx/文件名 /xxx/xxx/文件名
```

**目录挂载**

```
docker run -di -v /usr/loca1/myhtm1:/usr/loca1/myhtm1 --name=mycentos3 centos:7
```

**查看容器状态和参数**

```
docker inspect 容器名
```



**删除容器**

```
docker rm 容器id
```



### 安装和使用rabbitmq

```
docker pull rabbitmq:3.8.5

docker run -di --name=rabbitmq -p 5671:5671 -p 5672:5672 -p 25672:25672 -p 4369:4369 -p 15671:15671 -p 15672:15672 rabbitmq:3.8.5


docker exec -it rabbitmq /bin/bash
rabbitmq-plugins enable rabbitmq_management
```

```
docker run -di --name rabbitmq -p 15672:15672 -p 5672:5672 rabbitmq:3.8.5

```

