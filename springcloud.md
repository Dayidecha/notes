### 

学习视频：https://www.bilibili.com/video/BV18E411x7eT?p=9

学习笔记：https://blog.csdn.net/qq_41211642/article/details/104772140

java：程序包org.xxxx不存在：https://blog.csdn.net/lanben886/article/details/106622900/



### Maven依赖下载不到问题

https://www.jb51.net/article/190812.htm

### springcloud整合的主流技术架构

服务注册与发现 Consul/Zookeeper/nacos

服务调度均衡和调度 Ribbon/LoadBalancer/apollo

服务调用2 Feign(快不行了)/openFeign

服务熔断降级 Hystrix(快挂)/sentinel(阿里，强烈推荐)

服务网关 Zuul（快挂了）/gateway（主流）

服务分布式配置 SpringCloud Config（不太行）/apollo/Nacos

服务总线 bus(不太行)/nacos

服务开发 springboot



### Spring Cloud 和 SpringBoot 版本选择

https://spring.io/projects/spring-cloud 查看overview下面有个版本对应表

或者 start.spring.io/actuator/info



### 项目构建

约定>配置>编码

setting - file Encodings - 三个选项都选utf-8

setting - build,excution.. - compiler - Annotation Processors - ✔ enable annotation 使注解生效

setting - build,excution.. - compiler - java Compiler 1.5 改 1.8



写微服务模块的流程

1. 建module
2. 改POM
3. 写YML
4. 主启动
5. 业务类



业务类流程：

1. 建表sql
2. entities
3. dao
4. service 
5. controller



### RestTemplate



使用 xxxconfig.java

```java
@Configuration
public class ApplicationContextConfig {

    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```



## Eureka

### Eureka基础知识

Eureka提供两个组件：Eureka Server和Eureka Client

**Server提供服务注册服务**

各个微服务节点通过配置启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

**Client通过注册中心进行访问**

是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒)



**server使用**

1. 导入依赖

```xml
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
      <version>3.0.2</version>
    </dependency>
```

2. 改yml

   ```yml
   server:
     port: 7001
   eureka:
     instance:
       hostname: localhost #eureka服务端的实例名称
     client:
       #false表示不向注册中心注册自己
       register-with-eureka: false
       #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
       fetch-registry: false
       service-url:
         #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
         defaultZ4 one: http://${eureka.instance.hostname}:${server.port}/eureka/
   ```

   

3. 启动类

   ```java
   @EnableEurekaServer
   ```

   

**client使用**

1. 依赖

2. 改xml

   ```yml
   eureka:
     client:
       #表示是否将自己注册进EurekaServer默认为true
       register-with-eureka: true
       #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
       fetch-registry: true
       service-url:
         defaultZone: http://localhost:7001/eureka
   
   ```

3. 启动类

同上





### **Eureka集群**构建

微服务RPC远程调用的核心是什么：

高可用：搭建Eureka注册中心集群，实现负载均衡+故障容错

需要两个eureka相互注册

```yml
server:
  port: 7002
eureka:
  instance:
    hostname: eureka7002.com #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      # 多个注册时用,隔开
      defaultZone: http://eureka7001.com:7001/eureka/
```



### actuator完善微服务信息

作用：修改监视器，如eureka中的显示

修改主机名称显示

修改ip地址显示

**使用**(需要actuator依赖)

```yml
eureka:
  instance:
    instance-id: payment8002
    # 提示IP地址
    prefer-ip-address: true
```



### 服务发现

使用：

某controller中

```java
    @Resource
    private DiscoveryClient discoveryClient;
    @GetMapping(value = "/payment/discovery")
    public Object discovery(){
        //获得注册中心内所有的服务名称
        List<String> services = discoveryClient.getServices();
        for (String service : services) {
            System.out.println(service);
        }
        //获取某个服务名称下所有的服务实例
        List<ServiceInstance> instances = discoveryClient.getInstances("cloud-payment-service");
        for (ServiceInstance instance : instances) {
            System.out.println(instance.getServiceId()+" "+instance.getInstanceId()+" "+instance.getHost()+" "+instance.getPort()+" "+instance.getUri());
        }
        return discoveryClient;
    }
```



### Eureka自我保护

eureka在CAP中是AP的

