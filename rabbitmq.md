**spring整合rabbitmq**

添加依赖

```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
```

添加配置：

```yml
  #rabbitmq配置
  rabbitmq:
    #主机地址
    host: 101.34.9.213
    #用户名
    username: guest
    password: guest
    #virtual-host
    virtual-host: /
    #端口
    port: 5672
    listener:
      simple:
        #消费者最小数量
        concurrency: 10
        #消费者最大数量
        max-concurrency: 10
        #限制消费者每次只能处理一条消息，处理完再继续处理一下条
        prefetch: 1
        #启动时是否默认启动容器，默认是true
        auto-startup: true
        #被拒绝时重新进入队列
        default-requeue-rejected: true
    template:
      retry:
        #发布重试,默认false
        enabled: true
        #重试时间，默认1000ms
        initial-interval: 1000ms
        #重试最大次数，默认3次
        max-attempts: 3
        #重试最大时间间隔，默认10000ms
        max-interval: 10000ms
        #重试时间乘数，下次是这一次的 1*时间
        multiplier: 1
```

添加配置类rabbitmqconfig

```java
/**
 * rabbitmq配置类
 */
@Configuration
public class RabbitMQconfig {

    @Bean
    public Queue queue(){
        //配置队列持久化
        return new Queue("queue",true);
    }
}
```

使用：

**消费者**

```java
@RabbitListener(queues = "queue")
    public void receive(Object msg){
        log.info("接受消息："+msg);
    }


```

**生产者**

````java
    @Autowired
    private RabbitTemplate rabbitTemplate;

    public void send(Object msg){
        log.info("发送消息："+msg);
        rabbitTemplate.convertAndSend("queue",msg);
    }
````



**消息队列交换机的交换模式**

`direct`  `topic`    `heade `(几乎不用)    `fanout`



+ fanout 广播模式
  + 不会处理路由键，消息会被转发给所有的消费者

+ direct 路由模式
+ topic 主题模式
  + `*.route.#`:*匹配一个单词，#匹配一个或多个单词

使用，以路由模式为例

**config类** 把队列、交换机和路由绑定

```java
private final static String QUEUE1 = "QUEUE01_DIRECT";
private final static String ROUNTINGKEY01 = "queue.green";
private final static String ROUNTINGKEY02 = "queue.blue";
private final static String DIRECTEXCHANGE ="directExchange";

    @Bean
    public Queue queue03(){
        return new Queue(QUEUE1);
    }

    @Bean
    public Queue queue04(){
        return new Queue(QUEUE2);
    }

    @Bean
    public DirectExchange directExchange(){ return new DirectExchange(DIRECTEXCHANGE);}

    @Bean
    public Binding binding03(){
        return BindingBuilder.bind(queue03()).to(directExchange()).with(ROUNTINGKEY01);
    }
```

**消费者** 监听队列

```java
@RabbitListener(queues = "QUEUE01_FANOUT")
    public void receive01(Object msg){
        log.info("QUEUE01_FANOUT接受消息："+msg);
    }
```

**生产者**  把消息推送到指定的**交换机**和**路由**中

```java
  public void sendDirect1(Object msg){
        log.info("发送消息到queue.green："+msg);
        rabbitTemplate.convertAndSend("directExchange","queue.green",msg);
    }
```

