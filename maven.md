# Maven简介

项目构建工具

## Maven基础概念

**仓库**

+ 本地仓库
+ 远程仓库
  + 中央仓库：maven团队维护
  + 私服：公司自己的服务器

**坐标**

用于定位仓库在资源的位置

组成：

+ groupid：Maven项目所属的组织名称
+ artifactid：Maven项目的名称
+ version：项目的版本号

> packagingn：项目打包方式

## Maven本地仓库配置

默认本地仓库创建在 c:\user\username\.m2\repository中

**配置本地仓库**

修改maven安装目录中的conf/settings.xml

```xml
<localRepository>d:\maven\repository</localRepository>
```

**配置镜像仓库**

修改maven安装目录中的conf/settings.xml

```xml
<mirror>
      <id>nexus-aliyun</id>
     <!-->表示是中央仓库的镜像<-->
      <mirrorOf>central</mirrorOf>
      <name>Nexus aliyun</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
```

上面修改的是全局setting



# 第一个Maven项目

## Maven工程目录结构

project

+ src
  + main
    + java：java源程序
    + resources：java配置文件 
  + test
    + java：java源程序
    + resources：java配置文件 
+ pom.xml

## 构建命令

```shell
mvn compile #编译
mvn clean #清理 把编译生成的target目录删了
mvn test #测试
mvn package #打包
mvn install #安装到本地版本库
```



## 插件创建工程

略



## 在IDEA中配置Maven

### **配置Maven**

使用自己下载的maven版本：

在file-setting-搜索maven-略

### 添加插件

```xml
<build>
	<plugins>
    	<plugin>
        	对应的插件
        </plugin>
    </plugins>
</build>
```



### pom中的一些标签的作用

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!--指定pom版本-->
  <modelVersion>4.0.0</modelVersion>
	<!--打包方式，web工程打包为war,java工程打包为jar-->
  <packaging>war</packaging>
    
  <groupId>org.example</groupId>
  <artifactId>JavaPractice2</artifactId>
    <!--版本号:repease为正式发布的版本，snapshot为开发版本-->
  <version>1.0-SNAPSHOT</version>


  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>


</project>
```



# 依赖管理

## 依赖配置

+ 依赖指当前项目运行所需的jar，一个项目可以设置多个依赖

## 依赖传递

+ 依赖具有传递性
  + 直接依赖
  + 间接依赖

**问题：依赖冲突**

+ 路径优先（就近原则）：当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高。（如A依赖B，B依赖C，B依赖D_V_1，C依赖D_V_2，则B中的D覆盖C中的D）

+ 声明优先：同级谁先配谁优先

+ 特殊优先：在同一个pom中后配置的依赖**覆盖**前配置的依赖

**可选依赖**：对外隐藏当前依赖的资源

```xml
<dependency>
	<groupid>junit</groupid>
    <artifactId>junit</artifactId>
	<version>4.12</version>
    <optional>true</optional>
</dependency>
```

**排除依赖**：主动排除依赖的资源，被排除的资源不需要指定版本

```xml
<dependency>
	<groupid>junit</groupid>
    <artifactId>junit</artifactId>
	<version>4.12</version>
    <exclusions>
    	<exclusion>
        	<groupId>123</groupId>
        	<artifactId>456</artifactId>
        </exclusion>
    </exckusions>
</dependency>
```

## 依赖作用范围

+ 依赖的jar默认可以在任何地方使用，可以通过scope标签设定其作用的范围

+ 组用范围

  + 主程序范围（main文件范围内）
  + 测试程序范围（test文件夹范围）
  + 是否参与打包（package指令范围）

  ![image-20220622231542715](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20220622231542715.png)

```xml
<dependency>
	<groupid>junit</groupid>
    <artifactId>junit</artifactId>
	<version>4.12</version>
	<scope>runtime</scope>
</dependency>
```

作用范围在依赖传递时取交集

![image-20220622232112238](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20220622232112238.png)
