## IDEA使用

对于首次创建或打开的新项目，IntelliJ IDEA 都会创建项目索引，如上图标注 1 所示。大型项目在创建索引过程中可能必须会卡顿，所以建议创建索引过程最好不要动项目。（表现上就是右下角有些进度条在跑）

### 界面UI

打开View->Tool Windows,这里的基本都是一些常用窗口，首先Project项目窗口不用说，这个是必备的，可以查看项目结构，选择文件等；Run和Debug窗口会返回运行结果报错信息等；Version Control版本控制窗口为我们提供了Git的基本操作，Maven Projects窗口同样也很重要，在下面会说到，Maven控制着程序的生命周期，通过这个窗口我们可以管理项目的依赖和插件，打包和运行程序等。

![](http://ww1.sinaimg.cn/large/aa003451gy1g0ma09fx4vj20cz0eymxx.jpg)

### 重要概念

#### Project 和 Module 介绍

Eclipse使用工作空间的概念，同一个窗口管理多个项目，IDEA则与VS的一个窗口一个解决方案类似，一个窗口只打开一个Project

Project下面一级是Module，一个 Project 可以有多个 Module，Module的概念即是VS中的项目，一个解决方案下可以有多个项目或Web。

IDEA的Project是一个没有具备任何编码设置、构建等开发功能的，主要起到一个项目定义、范围约束、规范等类型的效果，也许我们可以简单地理解为就是一个单纯的目录，只是这个目录命名上必须有其代表性的意义。

![](http://ww1.sinaimg.cn/large/aa003451gy1g0ma09f2lcj20mr0gygm1.jpg)

#### 安装SDK

SDK是开发java项目必备的

按 下`Ctrl` + `Shift` + `Alt` + `S` 弹出项目结构设置区，在SDK页进行添加或管理

![](http://ww1.sinaimg.cn/large/aa003451gy1g0ma09fo8rj20sf0ni75n.jpg)

[SDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

### 基础设置

[推荐设置](https://github.com/judasn/IntelliJ-IDEA-Tutorial/blob/master/settings-recommend-introduce.md)

随着IDEA版本的更新，已经有些设置发生了变化，但是基本上位置没变。个人觉得需要修改的有以下内容：

- 代码提示是否区分大小写（默认：首字母区分）
- 自动import包
- 鼠标滚轮控制缩放
- 行号和方法线
- 文件选项卡单层或多层
- 注释斜杠位置（是否空行等）
Code Style -> Java -> Code Generation -> Comment Code
Line comment at first column 注释标记线在第一列（默认勾选）
Add a space at comment start 在注释开始的位置添加一个空格

### 常用快捷键

IDEA的快捷键非常多，且有很多常用的键位已经被搜狗和QQ的不常用功能占用，建议关闭或修改需要的键位

[快捷键](https://github.com/judasn/IntelliJ-IDEA-Tutorial/blob/master/keymap-introduce.md)

## SSM框架

### Spring Framework

Spring是一个开源的Java／Java EE全功能栈（full-stack）的应用程序框架，以Apache License 2.0开源许可协议的形式发布，也有.NET平台上的移植版本。该框架基于 Expert One-on-One Java EE Design and Development（ISBN 0-7645-4385-7）一书中的代码，最初由Rod Johnson和Juergen Hoeller等开发。

Spring Framework提供了一个简易的开发方式，这种开发方式，将避免那些可能致使底层代码变得繁杂混乱的大量的属性文件和帮助类。 简单来说，Spring是一个轻量级的 **控制反转（IoC）** 和 **面向切面（AOP）** 的容器框架。
如果学习JAVA Spring，这两个东西应该是绕不开的，但是暂时理解不深刻，后面准备深入到代码实现层面，与.NET进行一些比较。

### Spring MVC

Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring MVC 分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

#### spring boot

Spring Boot就是Spring,它做了那些没有它你也会去做的Spring Bean配置。它使用“习惯优于配置”（项目中存在大量的配置，此外还内置了一个习惯性的配置，让你无需手动进行配置）的理念让你的项目快速运行起来。使用Spring Boot很容易创建一个独立运行（运行jar,内嵌Servlet容器）、准生产级别的基于Spring框架的项目，使用Spring Boot你可以不用或者只需要很少的Spring配置。

##### 正常的Spring MVC构建需要

- 一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件，最起码要有Spring MVC和Servlet API这些依赖。
- 一个web.xml文件（或者一个WebApplicationInitializer实现），其中声明了Spring的DispatcherServlet。
- 一个启动了Spring MVC的Spring配置
- 一控制器类，以“hello World”相应HTTP请求。
- 一个用于部署应用程序的Web应用服务器，比如Tomcat。

##### spring boot 特点

- 自动配置：针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置
- 起步依赖：告诉Spring Boot需要什么功能，它就能引入需要的库。
- 命令行界面：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。
- Actuator：让你能够深入运行中的Spring Boot应用程序，一套究竟。

##### 使用Spring Boot的方法之一：Spring Initializr

- 通过Web构建 http://start.spring.io/

![enter image description here](http://upload-images.jianshu.io/upload_images/1637925-8fd3e8f13ba45de6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- IDEA

 1. 先选择SDK

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8qcfxe0pj20jd0h2dgk.jpg)

 2. 然后填写项目的基本信息

groupid和artifactId被统称为“坐标”，是为了保证项目唯一性而提出的，如果你要把你项目弄到maven本地仓库去，你想要找到你的项目就必须根据这两个id去查找。

groupId第一段为域，如：org、com、cn等，第二段为公司名称或作者名称等，artifactId一般与项目有关

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8qd147zxj20jq0h7gm8.jpg)

 3. 选择需要的依赖

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8qd8k36jj20n70h7dgl.jpg)

### 1.3 Mybatis

MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。MyBatis是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAO）MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的POJOs（Plain Old Java Objects，普通的 Java对象）映射成数据库中的记录。


#### 1.3.1 和Hibernate的区别

##### 从性能角度考虑
由于 Hibernate 比 MyBatis 抽象封装的程度更高，理论上单个语句执行的性能会低一点。

但 Hibernate 会设置缓存，对于重复查询有一定的优化，而且从编码效率来说，Hibernate 的编码效果肯定是会高一点的。所以，从整体的角度来看性能的话，其实两者不能完全说谁胜谁劣。

##### 从ORM角度考虑
Hibernate 是完备的 ORM 框架，是符合 JPA 规范的，但 MyBatis 不是。MyBatis 比单纯写 JDBC 肯定是方便一点，但无可避免还是要写SQL，且无法做到跨数据库 。Hibernate 使用 JPA 就可以无需考虑数据库的兼容性问题。

使用 Hibernate 的一个难点是，如何来设计对象之间的关系。如果是关系型数据库的话，表和表是通过外键来进行关联的。而在 ORM 中，则需要从面向对象的角度出发，来设计对象之间的关联关系。这个是需要思路上做一个转变的。

## Maven

**Apache Maven**，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（Project Object Model，POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤（生命周期）。

.NET通过NuGet进行包管理，使用MSBuild进行生产，
java通过Maven实现这两个事情，除了负责管理项目模板，Maven还控制项目的打包(jar包等)及生命周期，而这些操作都是依赖于JDK的

![](http://ww1.sinaimg.cn/large/aa003451gy1g0ma09ewyaj208l0973yh.jpg)

Visual Studio按照标准提供了基于.sln文件+csproj文件的项目模板。

Java平台的编译器的编译配置是xml文档，由于Java官方没有项目模板，IDE只负责帮你组织项目，但是并没有模板，你可以将任意目录指定为SourceRoot（代码根目录），ResourceRoot（资源文件根目录：比如配置文件）也可以任意指定，编译的时候，IDE会将你的项目代码，以及编译器所需要的编译描述/配置xml文档告诉编译器该如何编译你的项目。确实非常灵活，但是也增加了项目管理的成本。

### pom.xml

``` xml
<project>
   <modelVersion>4.0.0</modelVersion>

   <groupId>com.example</groupId>
   <artifactId>demo</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <packaging>jar</packaging>

   <name>demo</name>
   <description>Demo project for Spring Boot</description>

   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.1.0.RELEASE</version>
       <relativePath/> <!-- lookup parent from repository -->
   </parent>

   <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
       <java.version>1.8</java.version>
   </properties>
 <!-- 添加classpath依赖 -->
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   <!-- 开发者工具，当classpath下有文件更新自动触发应用重启 -->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
 <!-- maven编译插件，用于创建可执行jar包 -->
   <build>
       <plugins>
           <plugin>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
           </plugin>
       </plugins>
   </build>
</project>
```

## 项目启动

### 连接数据库

1. 添加依赖

在pom.xml中添加MyBatis与相应数据库的依赖

```xml
<!-- MyBatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>

<!--mysql-->
<dependency>
	 <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>8.0.13</version>
</dependency>

<!--Oracle-->
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc6</artifactId>
    <version>11.2.0.2.0</version>
    <!-- 下面两行一定要加上 -->
    <scope>system</scope>
    <systemPath>${project.basedir}/src/main/resources/lib/ojdbc6.jar</systemPath>
</dependency>
```
添加依赖之后，IDEA会自动导入相应的包

注意MySQL的Version节点中的版本号要与你使用的MySQL驱动的版本一致，8.0左右的高版本在连接串设置上也与低版本不同，会在下文说明

Oracle的情况比较特殊，有两个地方
 - 简单的添加依赖是无法引用的，因为Maven没有Oracle的授权，所以不能自动添加jar包，我们要自己去Oracle的目录拷贝一份，11g的目录如下E:\\app\\{user}}\\product\\11.2.0\\dbhome_1\\jdbc\\lib\\ojdbc6.jar,容易混淆的一点是ojdbc6.jar才是最新的版本，ojdbc14.jar则是更早的版本，具体的版本说明可以看这篇文章

 [OJDBC版本区别](https://blog.csdn.net/baidu_37107022/article/details/75095514)

- 添加依赖之后还要在plugin节点下添加一个配置节点,不然在打包时会出现问题

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <!--必须加上下面的配置，在jar包运行的时候找不到oracle的驱动类-->
    <configuration>
        <includeSystemScope>
            true
        </includeSystemScope>
    </configuration>
</plugin>
```

2. 连接串

连接串有两种写法，但是其实都是在application.properties这个文件里，但是可以把这个文件的后缀改为**.yml**使用，这样就是application.yml

- application.properties

```
spring.datasource.url=jdbc:mysql://localhost:3306/test?characterEncoding=utf-8&useSSL=false&useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=limingxu
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```
- application.yml

![](https://img2018.cnblogs.com/blog/1368608/201811/1368608-20181125150204883-276581902.png)

写法的作用是一样的，我没用使用yml格式的，所以找了用了别人的图，要注意的是url后面的那一串，这就是前面说的数据库连接问题，正常使用5.x版本的MySQL可以直接使用这种普通的连接串，但是我一开始不知道，用了最新的8.x版本，直接连接数据库会报错，要像application.properties里面的写法在后面通过get传值的方式加上一串说明，具体作用还没有深入研究

### 三层结构

三层的结构如图，我的理解是DataObject→Mapper→Service→Controller

![](http://ww1.sinaimg.cn/large/aa003451gy1fxoybvx39xj206v09bjre.jpg)

- DataObject
建立与数据库表对应的类（javabean）

```java
package com.example.dataobject;

public class User {
    private Long id;
    private String name;
    private Integer age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public User(){
    }
}
```
这里除了相应的属性，还要添加get/set方法，实例化的对象才能获取相应属性的值，IDEA的快捷键是**ALT+INSERT**

- Mapper

Mapper中的SQL语句的位置有两种情况，比较常用且不不会更改的语句直接写在程序里，另一些则是以特殊的格式写在XML文件，这样会使得对数据源的控制更加灵活，这样即使是在运行中的网站也可以修改获取的数据，而不需要重新编译生成整个项目，这种写法也是得益于Mybatis，在application.properties中还需要一行配置映射到xml文件

```java
package com.example.datamapper;

import com.example.dataObject.User;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface UserMapper {
    @Select("SELECT * FROM USER WHERE NAME = #{name}")
    User findByName(@Param("name") String name);

    @Select("SELECT * FROM USER WHERE ID = #{id}")
    User getById(@Param("id") Long id);

    @Insert("INSERT INTO USER(NAME, AGE) VALUES(#{name}, #{age})")
    int insert(@Param("name") String name,@Param("age") Integer age);

    // XML写法 与xml文件配合使用
    User selectByPrimaryKey(String userId);
}
```

```xml
<select id="selectByPrimaryKey" resultMap="BaseResultMap" parameterType="java.lang.String" >
    select <include refid="Base_Column_List" /> from EP_CONFIG where CONFIG_ID = #{configId,jdbcType=VARCHAR}
</select>
```

```
mybatis.mapperLocations=classpath:mapper/*.xml
```

- Service

```java
package com.example.service;

import com.example.dataMapper.UserMapper;
import com.example.dataObject.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {
    @Autowired
    UserMapper userMapper;

    public User findUser(Long id){
        User user = userMapper.getById(id);
        return  user;
    }

    public User findUserByName(String name){
        User user = userMapper.findByName(name);
        return  user;
    }
}

```
- Controller

```java
package com.example;

import com.example.dataObject.User;
import com.example.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class DemoController {
    @Autowired
    UserService userService;

    @RequestMapping("/")
    @ResponseBody
    public String index(){
        return "Hello Spring Boot";
    }

    @RequestMapping("/user/{id}")
    @ResponseBody
    public String getUser(@PathVariable("id") Long id){
        User user = userService.findUser(id);
        return user.getName();
    }

    @RequestMapping("/username/{name}")
    @ResponseBody
    public User getUserByName(@PathVariable("name") String name){
        User user = userService.findUserByName(name);
        return user;
    }
}
```

### 构建启动的三种方式

1. 在IDE（或者命令行工具中的java）启动main函数，IDE中一般都自带Maven，能够帮助我们下载安装Maven依赖。

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8tn3nrrij20gg0a8mxu.jpg)
2. 运行mvn spring-boot:run命令，但是此种方法要求你在本地环境中必须安装Maven
使用命令行有两种方式，一种是直接找到Maven项目视图中的spring boot启动命令直接运行

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8toc327hj20cy0cr757.jpg)

![](http://ww1.sinaimg.cn/large/aa003451gy1fx9qcflij0j20e20ch755.jpg)
另一种是直接输入命令
![](http://ww1.sinaimg.cn/large/aa003451gy1fx9qeqeclbj20l306vgly.jpg)
3. 使用mvn package命令进行打包，生成一个可以直接运行的 JAR 文件，路径是项目文件中的target目录，使用“java -jar”命令就可以直接运行。

![](http://ww1.sinaimg.cn/large/aa003451gy1fx9qs8geknj20eb097t8v.jpg)

![](http://ww1.sinaimg.cn/large/aa003451gy1fx9qky9vtkj20r70e4jry.jpg)

不管使用哪种方法都可以启动项目，然后访问http://localhost:8080 就可以打开网站看到控制器里面的内容了

### 应用入口

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class DemoApplication {
    @RequestMapping("/")
    public String index(){
        return "Hello Spring Boot";
    }
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

对main函数所在的类进行修改，时期能接收Http请求并返回内容。

- main()方法启动了一个HTTP服务器程序，这个程序默认监听8080端口，并将HTTP请求转发给我们的应用来处理
- @Controller标注表示Application类是一个处理HTTP请求的控制器，该类中所有被@RequestMapping标注的方法都会用来处理对应URL的请求，@ResponseBody标注告诉Spring MVC直接将字符串作为Web响应（Reponse Body）返回，如果@ResponseBody标注的方法返回一个对象，则会自动将该对象转换为JSON字符串返回
- index()方法上包含@GetMapping("/")标注，意味着在浏览器中访问http://localhost:8080/（不考虑协议、host和port信息后的路径为"/"）后浏览器发送的请求会交给该方法进行处理
- index()方法直接返回一个字符串，那么相当于直接将字符串"Hello World"作为HTTP请求的响应返回给浏览器，于是我们在浏览器中可以看到相应的返回值

- 一个启动了Spring MVC的Spring配置
- 一控制器类，以“hello World”相应HTTP请求。
- 一个用于部署应用程序的Web应用服务器，比如Tomcat。
