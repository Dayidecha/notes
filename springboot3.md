#  Springboot基础入门

## HelloWorld

**步骤：**

**创建maven工程**

**引入依赖**

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.12.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

**创建主程序**

创建`MainApplication.java`

```java
/**
 * 这是一个springboot应用
 * 该类成为主程序类
 */
@SpringBootApplication
public class ManApplication {
    public static void main(String[] args) {
        SpringApplication.run(ManApplication.class,args);
    }
}
```



**编写业务**

**测试**

**简化配置**

**简化部署**

可以直接打成Jar包运行

包括运行所需要的所以环境

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

把项目打成jar包，直接在目标服务器上执行即可。

注意点：

取消cmd的快速编辑模式



## **springboot自动配置原理**

**依赖管理**

+ 父项目做依赖管理

  ```xml
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.3.12.RELEASE</version>
      </parent>
  ```

+ 开发时导入starter场景启动器`spring-boot-starter-*`

  > 所有场景启动器的底层依赖时`spring-boot-starter`

+ 无需关注版本号，自动版本仲裁

  > 当需要修改版本时，在项目的`pom.xml`中做如下修改

  ```xml
  <properties>
  	<mysql.version>8.0.25</mysql.version>
  </properties>
  ```

**自动配置**

+ 自动配置好了Tomcat

+ 自动配置好了SpringMVC

+ 自动配置好了Web常见功能，如：字符编码问题

+ 默认的包结构

  + 主程序类所在同级目录的包及其下面的所有子包里面的

  + 如果需要改变包扫描的路径，修改：

    ```java
    @SpringBootApplication(scanBasePackages = "com.haha")
    ```

    

+ 各种配置拥有的默认值

  默认值都映射到某一个类上，这个类会在容器中创建对象，如服务器类会映射到：

  ```java
  @ConfigurationProperties(
      prefix = "server",
      ignoreUnknownFields = true
  )
  public class ServerProperties {
      ...
  }
  ```

  

+ 按需加载所有自动配置项

  + 引入了哪些starter，就会加载相应的配置类
  + SpringBoot所有自动配置功能都在`spring-boot-autoconfigure`包里边



## 容器功能

### **组件添加**

**1.@Configuration**

+ 基本使用
+ Full模型与Lite模式

```java
//告诉spring这是一个配置类
//@Bean方法注册的组件默认是单例的
@Configuration
public class Myconfig {
    //给容器中添加组件，以方法名作为组件的id。返回值就是组件的类型，
    // 返回的值，就是组件在容器中的实例
    @Bean
    public User user01(){
        return new User("haha");
    }
    //自定义组件的名字
     @Bean("tom")
    public Pet pet01(){
        return new Pet("haha");
    }

}
```

****************

`@Configuration(proxyBeanMethods=True)`表示这个配置类会被代理增强，当直调用这个配置类中用@bean注释的方法时，spring会检查该方法是否已经在容器中创建，保证组件的单例

+ Full(proxyBeanMethods=True)

+ Lite(proxyBeanMethods=False)

  主要解决组件依赖的问题

**2.@Bean、@Component、@Controller、@Service、@Repository**



**3.@ComponentScan、@Import**

```java
@import({User.class,Pet.class})
@Configuration
class Person{
    ...
}
```



调用无参构造方法给容器中创建这两个组件

**4.@Conditional**

条件装配：满足Conditional指定的条件，则进行组件注入，可以修饰类或者方法





**5.ImportResource**

spring老项目中写了beans.xml，要迁移到springboot上时，

```java
@ImportResource("classpath:beans.xml")
```



### 配置绑定

把properties或yaml文件中的内容，读取并封装到JavaBean中

原生方法：`Properties pps = new Properties();`

**1.ConfigurationProperties**

```java
@Component
@ConfigurationProperties(prefix = "mycar")
public class Car {
    private String brand;
}
```

> 在properties文件中：mycar.brand = byd

**2.EnableConfigurationProperties+ConfigurationProperties**

```java
@EnableConfigurationProperties({Car.class})
class Myconfig{
    ...
}
```

car类中就不需要Component了



### 看哪些自动配置类生效了

在properties中修改：`debug=true`来开启自动配置报告

### 看需要修改哪些配置项

https://docs.spring.io/spring-boot/docs/2.3.12.RELEASE/reference/html/appendix-application-properties.html#common-application-properties



## 开发小技巧

### **Lombok**

1. 引入依赖

2. 在setting-plugin中搜索安装Lombok插件

3. 使用

   ```java
   @Data //生成set and get
   @ToString
   @AllArgsConstructor
   @NoArgsConstructor
   @Slf4j //注入一个log
   public class TestDemo {
       public static void main(String[] args) {
           log.info("haha");
           log.error("asd");
       }
   }
   ```

### spring initializer

~

# springboot核心功能

## 配置文件

**yaml语法**

对象:

