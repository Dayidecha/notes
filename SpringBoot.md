#  SpringBoot2基础入门

**Springboot核心：自动装配**

![image-20210412134853907](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210412134853907.png)



## **Spring是如何简化Java开发的**

1. 基于POJO的轻量级和最小入侵性编程
2. 通过IOC，依赖注入（DI）和面向接口实现松耦合
3. 基于切面（AOP）和惯例进行声明式编程
4. 通过切面和模板减少样式代码



## 原理

pom.xml

+ Spring-boot-dependencies：核心依赖在父工程中
+ 引入Springboot依赖的时候，不需要指定版本，就因为有这些版本仓库



**启动器**

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
```

+ 启动器：Sprintboot启动场景
+ spring-boot-starter-web，他就会自动导入web环境所有的依赖
+ springboot会把所有的功能场景，都变成一个个启动器
+ 需要什么功能，



**主程序**

```java
//标准这个类是一个springboot应用
@SpringBootApplication
public class HelloWorldApplication {

	public static void main(String[] args) {
		
		SpringApplication.run(HelloWorldApplication.class, args);
	}

}
```



+ 注解

```java
@SpringBootConfiguration:sprintboot配置
	@Configuration:spring:配置类
    @Component:说明这也是一个spring组件
@EnableAutoConfiguration:自动配置
    @AutoConfigurationPackage:自动配置包
        @Import(AutoConfigurationPackages.Registrar.class):导入选择器
```









结论：springboot所有的自动配置都是在启动的时候扫描并加载：`spring.factories`所有的自动配置类都在这里面，但不一定生效，要判断条件是否成立，只要导入了对应的启动器，我们自动装配就会生效，然后配置成功



1. springboot在启动的时候，从类路径下/META-INF/spring.factories获取指定的值
2. 将这些自动配置的类导入容器，自动配置就会生效，帮我们自动配置
3. 整合JavaEE，解决方案和自动配置的东西都在`spring-boot-autoconfigure-2.4.4.jar`包下
4. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器
5. 容器中会存在很大xxxAutoConfiguration的文件(@Bean)



启动器Springbootapplication

1. 推断应用的类型是普通的项目还是Web项目

2. 查找并加载所有可用初始化器，设置到initializers属性中

3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类



## Yaml语法

严格空格！！！！！！！！！！！！！！！！！！！！

```yaml
#可以注入到配置类中
#数组
pets:
  - cat
  - dog
  - pig
	
pets: [pig,cat,dog]

#对象
student:
  name: liming
  age: 3
  
student: {name: liming,age: 3}
```





yaml可能给实体类赋值

方法1：

```java
//person在yaml中
@ConfigurationProperties(prefix = "person")
public class Person {}
```

+ @ConfigurationProperties：将配置文件中每一个属性的值，映射到组件中
+ 可在yaml中写el表达式`${random.int}`



方法2：

```java
//加载指定的配置文件
@PropertySource(value="classpath:xxx.properties")
public class Person {
    //SPEL表达式
    @value("${name}")
    private String name;
  
}
```



## SpringBoot 探究



```java
//标准这个类是一个springboot应用
@SpringBootApplication
public class HelloWorldApplication {

	public static void main(String[] args) {
		//返回IOC容器
		ConfigurableApplicationContext run = SpringApplication.run(HelloWorldApplication.class, args);
        //查看配置好的组件
		String[] beanDefinitionNames = run.getBeanDefinitionNames();
		for (String beanDefinitionName : beanDefinitionNames) {
			System.out.println(beanDefinitionName);

		}

	}

}
```



### 启动器

启动器是一组集合依赖的描述，`spring-boot-starter-*`：*为某种场景

`*-spring-boot-starter`：第三方的启动器

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter**-`web`：

 spring-boot-starter：spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器

所有的场景都在：

[这]: https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter



## Springboot特点

+ ### **依赖管理**



如果需要修改自动配置的版本时：

1. 在pom.xml中添加

```xml
<properties>
    <mysql.version>5.1.43</mysql.version>
