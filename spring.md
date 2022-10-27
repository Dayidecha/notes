## 1.Spring

**1.1 简介**：spring是一个轻量级的，控制反转（IOC)和面向切面编程（NOP）的框架



**1.2 优点：**

+ spring是一个开源的框架（容器）
+ Spring是一个轻量级、非侵入式的框架
+ 控制反转（IOC），面向切面编程（AOP）
+ 支持事务处理，对框架整合的支持



**总结：spring是一个轻量级的控制反转（IOC)和面向切面编程的（AOP）的框架**



**1.3 组成**

![image-20210420194943677](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210420194943677.png)



## 2.IOC理论

**控制反转IoC(Inversion of Control)，是一种设计思想，DI（依赖注入）是实习IoC的一种方法**



控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方式是依赖注入DI





## 3.第一个Spring

1.导包

```xml
    <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.12.RELEASE</version>
        </dependency>
```

2.添加配置beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    
<bean id="hello" class="com.l.pojo.Hello">
    
    <property name="str" value="spring"/>
</bean>

</beans>
```

使用Spring来创建对象

id = 变量名，class=new 的对象

property 相当于set（spring会调用类的set方法）

`<property name="str" ref=""/>`

+ ref是引用spring容器中创建好的对象
+ value是具体的值



3.实例化容器，通过**ApplicationContext**拿到注册的对象

```java
//获取Spring上下文对象   
       ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Object hello = context.getBean("hello");
        System.out.println(hello.toString());
```



+ spring默认使用无参构造器创建对象

+ 有参构造时

  ```xml
  <bean id="hello" class="com.l.pojo.Hello">
      <!--有参构造1.通过下标-->
      <constructor-arg index="0" value="haha"/>
       <!--有参构造2.通过参数名-->
      <constructor-arg name="name" value="haha"/>
  </bean>
  
  ```



## 4.Spring配置

### 4.1别名

```xml
<alias name="fromName" alias="toName"/>
```

### 4.2 bean配置

```xml
<bean id="hello" class="com.l.pojo.Hello"></bean>
```

id：bean的唯一标识符，也就是对象名

class：bean对象所对应的全限定名：包名+类型

name：别名，可同时去多个别名，比alias高级

scope：选择【单例【原型

### 4.3 import

用于团队开发使用，将多个配置文件导入合并为一个

总配置：applicationContext.xml

`<import resource="beans.xml/>"`



## 5.依赖注入

### 5.1 构造器注入



### 5.2 set注入【重点】

+ 依赖注入：Set注入
  + 依赖：bean对象的创建依赖于容器
  + 注入：bean对象中的所有属性，由容器来注入

```xml
    <bean id="student" class="com.l.pojo.Student">
        <!--第一种，普通类注入-->
        <property name="name" value="xiaoming"/>
        <!--第二种，bean注入-->
        <property name="address" ref="address"/>
        <!--第三种，数字注入-->
        <property name="book">
            <array>
                <value>c++</value>
                <value>Java</value>
            </array>
        </property>
        <!--第四种，list注入-->
        <property name="hobbies">
            <list>
                <value>看书</value>
                <value>学习</value>
            </list>
        </property>
        <!--第五种，map注入-->
        <property name="cars">
            <map>
                <entry key="苹果" value="apple"/>
            </map>
        </property>
        <!--第六种，properties注入-->
        <property name="info">
            <props>
                <prop key="emial">123456@qq.com</prop>
            </props>

        </property>

    </bean>
```



### 5.3 扩展方式注入

p命名和c命名

+ p命名空间可以直接注入属性的值,通过setter构造

使用：

在配置文件头添加约束：` xmlns:p="http://www.springframework.org/schema/p"`

```xml

