## 杂知识

`netstat - ano`

`URI`统一资源定位符

`URL`统一资源标识符



## JS



Global：

 1. 特点：全局对象，直接 `方法名（）`就能调用

 2. 方法：

    encodeURI()：URL编码

    parseInt()：把是数字的部分转为数字

    isNaN()：判断释放是数字

    eval(string)：把字符串当js脚本执行

3. URL编码

   %E4%BC



## XML

### 概念

**Extensible Markup Language 可扩展标记语言**

+ 可扩展：标签都是自定义的

**功能**

+ 存储数据
  + 配置文件
  + 在网络中传输

**XML和HTML的区别**

1. xml标签都是自定义的
2. xml语法严格
3. xml是存储数据的，html是展示数据的

### 语法

1. xml有且只有一个根标签
2. 第一行必须定义为文档声明
3. 属性值必须用引号引起来
4. xml区分大小写
5. 标签必须封闭

**组成部分**

1. 文档声明 

+ `<?xml 属性列表?>`
+ 属性列表：
  + version：版本号
  + encoding：编码方式，告知解析引擎当前文档使用的字符集
  + standalone：是否独立

2. 属性：

   id值唯一

3. 文本：`cdata`区中的数据会原样展示

   ```xml
   <![CDATA[
   	内容
   ]]>
   ```

4. 约束

   + 规定xml文档的书写规则

   + 能在xml中引入约束文档

   + 分类

     + DTD：一种简单的约束技术

       引入dtd文档到xml文档中

       + 内部dtd：将约束规则定义在xml文档中
       + 外部dtd：将约束的规则定义在外部的dtd文件中
         + 本地：`<!DOCTYPE> 根标签名 SYSTEM 'dtd文件位置'>`

     + Schema：一种复杂的约束技术

### 解析

操作xml文档，将文档的数据读取到内存中



**解析方式**

1. DOM：将标记语言一次性读取到内存
   + 优点：
     + 操作方便，能对文档进行CRUD的操作
   + 缺点：占内存
2. SAX：逐行读取，基于事件驱动的。
   + 优点：不占内存
   + 缺点：只能读取
3. 常见的解析器
   + JAXP：sun公司提供，没人用
   + DOM4J
   + Jsoup：
   + PULL：android内置的
4. Jsoup
   + 使用：
     1. 导入jar包
     2. 获取Document对象
     3. 获取对应的Element对象
     4. 获取数据

5. jsoup的对象
   + `document Jsoup.parse(File in,String 'utf-8')`
   + `document Jsoup.parse(URL url,String int timeoutMills)`
   + `element/s documen.getElement/sByTag`
   + `element element.getElementById`获取子元素
   + `Value element.attr(key)`获取属性值
6. Node:节点对象
   + 是Document和Element的父类



**快捷查询方式**

1. selector
   + Elements select(String cssQuery)

2. XPath

   + xml的路径语言

   + 需要额外导入jar包
   + `JXDocument jxDocument = new jxdocument(document)`
   + `List<jxnode> selN(String xpath)`
   + `object selOne(String xpath)`





## web服务器软件

web服务器软件：接收用户请求，处理请求，做出响应

常见的java相关web服务器软件：

+ webLogic：oracle公司，收费，支持所有的JavaEE规范
+ webSphere
+ JBOSS
+ Tomcat：Apache基金组成，中小型JavaEE服务器，仅仅支持少量JavaEE规范



**Tomcat：web服务器软件**

1. 下载
2. 安装
3. 卸载
4. 启动
5. 关闭
6. 配置
7. 部署
   + 将项目打包成war包，放置到webapps目录下
     + war包会自动解压缩

8. 部署2 

   + 在server.xml的host标签体中添加

   + 需要重新启动服务器

     ```xml
     <!-- 虚拟目录 -->
     <Context docBase="D:\hello"  path="/hehe" /> 
     ```