</properties>
```

2. 点右边的maven，选择刷新



+ ### 自动配置



1. 自动配置好SpringMVC

   + 引入SpringMVC全套组件
   + 自动配置好SpringMVC常用组件

2. 自动配置好Web常见功能，如字符编码问题

   + SpringBoot帮我们配置好了所有web开发的常见场景

3. 默认的包结构

   + 主程序所在的包及其下面的所有子包里面的组件都会被默认扫描进来

     + `@SpringBootApplication(scanBasePackages="com.boot")`

       > 扩大自动扫描范围

     + 或者`@ComponentScan("")`指定扫描路径，这个注解不能重复



+ 各种配置都有默认值
  + 默认配置最终是映射到MultiPartProperties
  + 配置文件的值最终都会绑定每个类上，这个类会在容器中创建对象
+ 按需加载自动配置
  + 引入了哪些场景这个场景的自动配置才会开启
  + 



##  容器功能

### 添加组件

1. **@Configuration**

```java
/*
* 1.配置类里面使用@Bean标准方法给容器注册组件，默认是单实例的
* 2.配置类本身也是组件
* */
@Configuration
public class MyConfig {

    /* 在配置类中给容器中添加组件，以组件名为方法名，返回值类
    型是组件类型，返回值就是组件在容器中的实例*/
    @Bean("狗") //可以自定义组件名
    public Dog dog1(){
        return new Dog("旺财",2);
    }
}

//application中
        MyConfig myConfig = run.getBean(MyConfig.class);
        //如果@Configuration(proxyBeanMethods = true)代理调用方法，Springboot会检查这个组件是否在容器里被创建，若被创建，则直接赋值
        Person person = myConfig.person1();
        Person person1 = myConfig.person1();
        System.out.println(person==person1);
```

```java
proxyBeanMethods代理bean方法：
    full(proxyBeanMethods=true)
    list(proxyBeanMethods=false)
```



2. **@Bean @Component @Controller @Service @Repository**



3. **@ComponentScan @Import**

```java
@Import({User.class,DBHelper.class})
```

`run.getBeanNamesForType(User.class)`获取所有User类型的组件



4. **@Conditional**

   **条件装配**:当满足Conditional指定的条件，则进行组件注入

`ConditionalOnBean(name = "name")`在程序中有name时，才注入某个组件

`ConditionalOnMissingBean(name="name")`:没有name组件时，注入某个组件

`ImportResource("classpath:beans.xml")`从resouce导入spring配置文件



### **配置绑定**

方法1：

```java
//只有在容器中的组件，才会用于SpringBoot提供的强大功能
@Component
@ConfigurationProperties(prefix="name")


//使用: xxxController.java;
@AutoWried
Name name;
```



放法2：（写在配置类,注册第三方包的组件到容器中）

```java
//config/myConfig.java
@EnableConfigurationProperties(Name.class)
//1.开启组件配置功能

