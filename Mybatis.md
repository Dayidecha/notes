

框架：软件开发中的一套解决方案



三层架构

+ 表现层：展示数据的
+ 业务层：是处理业务需求的
+ 持久层：是和数据库交互的



持久层技术解决方案：

​		JDBC技术

​		Spring中的JdbcTemplate：spring对jdbc的简单封装

​		Apache的DBUtils



## MyBatis框架概述

mybatis是**持久层框架**，封装了jdbc操作的很多细节，开发者只需关注sql语句本身。

mybatis通过xml或注解的方式将各种statement配置起来

采用ORM思想解决了实体和数据库映射的问题

> ORM:object relational Mapping：把数据表和实体类的属性对应起来
>
> 让我们操作实体类就可以操作数据库表



## 第一个Mybatis

+ 编写mybatis的核心配置文件
+ 编写mybatis的工具类

+ 编写实体类，Dao接口，接口实现类



注意：每一个mapper.mxl都需要在mybatis核心配置文件中注册

maven资源过滤问题（配置文件不放在resouces文件夹的情况）

```xml
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```



## CRUD

流程：

1. 编写接口

2. 编写对应的mapper.xml中的sql语句
3. 测试

### 1.namespace

namespace包名要和Dao/mapper**接口**的包名一一致



### 2.select

+ id:对应的namespace中的方法名
+ resultType：sql执行语句的返回值
+ parameters(接口中带有方法)：参数类型

```xml
<mapper namespace="com.l.dao.UserDao">
    <select id="getUserList" resultType="com.l.pojo.User" >
        select * from mybatis.user
    </select>
</mapper>
```



###  3.insert

```xml
    <insert id="addUser" parameterType="com.l.pojo.User">
        insert into mybatis.user (id,name,pwd) value (null,#{name},#{pwd})
    </insert>
```

### 4.delete



### 5.update



### 6.万能Map

如果只想要修改某一条数据时的某一项，可以使用**parameterType=“map”**

注意：

+ **增删改需要提交事务**



## 配置解析

### 1.核心配置文件