<bean id="user" class="com.l.poji.User" p:name="xiaoming" p:age=18/>
```



+ c命名空间：构造器注入

使用：

在配置文件头添加约束：` xmlns:c="http://www.springframework.org/schema/c"`



### 5.4 Bean作用域

| Scope                                                        | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [singleton](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-singleton) | (默认)将每个 Spring IoC 容器的单个 bean 定义范围限定为单个对象实例。 |
| [prototype](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-prototype) | 将单个 bean 定义的作用域限定为任意数量的对象实例。           |
| [request](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-request) | 将单个 bean 定义的范围限定为单个 HTTP 请求的生命周期。也就是说，每个 HTTP 请求都有一个在单个 bean 定义后面创建的 bean 实例。仅在可感知网络的 Spring `ApplicationContext`中有效。 |
| [session](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-session) | 将单个 bean 定义的范围限定为 HTTP `Session`的生命周期。仅在可感知网络的 Spring `ApplicationContext`上下文中有效。 |
| [application](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-application) | 将单个 bean 定义的范围限定为`ServletContext`的生命周期。仅在可感知网络的 Spring `ApplicationContext`上下文中有效。 |
| [websocket](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#websocket-stomp-websocket-scope) | 将单个 bean 定义的范围限定为`WebSocket`的生命周期。仅在可感知网络的 Spring `ApplicationContext`上下文中有效。 |

1.singleton 代理模式

2.prototype原型模式：每次从容器种`get`的时候，都会产生一个新对象



## 6.Bean自动装配

+ 自动装配式Spring满足bean依赖的一种方式
+ Spring会在上下文种自动寻找，并自动给bean装配



在Spring中有三种装配方式

1. 在XML中显示装
2. 在java中显示装配
3. 隐式自动装配



### 6.1 byName和byType

```xml
<!--
byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid
byType:会自动在容器上下文中查找，和自己对象字段类型相同的beanid
-->
    <bean id="person" class="com.l.pojo.Person" autowire="byName">
        <property name="name" value="xiaoming"/>
    </bean>
```



小结：

+ byName时，需要保证所有的beanid唯一
+ byType时，需要保证所有bean的class唯一（单例）



### 6.2 使用注解实现自动装配

jdk1.5支持注解



使用：

1.导入约束：context



2.配置注解的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
	<!-- 开启注解支持 -->
    <context:annotation-config/>

</beans>
```



**@Autowired**

**@Qualifier**(value = "beans.xml中的beanId")

使用autowire可以省略set方法，默认匹配byType或byName

autowired默认通过byType方式，如果有多个类型相同的，在通过byName

如果xml文件中同一个对象被多个bean使用，Autowired无法按类型找到，可以用@Qualifier指定id查找

```java
//说明该对象可以为null    
	@Autowired(required = false)
    private Cat cat;
```

```java
 //标记的字段指字段可以为null
public People(@Nullable String name){}
```



小结：@Resource和@Autiwired的区别

+ 都是用来字段装配，都可以



## 8.使用注解开发

spring4之后使用注解开发，必须保证aop包导入了

在使用注解需要导入context约束，增加注解的支持

1.bean

2.属性如何注入    

```java
@Component
@Scope("singleton")
public class User {
   @Value("haha")
   public String name;
}

```

3.衍生的注解

@Component有几个衍生注解，在web开发中，会按照mvc三层架构分层，功能和component一致

+ dao【@Repository】
+ service【@Service】
+ controller【@Controller】

4.自动装配置

```
@Autowired
@Nullable -字段标记这个注解，说明这个字段可以为null
```



5.作用域

@Scope

6.小结

xml和注解：

+ xml更加万能，适用于任何场合，维护简单方便
+ 注解：不是自己的类使用不了，维护相对复杂

xml和注解的实践：

+ xml用来管理bean；
+ 注解只负责完成属性的注入

### 注解说明

@Component：组件，放在类上说明这个类被Spring管理了，就是bean



## 9.使用Java的方式配置Spring

@Configuration代表这个是个配置类

```java
@Configuration
//这里加componentscan扫描就可以不用注册bean了，两种方式选一个就行
//两种方式，配置类+@bean  或者  配置类+扫描包
@ComponentScan("com.l.pojo")
//相当于xml中的import
@Import(application2.class)
public class AppConfig {
//方法名相当于bean id,返回值相当于bean class
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

上面的`AppConfig`类等效于下面的 Spring `<beans/>` XML：

```xml
<beans>
    <bean id="myService" class="com.acme.services.MyServiceImpl"/>