//Bean/Car
//不用写@Component
@ConfigurationProperties(prefix="name")
```



## 自动配置原理

1. @SpringBootConfiguration

+ 代表当前一个配置类

2. @ComponentScan

+ 指定扫描，Spring注解：

3. EnableAutoConfiguration

   + @AutoConfigurationPackage

     自动配置包



**按需装配**

springboot启动时加载所有场景的自动配置，但按照条件装配规则，最终会按需配置

导入的包的类中有`@ConditionalOnxxx`实现按需开启

**总结：**

+ SpringBoot先加载所有的自动配置类 xxxAutoConfiguration
+ 每个自动配置类按照条件进行生效，默认会绑定配置文件指定的值。xxxProperties里面拿，xxxxProperties和配置文件进行了绑定
+ 生效的配置类就会给容器装配很多组件
+ 只有容器中有这些组件，就相当于这些功能有了
+ 定制化配置
  + 用户直接自己@Bean替换底层的组件
  + 用户去看这个组件是获取的配置文件什么值就去修改



### 最佳实践

+ 引入场景依赖
  + https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
+ 查看自动配置了哪些
  + 引入场景对应的自动配置一般都生效
  + 在项目的application.properties中开启`debug=true`,运行项目，控制台会输入
  + `Positive matches ` ：（自动配置类启用的）
  + `Negative matches`：（没有启动，没有匹配成功的自动配置类）
+ 是否需要修改
  + 参照文档修改配置
    + 自己分析：xxxProperties绑定了哪些属性
    + 参考文档：https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
  + 自定义加入或替换组件
    + @Bean、@Component
  + 自定义器 xxxxCustomizer





## 开发小技巧



### Lombok

简化javaBean



1. 引入依赖

   ```xml
         <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <version>${lombok.version}</version>
         </dependency>
   
   //搜索在idea中lombok
   ```

2. 使用

   ```java
   //lombok注解
   @Slf4j
   @Data
   @ToString
   @AllArgsConstructor
   @NoArgsConstructor
   @EqualsAndHashCode
   public class Car{
       private String brand;
       
       //不用写get和set
       
       log.info("~")
   }
   ```




### Dev-Tools

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
```







## Spring Initializer

~



# SpringBoot核心功能



## 配置文件

**基本语法**

+ 大小写敏感
+ 使用缩进进行层级关系
+ 不允许使用tab，只允许空格
+ '#' 表示注释
+ 字符串无需加`""`号



**数据类型：**



**字面量**

```
k: v
```

字符串默认不用加上单引号或者双引号；

`""`：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

*eg：* name: "zhangsan \n lisi"：输出；zhangsan 换行 lisi

`''`：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

*eg：* name: ‘zhangsan \n lisi’：输出；zhangsan \n lisi

**对象，map：**

`k: v`在下一行来写对象的属性和值的关系；注意缩进

1. ```yaml
   person:
     name: 张三
     gender: 男
     age: 22
   ```

2. 行内写法

   ```yaml
   person: {name: 张三,gender: 男,age: 22}
   ```

**MAp**

```
acme:
  map:
    "[/key1]": "value1"
    "[/key2]": "value2"
    "[/key3]": "value3"
```





## Web开发

![image-20210414173231279](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210414173231279.png)





### 1.简单的功能分析



**静态资源访问**