9. 部署3

   + 在`config\catalina\localhost`中创建`xxx.xml`文件

   + 在文件中输入

     ```xml
     <Context docBase="D:\hello"/>
     ```

   + 虚拟路径就是`xxx.xml`中的`xxx`

   + **热部署**

   + **推荐**



### 目录结构

-- 项目根目录

 	-- WEB-INF目录：

​			-- web.xml：web项目的核心配置文件

​			-- classes目录：放置字节码文件目录

​			-- lib目录：放置依赖的jar包



## Servlet

**server applet**

+ 概念：运行在服务器端的小程序
+ servlet就是一个接口，定义了Java类被浏览器访问到（tomcat识别）的规则。

**快速入门：**

1. 创建JavaEE项目
2. 定义一个类，实现Servlet接口
3. 实现接口的抽象方法
4. 配置Servlet

```xml
    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>com.example.day11_tomcat.ServletDemo1</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
```



**servlet执行原理**

![image-20210409144852269](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210409144852269.png)



1. 服务器接收到客户端浏览器请求后，会解析url，获取访问servlet的资源路径
2. tmocat会查web.xml文件，是否有对应的<url-pattern>标签内容
   + servlet3.0支持注解`@webServket(name = "",value="/hellos-servlet")`配置，不用xml
3. 如果有，则找到对应的<servlet-class>全类名，
4. tomcat会把字节码文件加载进内存，并创建其对象（反射）
5. 调用方法



**Servlet生命周期**

+ init()：初始化

  + 可指定servlet的创建时间

    `<load-on-startup>5</load-on-startup>`：-1为默认，0-10为服务器创建时调用

  + servlet是单例的，多个用户同时访问时，可能存在线程安全问题

  + 解决：尽量不要定义成员变量

+ service()：提供服务方法，每次被访问执行一次
+ destory()：服务器正常关闭时执行（servlet销毁前）



**IDEA 与 tomcat 相关配置**

1. IDEA会为每个tomcat部署的项目单独创建一份配置

   ```
   Using CATALINA_BASE:   "C:\Users\y\AppData\Local\JetBrains\IntelliJIdea2020.3\tomcat\a629a3f6-ced3-43aa-b746-eda24bad155f"
   
   ```

2. 工作空间项目 和 tomcat部署的web项目

   + tomcat访问的是**tomcat部署的web项目**
   + `web-info`的资源无法访问



**Servlet体系结构**

Servlet

​	|

GenericServlet

​	|

HttpServlet:：对http协议的一种封装



**Servlet相关配置**

路径定义规则：