</beans>
```





## 10.AOP

SpringAOP底层：代理模式 

代理模式分类：

+ 静态代理
+ 动态代理



### 10.1 静态代理

+ 抽象角色：一般会使用接口或者抽象类来解决
+ 真实角色：被代理的角色
+ 代理角色：代理真实角色
+ 客户：访问代理对象的人

缺点：一个真实的角色就会产生一个代理角色，代码量会翻倍



## 10.2 动态代理

+ 底层是反射
+ 动态代理和静态代理角色是一样的
+ 动态代理是动态生成的
+ 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  + 基于接口-----JDK动态代理
  + 基于类-----cglib
  + java字节码----javassist

需要了解两个类：**Proxy**，**InvocationHandler**

Proxy是生成动态代理实例对象的 InvocationHandler调用处理程序并返回结果的



动态代理好处：

+ 公务业务发送拓展的时候，方便集中管理
+ 一个动态代理类代理的是一个接口，一般就是对应的一类业务
+ 一个动态代理类可以代理多个类，只要实现同一个接口即可



## 11.AOP

AOP意为面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

+ 横向编程思想，在不影响原理业务代码的情况下，实现业务的增强

### 11.1 Aop在Spring中的作用

==提供声明式事务，允许用户自定义切面==

+ 横切关注点：跨越应用程序多个模块的方法或功能，即是与我们业务逻辑无关的，但我们需要关注的部分，就是横切关注点。如日志，缓存，安全，事务等等。。
+ 切面（ASPECT）：横切面关注点，被模块化的特殊对象，是一个类。
+ 通知（ADVICE)）：切面必须完成的工作，是类中的一个方法
+ 目标（TARGET）：被通知的对象
+ 代理（PROXY）：向目标对象应用通知之后被创建的对象
+ 切入点（POINTCUT）：切面通知执行的“地点”的定义
+ 连接点（JOINTPOINT）：与切入点匹配的执行点

![image-20210421235813174](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210421235813174.png)



### 方式1：使用api接口

applicationContext.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    
        <!--方式1：使用原生Spring API接口-->
    <!--配置aop,需要打偶aop约束-->
    <aop:config>
        <!--切入点 expression：表达式，execution（要执行的位置）-->
        <aop:pointcut id="ponitcut" expression="execution(* com.l.service.UserServiceImp.*(..))"/>

        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="ponitcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="ponitcut"/>
    </aop:config>
```



log.java(日志advisor)

```java
@Component
public class AfterLog implements AfterReturningAdvice {
    //o 返回值

    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("返回结果为："+o);
    }
}
```



### **方式2：使用自定义类实现AOP**

```xml
    <!--方式2：-->
    <aop:config>
        <!--自定义切面，ref为要引用的类-->
        <aop:aspect ref="diyPointCut">
            <!--切入点-->
            <aop:pointcut id="ponitcut" expression="execution(* com.l.service.UserServiceImp.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="ponitcut"/>
            <aop:after method="after" pointcut-ref="ponitcut"/>

        </aop:aspect>

    </aop:config>
```



### 方式3：使用注解实现

```xml
<!--方式3.使用注解-->
    <aop:aspectj-autoproxy/>
```

```java
@Aspect
public class AnnotationPointcUT {
    @Before("execution(* com.l.service.*.*(..))")
    public void before(){
        System.out.println("====before++++");
    }
}

    /*在环绕增强中，可以给定一个参数，代表我们要获取处理切入的点*/
    @Around("execution(* com.l.service.UserServiceImp.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");

        System.out.println(joinPoint.getSignature());
        //执行方法
        joinPoint.proceed();

        System.out.println("环绕后");
    }
```



`proxy-target-class="false"` false表示动态代理通过JDK实现，true表示通过cglib实现





## 12.整合Mybatis

步骤：

1. 导入相关jar包
   + junit
   + mybatis
   + mysql
   + spring
   + aop
   + mybatis-spring
2. 编写配置文件
3. 测试



### 12.1 Mybatis-spring

1.编写数据源配置

2.sqlSessionFactory

3.sqlSessionTemplate

4.需要给接口加实现类

5.将实现类注入到spring



sqlSessionTemplate是线程安全的





## 13.声明式事务

事务的ACID原则：

+ 原子性
+ 一致性
+ 隔离性
+ 持久性

### 13.2 spring中的事务管理

+ 声明式事务：aop
+ 编辑式事务：需要在代码中，进行事务的管理



声明式事务：

要开启 Spring 的事务处理功能，在 Spring 的配置文件中创建一个 `DataSourceTransactionManager` 对象：

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <constructor-arg ref="dataSource" />
</bean>
```



注意，如果你想使用由容器管理的事务，而**不想**使用 Spring 的事务管理，你就**不能**配置任何的 Spring 事务管理器。并**必须配置** `SqlSessionFactoryBean` 以使用基本的 MyBatis 的 `ManagedTransactionFactory`：

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
  <property name="transactionFactory">
    <bean class="org.apache.ibatis.transaction.managed.ManagedTransactionFactory" />
  </property>
</bean>
```

```xml
<tx:jta-transaction-manager />
```



## 14.SpringMVC

MVC：模型（dao,service) 视图（~）控制层（Servlet)