类路径下（resources)`/static` (or `/public` or `/resources` or `/META-INF/resources`中的资源，可以通过`/**`直接访问

原理：静态映射/**



**静态资源访问前缀**

```xml
spring.mvc.static-path-pattern=/resources/**
```



**改变默认静态资源路径**

```xml
spring.web.resources.static-locations=classpath:/haha
```



**webjar**

`/webjar`





**欢迎页支持**

+ 在静态资源目录`index.html`

+ controller能处理/index

  

**自定义Favicon**



**禁用静态资源**

`spring.web.resources.add_mappings:false`

**静态资源缓存时间**

`spring.web.resources.cache.period:11000`

### **2.静态资源配置原理**

+ 以下是SpringBoot对SpringMVC的默认配置

  **`org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration`**

  自动配置在Spring的默认值之上添加了以下功能：

  - 包含`ContentNegotiatingViewResolver`和`BeanNameViewResolver`。--> 视图解析器
  - 支持服务静态资源，包括对WebJars的支持（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)）。--> 静态资源文件夹路径
  - 自动注册`Converter`，`GenericConverter`和`Formatter `beans。--> 转换器，格式化器
  - 支持`HttpMessageConverters`（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)）。--> SpringMVC用来转换Http请求和响应的；User---Json；
  - 自动注册`MessageCodesResolver`（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-message-codes)）。--> 定义错误代码生成规则
  - 静态`index.html`支持。--> 静态首页访问
  - 定制`Favicon`支持（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)）。--> 网站图标
  - 自动使用`ConfigurableWebBindingInitializer`bean（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)）。

  如果您想保留 Spring Boot MVC 的功能，并且需要添加其他 [MVC 配置](https://docs.spring.io/spring/docs/5.1.3.RELEASE/spring-framework-reference/web.html#mvc)（拦截器，格式化程序和视图控制器等），可以添加自己的 `WebMvcConfigurer` 类型的 `@Configuration` 类，但**不能**带 `@EnableWebMvc` 注解。如果您想自定义 `RequestMappingHandlerMapping`、`RequestMappingHandlerAdapter` 或者 `ExceptionHandlerExceptionResolver` 实例，可以声明一个 `WebMvcRegistrationsAdapter` 实例来提供这些组件。

  如果您想完全掌控 Spring MVC，可以添加自定义注解了 `@EnableWebMvc` 的 @Configuration 配置类。

  

### 3.请求参数处理

#### **请求映射**

+ xxxMapping

+ Rest风格支持（使用HTTP请求方式动词来表示对资源的操作）

  + 以前 /getUser获取用户 /deleteUser 删除用户 /edutUser 修改用户
  + 现在：/user GET-获取用户 DELETE-删除用户 PUT-修改用户 POST-保存用户
  + 核心Filter;HiddenHttpMethodFilter
    + 就是都以post方式提交,但是后台会根据隐藏域中_method的值把请求方式改掉
    + 在springboot中手动开启
    + `spring.mvc.hiddenmethod.filter.enabled=true`

  

```java
    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String getUser(){
        return "get";
    }

    @RequestMapping(value = "/user",method = RequestMethod.POST)
    public String postUser(){
        return "post";
    }
```



**Rest原理（表单提交要使用REST的时候）**

+ **表单提交会带上method=PUT**
+ 请求过来被**HiidenHttpMethodFilter**拦截
  + 过滤器放行的时候使用**wrapper**,以后的方法调用**getMethod**的调用**requestWrapper**



**Rest使用客户端工具**

+ 如PostMan直接方法put、delete即可



**请求映射原理**

+ 所有的请求映射都保存在HandlerMapping中
  + SpringBoot自动配置了WelcomePageHandlerMapping
  + RequestMappingHandlerMapping
+ 请求进来，遍历所有HandlerMapping，
  + 如果找到，就请求对应的handler
+ 可以自己给容器中放**HandlerMapping**



#### 普通参数与基本注解

+ **注解**

@PathVariable(路径变量)  

@RequestHeader(请求头) 

@RequestParam(获取请求参数) 

 @CookieValue(获取cookie值)

 @RequestAttribute(获取request域属性)

 @ReuqestBody(获取请求体)

 @MatrixVariable(矩阵变量)

```java
    @GetMapping("/car/{id}/owner/{name}")
    public Map<String,Object> getCar(@PathVariable("id") Integer id, @RequestHeader("User-agent") String agent){
    }
```

 矩阵变量：`/cars/{path;low=34;brand=byd,audi,yd}`

在url中：`<a href="/cars/sell;low=34;brand=yd,audi">`

面试题：页面开发，cooKie被禁用了，session里面的内容怎么使用：

url重写：/abc;jsessionid=xxxx 把cookie的值使用矩阵变量的方式进行传递

> SpringBoot默认禁用矩阵变量的功能



springMVC目标方法能写多少种参数类型，取决于参数解析器



+ **Servlet API**

 WebRequest、ServletRequest、MultipartRequest、HttpSesion、javax.servlet.http.PushBuilder、Principal、InputStream、Reader、HttpMethod、Locale、TimeZone、ZoneId



+ **复杂参数**

Map、Errors/BindingResult、Model、RedirectAttributes（重定向携带数据）、ServletResponse、SessionStatus、UriComponentsBuilder,ServletUriComponentsBuilder

> map、model里面放的数据会被放在request的请求域，相当于request.setAttribute





+ **自定义对象参数**

  数据绑定：页面提交的请求数据，可以和对象属性进行绑定



#### POJO封装过程

+ 解析器：ServletModelAttributeMethodProcessor





### 5.视图解析与模板引擎

## 