1. /xxx
2.  /xxx/xxx
   + /*  通配符，写什么都匹配
3. *.do



## HTTP

定义了客户端和服务端通信时，发送数据的格式

**特点：**

	1. 基于TCP/IP的高级协议
	2. 端口号：80
	3. 基于请求/响应模型的：一次请求一次响应
	4. 无状态的：每次请求相互独立，请求之间不能交互数据

**历史版本：**

+ /1.0：每一次请求响应都会建立新的连接
+ /1.1：副用连接，有个定时器，超时断开



### **请求消息数据格式**

1. 请求行

   请求方式 	请求url 	请求协议/版本

   + 请求方式
     1. get
        + 请求url长度有限制
     2. post
        + 请求url长度没有限制

2. 请求头

   请求头名称：请求头值

   常见请求头：

   + Referer：http://localhost/login.html

     + 作用：

       + 防盗链

       + 统计

     

3. 请求体

   key=value





```
GET /login.html HTTP/1.1
Accept: image/avif,image/webp,image/apng,image/svg+xml,image/*,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Connection: keep-alive
Cookie: BIDUPSID=6B887F63823DC43B71DA50A2BFC8035C;
PSTM=1592121182;
PHPSESSID=4fpm3eeu2tnkm026gc5euvn230; 
Host: sp0.baidu.com
Referer: https://www.baidu.com/
sec-ch-ua: "Google Chrome";v="89", "Chromium";v="89", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
Sec-Fetch-Dest: image
Sec-Fetch-Mode: no-cors
Sec-Fetch-Site: same-site
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.114 Safari/537.36
```





### 响应消息数据格式

1. 响应行

   HTTP/2.0 200 OK

   响应状态码：

   	1. 1xx：服务器接收客户端消息，但没接收完，等待一段时间后发送，问客户端是否还有数据
   	2. 2xx：成功
   	3. 3xx：重定向： 301（永久重定向，能缓存），302（临时重定向），304（访问缓存）
   	4. 4xx：405（请求方式没有对应的方法）
   	5. 5xx：500（服务器内部异常）

2. 响应头

   + content-type:text/html;chartset=utf-8：告诉浏览器本次响应体的数据格式以及编码方式

   + attachment;filename =xxx:以附件的形式打开响应体，文件下载

     `content-disposition:attachment;filename=xxx`

3. 响应体

```
accept-ranges: bytes
age: 4010269
cache-control: max-age=315360000
content-encoding: gzip
content-length: 32738
content-type: application/javascript
date: Fri, 09 Apr 2021 17:03:02 GMT
etag: "16947-5bbe6195dc080"
expires: Thu, 20 Feb 2031 07:05:13 GMT
last-modified: Mon, 22 Feb 2021 05:30:26 GMT
ohc-cache-hit: nn3un64 [4]
ohc-response-time: 1 0 0 0 0 0
server: JSP3/2.0.14
vary: Accept-Encoding,User-Agent
```





## Request和Response

![image-20210409165133423](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210409165133423.png)



+ request和response对象由服务器创建



### Request

功能：

1. 获取请求消息的数据

   1. 获取请求行数据

      ```
      * GET /day14/ demo1?name=zhangsan HTTP/1.1
      * 方法:
      1．获取请求方式:GET
      	* string getMethod()
      2．获取虚拟目录:/day14  (*********)
      	* string getcontextPath()
      3．获取servlet路径:/demo1
      	* string getservletPath()
      4．获取get方式请求参数:name=zhangsan
      	* string getQuerystring()
      5．获取请求URI :/day14/demo1  (*********)
      	* String getRequestURI():/day14/demo1
      	* StringBuffer getRequestURL():http://localhost/day14/demo1
      6．获取协议及版本:HTTP/1.1
      	* string getProtocol()
      7. 获取客户机的IP地址
      	* String getRemoteAddr()
      ```

      ​	

   2. 获取请求头

      ```
      String getHeader(String name) 
      
      Enumeration<String> getHeaderNmaes() 获取所有请求头
      ```

      

   3. 获取请求体（post)

      步骤：

       1. 获取流对象

          + `BufferdReader getReader()` ：获取字符输入流
          + `SerletInputStream getInputStream()`：获取字节流

       2. 从流对象中拿数据

          



**************其他功能（常用）：**********************

1. **获取请求参数的通用方式，无论get或Post**

   + `String getParameter(String name)`	根据参数名获取参数值

   + `String getParameterValues(String name)`根据参数名称获取参数数组值

   + `Enumeration<String> getParameterNames()` 获取所有请求的参数名称

   + `Map<String,String[]> getParameterMap()`获取所有键值对

     + 可配和`BeanUtils.populate(object,map)`直接封装成对象
     + 需要导入**BeanUtils**包
     
     ​	

2. **请求转发：一种在服务器内部资源跳转**

   1. 步骤

      1. 通过request获取请求转发器对象：`RequestDispatcher getRequestDispatcher(String path)`

      2. 通过`RequestDispatcher`对象的`forward(ServletRequest,ServletResponse)`来转发

   2. 特点：

      + 浏览器地址栏路径不发生变化
      + 只能转发到服务器内部的资源中
      + 转发是一次请求

3. 共享数据

   + 域对象：一个有作用范围的对象，可以在范围内共享对象
   + requests域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
   + 方法：
     1. `setAttribute(String name,Object obj)`
     2. `Object getAttribute(String)`
     3. `void removeAttribute(String)` ：通过键移除键值队

4. 获取ServletContext

5. 中文乱码问题

   + tomcat 8 之后已经把**get**的乱码问题解决了
   + post在获取参数方法前面加`request.setCaraterEncoding("utf-8")`





**登录验证练习**

![image-20210409231745448](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210409231745448.png)

要求：

1. 编写login.html页面
2. 使用Druid连接池技术，操作db4中的user表
3. 使用JDBCTemplate封装JDBC
4. 登录成功/失败跳转到相应页面
5. ~



开发步骤：

1. 创建项目，导入静态资源，配置文件，jar包
2. 创建数据库环境



遇到的问题：

+ jar包存放的地方要叫**lib**文件夹，libs都不行



**BeanUtils工具类，用于简化数据的封装**

+ JavaBean一般放在domain文件夹里
+ 方法：
  + `popupate()`





### RespOnse

功能：设置响应消息

 1. 设置响应行

    `setStatus(int sc)`

	2. 设置响应头：

    `setHeader(String name,String value)`

	3. 设置响应体



**forward和redirect的区别**

重定向特点：

1. 地址栏变化
2. 可以访问其他站点（服务器）的资源
3. 2次请求，不使用request共享数据

`response.sendRedirect(path)`

转发的区别：

1. 地址栏不变
2. 只能访问本服务器
3. 1次请求，使用request共享数据



**路径的写法**

分类：

 1. 相对路径：通过相对路径不可唯一确定资源

    `./index.html`，不写默认`./`

    规则：确定访问的当前资源和目标资源的相对位置关系

 2. 绝对路径：通过绝对路径可以确定唯一资源

    + 以`/`开头的路径

    `/day15/responseDemo`

    规则：判断定义的路径给谁用的？

    + 给客户端浏览器用：需要加虚拟目录(项目的访问路径)
    + 给浏览器用：

动态获取虚拟目录：

`String contextPath = request.getContextPath()`



**简便的设置编码**

`response.setContentType("text/html;charset=utf-8")`





## ServletContext对象

概念：代表整个web应用，可以和应用程序的容器（服务器）来通信

功能：

 	1. 获取MIME类型：
 	 + 一种互联通通信过程中定义的一种文件数据类型
 	 + 格式：大类型/小类型
 	 + 获取：`String getMimeType(String file)`
 	2. 域对象：共享数据（少用）
 	 + 范围：所有用户所有请求的数据
 	3. 获取文件的真实（服务器）路径
 	 + 获取web目录下的资源访问
 	   + `context.getRealPath("/b.txt")`
 	 + 获取src目录下的资源文件
 	   + `context.getRealPath("/WEB-INF/classes/b.txt")`
 	 + 获取/WEB-INF目录下的资源文件
 	   + `context.getRealPath("/WEB-INF/b.txt")`

获取：

1. `request.getServletContext()`
2. `this.getServletContext()`



**文件下载含有中文名问题**

有工具类：**DownLoadUtils**包

```java
判断浏览器
String filename = URLEncoder.encode(filename,"utf-8")
```





## 会话技术

会话：一次会话包含多次请求和响应

功能：在一次会话的范围内共享数据

方式：

+ 客户端会话技术：cookie
+ 服务端会话技术:session

### Cookie

概念：客户端会话技术，将数据保存到客户端

使用步骤：

 1. 创建Cookie对象，绑定数据

    `new Cookie(String name,String value)`

 2. 发送Cookie对象

    `response.addCookie(Cookie cookie)`

 3. 获取Cookie,拿到数据

    `Cookies[] request.getCookies()`

细节：

+ 一次可以发送多个cookie
+ 保存时间：
  + 默认情况下，浏览器关闭时，cookie数据被销毁
  + 持久化存储
    + `cookie.setMaxAge(int seconds)`
      + 正数，将Cookies写入到硬盘中seconds秒
      + 默认值
      + 0：删除cookie信息

+ tomcat 8之后能存中文数据

+ cookie共享问题：

  + 一个服务器中部署多个项目时，默认不共享cookies
    	+ `setPath(string path)`：设置cookie的获取范围，默认情况下设置当前虚拟目录
    	+ `cookie.setPath("/");`共享
  + 不同的服务器之间共享cookie
    + `setDomain(String path)`设置域名
    + `setDomain(".baidu.com")` 则tieba.baidu.com和news.baidu.com共享

+ cookie的特点

   	1. 不太安全
   	2. 浏览器对单个cookie的大小有限制（4kb)，对同一个域名下的总cookie数量有差异（20）
   	       	3. 一般用于存储少量不太敏感的数据
   	     	4. 在不登录情况下，完成服务器对客户端身份的识别

+ 小问题

  + 不支持空格

  + 解决：

    `URLencoder.encode(str,charset)`

案例：记住上一次访问的时间

### Session

概念：服务器端会话技术，将数据保存到客户端



快速入门：

+ 获取HttpSession对象

  `HttpSession session = request.getSession()`

+ 使用HttpSession对象：
  + `Object getAttribute(String name)`
  + `void setAttribute(String name,Object value)`
  + `void removeAttribute(String name)`

+ Session的实现依赖于

  ​	服务器第一次响应时`response`发送`set-cookie`存储Session对象的消息

  ​	客户端第二次请求时`request`携带`Cookie发送给服务端器

  ```
  Set-Cookie: JSESSIONID=510C53D3A334EBFE0F4E95D0180B7D1F;
  JSESSIONID=510C53D3A334EBFE0F4E95D0180B7D1F
  ```

  

细节：

 1. 客户端关闭后，服务器没关闭，默认获取的不是同一个Session

    + 如果期望客户关闭后，Session也相同

      ```java
      //一个小时内时同一个Session
      Cookie c = new Cookie("JSESSIONID",session.getId())
      c.setMaxAge(60*60)
      ```

	2.  客户端不关闭，服务器关闭后，两次session不是同一个

     + 不是同一个，但是要确保数据不丢失

       + session钝化

         + 在服务器正常关闭之前，将session对象序列化到硬盘上

         

       + session活化
         
  + 在服务器启动前，将session文件转化为session对象即可
    
       > tomcat目录下的word存储服务器运行过程中的动态数据
       >
   > tomcat会自动钝化和活化session

3.  session失效时间(什么时候被销毁）
    
 1. 服务器关闭
    
 2. 调用销毁方法`invalidate()`
    
 3. session默认失效时间为30分钟
    
    > 在tomcat目录下的web.xml中的<session-config><session-timeout>30</session-time></session-config>中修改
    
     

session特点：

1. session用于存储一次会话中多次请求的数据，存在服务器端
2. session可以存储任意类型，任意大小的数据



session和Cookie的区别

1. session存储在服务器，Cookie在客户端
2. session没有数据大小限制，Cookie有
3. session数据安全，Cookie相对不安全





## MVC

+ M：Model
  + 完成具体的业务操作：如查询数据库，封装对象
+ V：View
  + 展示数据
+ C：Controller
  + 获取用户输入
  + 调用模型
  + 将数据交给视图展示

![image-20210411204928308](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210411204928308.png)



## Filter 过滤器

+ 一般用户完成通过操作。如：登录验证、统一编码处理、敏感字符过滤...



+ 使用步骤：

  1. 定义类，实现接口Filter

  2. 覆写方法

  3. 配置拦截路径

     + web.xml配置

     + 注解

       `@webFilte("/*")` 访问所有资源之前都执行该过滤器

  4. 放行

     `filterChain.doFilter(servletRequest,servletResponse)`

## Listener 监听器

web三大组件之一

+ 事件监听机制
  + 事件：一件事情
  + 事件源：事件发送的地方
  + 监听器：一个对象
  + 注册监听：将事件，事件源，监听器绑定在一起。当事件源发送某个事件后，执行监听器代码



+ ServletContextListener



## **Maven**

仓库：本地仓库，远程仓库，中央仓库



项目组成：

+ 核心代码部分
+ 配置文件部分
+ 测试代码部分
+ 测试配置部分