Controller：

+ 取得表单数据
+ 调用业务逻辑
+ 转向指定的页面

Model：

+ 业务逻辑
+ 保存数据的状态



springMVC核心3要素：

+ 处理器映射器
+ 处理设配器
+ 视图解析器

### 14.1 MVC框架要做的事情

1.将url映射到java类或者java类的方法

2.封装用户提交的数据

3.处理请求-调用相关的业务处理-封装响应数据

4.将响应的数据进行渲染 jsp/html等表层数据



springmvc框架帮我们做了：

1.请求进来，被DIspatcherServlet捕获，DIspatcherServlet调用HandlerMapping-HandlerExecution找到对应的handler名字并返回给DIspatcherServlet

2.DIspatcherServlet调用HandlerAdapter并传入handler,HandlerAdapter调用对应controller，controller调用业务层service得到执行后的返回数据，封装成ModelAndView返回给DIspatcherServlet

3.DIspatcherServlet把ModelAndView传给ViewResolver（视图解析器）生成对应的页面返回给用户







### 14.2 快速入门

1、新建一个Moudle ， springmvc-02-hello ， 添加web的支持！

2、确定导入了SpringMVC 的依赖！

3、配置web.xml  ， 注册DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?><web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"        version="4.0">
   <!--1.注册DispatcherServlet-->   <servlet>       <servlet-name>springmvc</servlet-name>       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>       <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->       <init-param>           <param-name>contextConfigLocation</param-name>           <param-value>classpath:springmvc-servlet.xml</param-value>       </init-param>       <!--启动级别-1-->       <load-on-startup>1</load-on-startup>   </servlet>
   <!--/ 匹配所有的请求；（不包括.jsp）-->   <!--/* 匹配所有的请求；（包括.jsp）-->   <servlet-mapping>       <servlet-name>springmvc</servlet-name>       <url-pattern>/</url-pattern>   </servlet-mapping>
</web-app>
```

4、编写SpringMVC 的 配置文件！名称：springmvc-servlet.xml  : [servletname]-servlet.xml

说明，这里的名称要求是按照官方来的

```xml
<?xml version="1.0" encoding="UTF-8"?><beans xmlns="http://www.springframework.org/schema/beans"      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"      xsi:schemaLocation="http://www.springframework.org/schema/beans       http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

5、添加 处理映射器

- 

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
```

6、添加 处理器适配器

```xml
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```

7、添加 视图解析器

```xml
<!--视图解析器:DispatcherServlet给他的ModelAndView--><bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">   <!--前缀-->   <property name="prefix" value="/WEB-INF/jsp/"/>   <!--后缀-->   <property name="suffix" value=".jsp"/></bean>
```

8、编写我们要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个ModelAndView，装数据，封视图；

```java
package com.kuang.controller;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
//注意：这里我们先导入Controller接口public class HelloController implements Controller {
   public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {      
       //ModelAndView 模型和视图       
       ModelAndView mv = new ModelAndView();
       //封装对象，放在ModelAndView中。
       Model       mv.addObject("msg","HelloSpringMVC!");       
       //封装要跳转的视图，放在ModelAndView中  
       mv.setViewName("hello");  //: /WEB-INF/jsp/hello.jsp   
       return mv;  }   }
```

9、将自己的类交给SpringIOC容器，注册bean



```xml
<!--Handler--><bean id="/hello" class="com.kuang.controller.HelloController"/>
```

10、写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %><html><head>   <title>Kuangshen</title></head><body>${msg}</body></html>
```

11、配置Tomcat 启动测试！









### 14.3 使用注解开发

1.配置

web.xml

```xml
    <!--配置Dispatchervlet 这个是SpringMVC核心：前端控制器（请求分发器）-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--dispatcherServlet要绑定spring的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别：1-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--springmvc中 / 和 /*
        / 匹配所有请求,jsp除外
        /* 匹配所有请求
    -->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```



springmvc-servlet.xml

```xml
    <!--自动扫描包，让指定包下的注解生效，由IOC容器统一管理-->
    <context:component-scan base-package="com.l.controller"/>
    <!--让springmvc不处理静态资源-->
    <mvc:default-servlet-handler/>
    <!--让注解生效，完成映射关系，相当于注册mapping和adapter-->
    <mvc:annotation-driven/>

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```





### 14.4 Restful 风格

**概念**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

**功能**

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

**传统方式操作资源**  ：通过不同的参数来实现不同的效果！方法单一，post 和 get

​	http://127.0.0.1/item/queryItem.action?id=1 查询,GET

​	http://127.0.0.1/item/saveItem.action 新增,POST

​	http://127.0.0.1/item/updateItem.action 更新,POST

​	http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

**使用RESTful操作资源** ：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！

​	http://127.0.0.1/item/1 查询,GET

​	http://127.0.0.1/item 新增,POST

​	http://127.0.0.1/item 更新,PUT

​	http://127.0.0.1/item/1 删除,DELETE



在Spring MVC中可以使用  @PathVariable 注解，让方法参数的值对应绑定到一个URI模板变量上。

```java
@Controller
public class RestFulController {

   //映射访问路径
   @RequestMapping("/commit/{p1}/{p2}")
   public String index(@PathVariable int p1, @PathVariable int p2, Model model){
       
       int result = p1+p2;
       //Spring MVC会自动实例化一个Model对象用于向视图中传值
       model.addAttribute("msg", "结果："+result);
       //返回视图位置
       return "test";
  }
}
```





### 14.5 重定向和转发

**通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

测试前，需要将视图解析器注释掉

```java
@Controller
public class ResultSpringMVC {
   @RequestMapping("/rsm/t1")
   public String test1(){
       //转发
       return "/index.jsp";
  }

   @RequestMapping("/rsm/t2")
   public String test2(){
       //转发二
       return "forward:/index.jsp";
  }

   @RequestMapping("/rsm/t3")
   public String test3(){
       //重定向
       return "redirect:/index.jsp";
  }
}
```



**通过SpringMVC来实现转发和重定向 - 有视图解析器；**

重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题.

可以重定向到另外一个请求实现 .

```java
@Controller
public class ResultSpringMVC2 {
   @RequestMapping("/rsm2/t1")
   public String test1(){
       //转发
       return "test";
  }

   @RequestMapping("/rsm2/t2")
   public String test2(){
       //重定向
       return "redirect:/index.jsp";
       //return "redirect:hello.do"; //hello.do为另一个请求/
  }

}
```



### 14.6 请求参数提交

**处理提交数据**

**1、提交的域名称和处理方法的参数名一致**

提交数据 : http://localhost:8080/hello?name=kuangshen

处理方法 :

```
@RequestMapping("/hello")
public String hello(String name){
   System.out.println(name);
   return "hello";
}
```

后台输出 : kuangshen



**2、提交的域名称和处理方法的参数名不一致**

提交数据 : http://localhost:8080/hello?username=kuangshen

处理方法 :

```java
//@RequestParam("username") : username提交的域的名称 .
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
   System.out.println(name);
   return "hello";
}
```

后台输出 : kuangshen



**3、提交的是一个对象**

要求提交的表单域和对象的属性名一致  , 参数使用对象即可

1、实体类

```java
public class User {
   private int id;
   private String name;
   private int age;
   //构造
   //get/set
   //tostring()
}
```

2、提交数据 : http://localhost:8080/mvc04/user?name=kuangshen&id=1&age=15

3、处理方法 :

```java
@RequestMapping("/user")
public String user(User user){
   System.out.println(user);
   return "hello";
}
```

后台输出 : User { id=1, name='kuangshen', age=15 }

说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。



**数据显示到前端**

**第一种 : 通过ModelAndView**

我们前面一直都是如此 . 就不过多解释

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```



**第二种 : 通过ModelMap**

ModelMap

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("name",name);
   System.out.println(name);
   return "hello";
}
```



**第三种 : 通过Model**

Model

```java
@RequestMapping("/ct2/hello")
public String hello(@RequestParam("username") String name, Model model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("msg",name);
   System.out.println(name);
   return "test";
}
```



**对比**

就对于新手而言简单来说使用区别就是：

```
Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；

ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；

ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。
```



### 14.7 乱码问题

web.xml

```xml
<filter>
   <filter-name>encoding</filter-name>
   <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter- class>
   <init-param>
       <param-name>encoding</param-name>
       <param-value>utf-8</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>encoding</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```



### 14.8 JSON

json乱码问题

springmvc-config.xml

```xml
<mvc:annotation-driven>
   <mvc:message-converters register-defaults="true">
       <bean class="org.springframework.http.converter.StringHttpMessageConverter">
           <constructor-arg value="UTF-8"/>
       </bean>
       <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
           <property name="objectMapper">
               <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                   <property name="failOnEmptyBeans" value="false"/>
               </bean>
           </property>
       </bean>
   </mvc:message-converters>
</mvc:annotation-driven>
```