```yaml
#行写法
k: {k1:v1,k2:v2}
#或
k:
	k1: v1
	k2: v2
```

数组：

```yaml
#行写法
k: [v1,v2,v3]
#或
k:
  - v1
  - v2
  - v3
```

在pom中添加以下依赖来开启配置绑定提示

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

打包时可以将上述依赖排除

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-configuration-processor</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```





## *web开发

**修改浏览器默认访问的路径**

```properties
spring.mvc.servlet.path=/mvc/
```

默认映射的是`/`



### SpringMVC自动配置概览

### 简单功能分析

1. **静态资源目录**

在类路径下`/static` (or `/public` or `/resources` or `/META-INF/resources`)下的静态资源可以直接访问：当前项目根路径/ + 静态资源名

修改**浏览器访问静态资源前缀**：

```properties
spring.mvc.static-path-pattern=/resources/**
```

修改**静态资源路径**：

```properties
spring.resources.static-locations=/haha/
```

原理：静态映射 `/**`

请求进来，先去找Controller看能不能处理，不能处理再找静态资源处理器`ResourceHttpRequestHandler`

2. **欢迎页支持**

把index.html放在静态资源路径下

+ 可以配置静态资源路径
+ 但不可以配置静态资源的**访问前缀**，否则会导致index.html不能被默认访问 



3. **定制Mvc配置功能**

**需要自定义Web功能的，实现WebMvcConfigurer接口**

方式1：

```java
@Configuration
public class myConfig implements WebMvcConfigurer{
    .......
}
```

方式2：

```java
@Configuration
public class Myconfig3 {

    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
        WebMvcConfigurer adapter = new WebMvcConfigurer() {
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/**").excludePathPatterns("/success","/goto");                     
            }
        };
        return adapter;
    }
}

```





### 请求参数处理

**普通参数与基本注解**

+ 注解:

  @PathVariable、@RequestHeader、@ModelAttribute、@RequestParam、@CookieValue、@RequestBody、@ReqeustAttribute、@MatrixVariable

  

**PathVariable**

用于获取路径变量（Restful风格）

```java
    //car/2/owner/zhangsan
    @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> getCar(@PathVariable("id") Integer id,
                                     @PathVariable("username")String username,
                            //如果参数是Map，会将所有的参数都封装到一个map中
                                     @PathVariable Map<String,String> map){
        Map<String,Object> map1 = new HashMap<>();
        map1.put("id",id);
        map1.put("username",username);
        map1.put("map",map);
        return map1;

    }
```

**RequestHeader**

获取请求头

```java
    //car/2/owner/zhangsan
    @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> getCar(@RequestHeader Map<String,String> map2){
        Map<String,Object> map1 = new HashMap<>();
        map1.put("id",id);
        map1.put("username",username);
        map1.put("map",map);
        return map1;

    }
```

**RequestParam**

获取请求参数

```java
//"hello?age=18&intest=football&intest=bastketball"
  @GetMapping("/hello")
    public Map<String,Object> getCar(@RequestParam("intest") List<String> intest
        ,@RequestParam Map<String,String> map2){}
```

**CookieValue**

获取cookie的值

```java
 public Map<String,Object> getCar(@CookieValue("_ga") String ga){}
```

**RequestBody**

获取请求体的值（post）

**ReqeustAttribute**

获取Request域中保存的属性，用来页面转发时取出当前的数据

> 不能通过map的方式获取全部的reqeustAttributes

```java
    @GetMapping("/goto")
    public String gotoPage(HttpServletRequest request){
        request.setAttribute("msg","哈哈");
        return "forward:/success";
    }

    @ResponseBody
    @GetMapping("/success")
    public String success(@RequestAttribute("msg") String msg, 
                          HttpServletRequest request){
        //获取rerequest参数的方法
        return request.getAttribute("msg").toString()+msg;
    }
```

**MatrixVariable**

SpringBoot默认禁用了矩阵变量的功能

获取请求路径中矩阵变量的值

查询字符串@RequestParam：`/car/{path}?xxxx=xxx&aaa=ccc`

矩阵变量：`/car/{path;low=34;brand=byd,auto}`，重点：分号前时路径变量，后时矩阵变量

**问题**：页面开发时，cookie禁用了，session里面的内容怎么使用；

一般来说,session.set(a,b) ----> jsessionid ----->cookie 客户端每次发请求时携带

**解决办法**：**url重写**，把jssessionid写在请求路径里



+ Servlet API:

  WebRequest、ServletRequest、MultipartRequest、HttpSession、javax.servlet.http.PushBuilder、Principal、InputStream.Reader、HttpMethod、Locale、TimeZone、Zoneld

+ 复制参数：

  Map、Errors/BindingResult、Model、RedirectAttributes、ServletRespone...

  

+ 自定义对象参数

### 数据响应与内容协商

### 视图解析与模板引擎

### 拦截器

****

**使用：**

1. **定义拦截器（实现HandlerInterceptor 接口）**

```java
public class LoginInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object haha = request.getAttribute("haha");

        if(haha!=null){
            request.setAttribute("msg","haha~");
            return true;
        }
        request.setAttribute("msg","haha missing!");
        request.getRequestDispatcher("/goto").forward(request,response);
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        request.setAttribute("msg","post haha~");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```

2. **把拦截器注册到容器中**

```java
@Configuration
public class Myconfig3 {

    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
        WebMvcConfigurer adapter = new WebMvcConfigurer() {
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/**") //会拦截静态资源
                        .excludePathPatterns("/success","/goto");
            }
        };
        return adapter;
    }
}

```











### 跨域

### 文件上传

Springboot将文件上传的相关过程封装到了MultipartFile组件中

文件上传的yaml配置前缀：

```yaml
spring:
  servlet:
    multipart:
```

前端：

```html
<form method="post" action="/upload" enctype="multipart/form-data">
    <input type="file" name="file"><br>
     <input type="file" name="files" multiple><br>
    <input type="submit" value="提交">
</form>
```

后端：

```java
    @PostMapping("/upload")
    @ResponseBody
    public String upload(@RequestParam("email")String email,
                         @RequestPart("headImg")MultipartFile headImg,
                         @RequestPart("Imgs")MultipartFile[] imgs) throws IOException {

        log.info("上传的信息：email={},headImg={},Imgs={}",
                email,headImg.getSize(),imgs.length);
        //保存文件到磁盘上
        if(!headImg.isEmpty()){
            headImg.transferTo(new File("d:\\save123\\"+headImg.getOriginalFilename()));
        }
        return "haha";
    }
```



### 异常处理



### 原生Servlet组件

Servlet、Filter、Listener

> 路径中 /* 是原生servlet写法， /** 是spring写法

**使用Servlet API**

@ServletComponentScan(basePackages = **"com.atguigu.admin"**) :指定原生Servlet组件都放在哪里

@WebServlet(urlPatterns = **"/my"**)：效果：直接响应，**没有经过Spring的拦截器？**

@WebFilter(urlPatterns={**"/css/\*"**,**"/images/\*"**})

@WebListener

推荐使用这种方式





**使用RegisterBean**

`ServletRegistrationBean`, `FilterRegistrationBean`, and `ServletListenerRegistrationBean`

不用在类上添加上述注释

```java
@Configuration
public class MyRegistConfig {

    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();

        return new ServletRegistrationBean(myServlet,"/my","/my02");
    }


    @Bean
    public FilterRegistrationBean myFilter(){

        MyFilter myFilter = new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet());
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/css/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MySwervletContextListener mySwervletContextListener = new MySwervletContextListener();
        return new ServletListenerRegistrationBean(mySwervletContextListener);
    }
}
```





### 嵌入式Web容器

+ 默认支持的webServeer
  + `Tomcat`，`Jetty`和`Undertow`

### 定制化原理

### 相关原理

**请求转发过程**

请求 ->DispatcherServlet ->handlermapper ->(mappedHandler{里面包含handler和interceptorlist}) ->handlerAdapter ->(adapter)

**拦截器原理**

1、根据当前请求，找到**HandlerExecutionChain【**可以处理请求的handler以及handler的所有 拦截器】

2、先来**顺序执行** 所有拦截器的 preHandle方法

- 1、如果当前拦截器prehandler返回为true。则执行下一个拦截器的preHandle
- 2、如果当前拦截器返回为false。直接    倒序执行所有已经执行了的拦截器的  afterCompletion；

**3、如果任何一个拦截器返回false。直接跳出不执行目标方法**

**4、所有拦截器都返回True。执行目标方法**

**5、倒序执行所有拦截器的postHandle方法。**

**6、前面的步骤有任何异常都会直接倒序触发** afterCompletion

7、页面成功渲染完成以后，也会倒序触发 afterCompletion

![image-20220808223246533](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20220808223246533.png)

## 数据访问

### SQL

**1.数据源的自动配置**

导入jdbc场景

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
```



**2.使用Druid数据源**



**3.整合MyBatis操作**



**4.整合MyBatis-Plus完成CRUD**

## 单元测试



## 指标监控



# 涉及到的杂知识

## 请求和转发

1、请求次数

重定向是浏览器向服务器发送一个请求并收到响应后再次向一个新地址发出请求，转发是服务器收到请求后为了完成响应跳转到一个新的地址；重定向至少请求两次，转发请求一次；

2、地址栏不同

重定向地址栏会发生变化，转发地址栏不会发生变化；

3、是否共享数据

重定向两次请求不共享数据，转发一次请求共享数据（在request级别使用信息共享，使用重定向必然出错）；

4、跳转限制

重定向可以跳转到任意URL，转发只能跳转本站点资源；

5、发生行为不同

重定向是客户端行为，转发是服务器端行为；



## 查看某个组件的配置

查看该组件的`组件名AutoConfiguration`