- configuration（配置）
  - [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties) *
  - [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings) *
  - [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases) *
  - [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
  - [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
  - [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - environments（环境配置） *
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
  - [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

### 2.环境配置（environments)

记住：尽管可以配置多个环境，但每个sqlSessionFactory只能选择一个环境



Mybatis默认的事务管理器是JDBC，连接池：POOLED



### 3.属性（properties)

通过properties属性来实现引用配置文件

```xml
  <!--引入外部文件-->
    <properties resource="db.properties"/>
```

优先级：外部比在properties中高



### 4.别名（typeAlias)

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```



也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

每一个在包 `domain.blog` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 `domain.blog.Author` 的别名为 `author`；若有注解，则别名为其注解值。见下面的例子：

```java
@Alias("author")
public class Author {
    ...
}
```



### 5.设置（setting）

~



### 6.映射器（Mappers)

方式一：用的比较多

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```



方式二：使用class文件绑定注册

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

注意点：

+ 接口和Mapper配置文件必须同名
+ 接口和Mapper配置文件必须在同一个包下

方式三：pakage

```xml
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

注意点：

+ 接口和Mapper配置文件必须同名
+ 接口和Mapper配置文件必须在同一个包下



### 8.生命周期和作用域

+ 理解我们之前讨论过的不同作用域和生命周期类别是至关重要的，因为错误的使用会导致**非常严重的并发问题**。

+ **SqlSessionFactoryBuilder**

+ 这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。 因此 SqlSessionFactoryBuilder 实例的最佳作用域是**方法作用域**（也就是**局部方法变量**）。 

**SqlSessionFactory**

+ 相当于数据库连接池

+ SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 
+ SqlSessionFactory 的最佳作用域是**应用作用域**（全局），很多方法可以做到，最简单的就是使用**单例模式或者静态单例模式**。

**SqlSession**

+ 连接到连接池的一个请求

+ 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例**不是线程安全的**，因此是不能被共享的，所以它的最佳的作用域是**请求或方法作用域**。

+ 用完之后一定要**关闭**！！！！！！！！！！！！！！！

**映射器实例Mapper**

每一个mapper相当于一个业务





## 解决属性名和字段名不一致问题

### ResultMap

结果集映射

```
id name pwd
id name password
```

```xml
    <resultMap id="UserMap" type="user">
        <!--column数据库中的字段，property实体类中的属性-->
       
       <!--  <result column="id" property="id"/>
        <result column="name" property="name"/> -->
         <!--只要写不匹配的那一个字段-->
        <result column="pwd" property="password"/>
    </resultMap>

    <select id="getUserList" resultMap="UserMap">
        select * from mybatis.user
    </select>
```



## 日志

### 日志工程

+ SLF4J
+ LOG4J
+ LOG4J2
+ STDOUT_LOGGING
+ NO_LOGGING



配置日志

```xml
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```



### LOG4J

Log4j是Apache的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是控制台、文件、GUI组件。

使用：

1.导包

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```



2.配置log4j.properties

````properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=【%c】-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/kuang.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=【%p】【%d{yy-MM-dd}】【%c】%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
````



3.在mybatis.config中配置

```xml
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
```



4.使用

```java
    @Test
    public void testLog4j(){
        logger.info("info");
        logger.debug("debug");
        logger.error("error");
    }
```



## 分页

+ 减少数据处理路

使用：

1. 在接口中创建方法

   ```java
       List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. 在mapper.xml中配置

   ```xml
     <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
           select * from mybatis.user limit #{start},#{pagesize}
       </select>
   ```

   

**MyBatis分页插件：PageHelper**





## **使用注解开发**

### 面向接口编程

好处：解耦，可拓展，提高复用。

接口的两类：

+ 第一类是对一个个体的抽象，它可对应为一个抽象体（abstract class);
+ 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface)



```java
package org.mybatis.example;
public interface BlogMapper {
  @Select("SELECT * FROM blog WHERE id = #{id}")
  Blog selectBlog(int id);
}
```

+ 使用注解来映射简单语句会使代码显得更加简洁，但对于稍微复杂一点的语句，Java 注解不仅力不从心，还会让你本就复杂的 SQL 语句更加混乱不堪。 因此，如果你需要做一些很复杂的操作，最好用 XML 来映射语句。



本质：反射机制实现

底层：动态代理





### CRUD



```JAVA
//删除
@Delete("delete from mybatis.user where id = #{id}")
int deleteUser(@Param("id") int id);
```



**关于@Param()注解**

+ 基本类型的参数或者String类型，需要加上
+ 引用类型不需要加
+ 如果只有一个基本类型的话，可以忽略，但建议还是加上



#{}和${}





## 多表查询

### 多对一

```xml
    <resultMap id="EmpAndDeptName" type="Emp">
        <!--复杂的属性，需要单独处理
        对象：association
        集合：collection
        -->
        <association property="dept" javaType="Dept">
            <result property="name" column="name"></result>
        </association>
    </resultMap>
    <select id="getEmpAndDeptName" resultMap="EmpAndDeptName">
        SELECT emp.*,dept.name
        FROM dept,emp
        WHERE emp.dept_id = dept.id
    </select>
```





### 一对多

```xml
<collection property="emp" ofType="Emp">
	<result property="id" column="id"/>
</collection>
```



小结：

1.关联 - association 【多对一】

2.集合 - collection 【一对多】

3.javaType  & ofType

	1. javaType用来指定实体类中的引用类型
	2. ofType用来指定映射到List或集合中的pojo类型，泛型中的约束类型





## 动态SQL

- if
- choose (when, otherwise)
- trim (where, set)
- foreach





### if

```xml
    <select id="queryBlog" parameterType="map" resultType="blog">
        select * from db2.blog
        where 1=1
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author !=null">
            and author = #{author}
        </if>
    </select>
```

### choose、when、otherwise

+ choose相当于java中的switch

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

### trim、where、set

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

+ *where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。



### SQL片段

常用的sql代码段



```xml
<sql id ="if-title-author">
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author !=null">
            and author = #{author}
        </if>
</sql>
```



```xml
    <select id="queryBlog" parameterType="map" resultType="blog">
        select * from db2.blog
 		<where>
        	<include refid="if-title-author"></include>
        </where>
    </select>
```



+ 最好基于单表来定义SQL片段

+ 不要存在where标签



### foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```



## 缓存

简介：