某个微服务在某个时间段不可用时（eurekaServer在一定时间内没有接收到该微服务实例的心跳，默认90秒），eureka不会立即删除

**自我保护机制**∶默认情况下Eurekaclient定时向EurekaServer端发送心跳包
如果Eureka在server端在一定时间内(默认90秒)没有收到EurekaClient发送心跳包，便会直接从服务注册列表中剔除该服务，但是在短时间( 90秒中)内丢失了大量的服务实例心跳，这时候Eurekaserver会开启自我保护机制，不会剔除该服务（该现象可能出现在如果网络不通但是EurekaClient为出现宕机，此时如果换做别的注册中心如果一定时间内没有收到心跳会将剔除该服务，这样就出现了严重失误，因为客户端还能正常发送心跳，只是网络延迟问题，而保护机制是为了解决此问题而产生的)

**关闭自我保护**

略



## Zookeeper

分布式协调工具，可以实现注册中心

cloud-zookeeper注册的节点是临时的

CP

使用：

1. 导入依赖

```xml
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
    </dependency>
```

如果出现依赖冲突(zookeeper依赖版本高于安装的版本)，先排除自带的zookeeper

```xml
<dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
    <exclusions>
    	<exclusion>
        	<groupId>org.apach.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </exclusion>
    </exclusions>
    </dependency>
<dependency>
	  	<groupId>org.apach.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
    <version>3.4.9</version>
</dependency>
```



## Ribbon

是**客户端** **负载均衡**工具

主要功能：提供客户端的**负载均衡**和**服务调用**

简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balancer(简称LB)后面所有的机器，Ribbon会自动的帮助你基于某种规则(如简单轮询，随机连接等）去连接这些机器。我们很容易使用Ribbon实现自定义的负载均衡算法。



**ribbon和nginx**

ribbon是本地负载均衡

nginx是服务端负载均衡



**集中式LB**

即在服务消费方和提供方之间使用独立的LB设施，如Nginx，该设施负责将请求通过某种策略转发至服务提供方

**进程内LB**

将LB逻辑集成在消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。

eureka自带集成了ribbon，3.x之后继承了loadbalance



**Ribbon负载均衡策略**



### RestTemplate

getForObject方法，返回对象为json

getForEntity方法，返回对象为ResponseEntity对象，包含一些重要信息，如响应头、状态码等



## OpenFeign

OpenFeign是一个声明式的Web服务客户端，让编写web服务客户端变得非常容易，只需要创建一个接口并在上面添加注解就可以了

前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)，即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量。

**使用**

接口+注解：微服务调用接口+`@FeignClient`

在启动类添加启动注解`@EnableFeignClients`



**超时控制**

springcloud版本问题搞不了

**日志增强**

NONE：默认，不显示任何日志

BASIC：记录请求方法、URL、响应状态码和执行时间

HEADERS：basic加请求和响应头信息

FULL：HEADERS+请求响应的正文以及元数据

**使用**

创建一个FeignConfig类

```java
@Configuration
public class FeignConfig{
    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

在yaml中配置

```yml
logging:
	level:
	#服务接口
		com.haha.xxxservice: debug
```



## **Hystrix断路器**

Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

作用：

+ 服务降级：返回一个客户端能处理的备用结果，如服务器繁忙，请稍后再试
  + 降级的情况：异常、超时、熔断触发、线程池/信号量满了
+ 服务熔断
+ 服务限流
+ 接近石狮监控



### 服务降级

服务端：

`@HystrixCommand`

```java
 @Override
    @HystrixCommand(fallbackMethod = "paymentInfoError_TimeoutHandler",commandProperties = {
            //设置这个线程的超时时间是3s，3s内是正常的业务逻辑，超过3s调用fallbackMethod指定的方法进行处理
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    public String paymentInfoError(Integer id){
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "Thread"+Thread.currentThread().getName()+"paymengInfoERROR"+"\t"+id+"耗时3s";
    }
    public String paymentInfoError_TimeoutHandler(Integer id){
        return "线程池："+Thread.currentThread().getName()+"   系统繁忙，请稍后再试,id："+id+"\t"+"o(╥﹏╥)o";
    }
```

主启动类加`@EnableCircuitBreaker`

**客户端：**

要配置yml