# Java开发学习心得（一）：SSM环境搭建

有一点.NET的开发基础，在学校学过基础语法，对JAVA有点兴趣，就简单学习了一下，记录一下从哪些方面入手的，暂时不打算深入到原理方面，先简单搭下环境看看，所以有些地方可能讲得不慎准确。

## 1 SSM框架

从网上的讨论来看，SSM框架似乎正在慢慢被Spring Cloud的取代。

### 1.1 Spring Framework

Spring是一个开源的Java／Java EE全功能栈（full-stack）的应用程序框架，以Apache License 2.0开源许可协议的形式发布，也有.NET平台上的移植版本。该框架基于 Expert One-on-One Java EE Design and Development（ISBN 0-7645-4385-7）一书中的代码，最初由Rod Johnson和Juergen Hoeller等开发。

Spring Framework提供了一个简易的开发方式，这种开发方式，将避免那些可能致使底层代码变得繁杂混乱的大量的属性文件和帮助类。 简单来说，Spring是一个轻量级的 **控制反转（IoC）** 和 **面向切面（AOP）** 的容器框架。
如果学习JAVA Spring，这两个东西应该是绕不开的，但是暂时理解不深刻，后面准备深入到代码实现层面，与.NET进行一些比较。

#### 控制反转 IoC(Inversion of Control)

在IoC出现以前，组件之间的协调关系是由程序内部代码来控制的，或者说，以前我们使用New关键字来实现两组间之间的依赖关系的。   
这种方式就造成了组件之间的互相耦合。IoC(控制反转)就是来解决这个问题的，它将实现组件间的关系从程序内部提到外部容器来管理。   
也就是说，**由容器在运行期将组件间的某种依赖关系动态的注入组件中。**

**依赖注入(Dependency Injection)**：这就是DI，字面上理解，依赖注入就是将服务注入到使用它的地方。对象只提供普通的方法让容器去决定依赖关系，容器全权负责组件的装配，它会把符合依赖关系的对象通过属性（JavaBean中的setter）或者是构造子传递给需要的对象。

相对于IoC而言，依赖注入(DI)更加准确地描述了IoC的设计理念。所谓依赖注入，即组件之间的依赖关系由容器在应用系统运行期来决定，也就是由容器动态地将某种依赖关系的目标对象实例注入到应用系统中的各个关联的组件之中。

> [Spring核心思想，IoC与DI详解](https://blog.csdn.net/Baple/article/details/53667767)

#### 面向切面编程 AOP(aspect-oriented programming)

POP面向过程程序设计

OOP面向对象的程序设计

微服务

> [关于 Spring AOP (AspectJ) 你该知晓的一切](https://blog.csdn.net/javazejian/article/details/56267036)

### 1.2 Spring MVC

Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring MVC 分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

#### 1.2.1 spring boot

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

1. 目前IDEA只支持Java8的JDK

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8qcfxe0pj20jd0h2dgk.jpg)

2.**Apache Maven**，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（Project Object Model，POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤（生命周期）。

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8qd147zxj20jq0h7gm8.jpg)

3. 选择需要的依赖

![](http://ww1.sinaimg.cn/large/aa003451gy1fx8qd8k36jj20n70h7dgl.jpg)

4. pom.xml

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

##### 构建启动的三种方式

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

不管使用哪种方法都可以启动项目，然后访问http://localhost:8080就可以打开网站看到控制器里面的内容了

##### 应用入口

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

### 1.3 Mybatis

MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。MyBatis是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAO）MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的POJOs（Plain Old Java Objects，普通的 Java对象）映射成数据库中的记录。


#### 1.3.1 和Hibernate的区别

##### 从性能角度考虑
由于 Hibernate 比 MyBatis 抽象封装的程度更高，理论上单个语句执行的性能会低一点。

但 Hibernate 会设置缓存，对于重复查询有一定的优化，而且从编码效率来说，Hibernate 的编码效果肯定是会高一点的。所以，从整体的角度来看性能的话，其实两者不能完全说谁胜谁劣。

##### 从ORM角度考虑
Hibernate 是完备的 ORM 框架，是符合 JPA 规范的，但 MyBatis 不是。MyBatis 比单纯写 JDBC 肯定是方便一点，但无可避免还是要写SQL，且无法做到跨数据库 。Hibernate 使用 JPA 就可以无需考虑数据库的兼容性问题。

使用 Hibernate 的一个难点是，如何来设计对象之间的关系。如果是关系型数据库的话，表和表是通过外键来进行关联的。而在 ORM 中，则需要从面向对象的角度出发，来设计对象之间的关联关系。这个是需要思路上做一个转变的。

##### 结论
从网上的讨论来看，新兴互联网公司更多地使用Mybatis，是因为其易于上手的特性，而旧的大型公司仍然会继续维护他们的Hibernate项目。

#### 1.3.2 连接数据库

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
```
注意MySQL的Version节点中的版本号要与你使用的MySQL驱动的版本一致，8.0左右的高版本在连接串设置上也与低版本不同，会在下文说明

添加依赖之后，IDEA会自动导入相应的包

2. 建立与数据库表对应的类（javabean）

```java
package com.example.dataObject;

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

3. 三层结构及连接池

三层的结构如图，我的理解是Mapper→Service→Controller

![](http://ww1.sinaimg.cn/large/aa003451gy1fxoybvx39xj206v09bjre.jpg)

- Mapper

```java
package com.example.dataMapper;

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
}
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

连接串有两种写法，但是其实都是在application.properties这个文件里，但是可以把这个文件的后缀改为**.yml**使用，这样就是application.yml

- application.properties

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/test?characterEncoding=utf-8&useSSL=false&useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=limingxu
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```
- application.yml

![](https://img2018.cnblogs.com/blog/1368608/201811/1368608-20181125150204883-276581902.png)

写法的作用是一样的，我没用使用yml格式的，所以找了用了别人的图，要注意的是url后面的那一串，这就是前面说的数据库连接问题，正常使用5.x版本的MySQL可以直接使用这种普通的连接串，但是我一开始不知道，用了最新的8.x版本，直接连接数据库会报错，要像application.properties里面的写法在后面通过get传值的方式加上一串说明，具体作用还没有深入研究

## 2 URL路由
**@Controller**标注的类表示是一个处理HTTP请求的控制器(即MVC中的C)，该类中所有被**@RequestMapping**标注的方法都会用来处理对应URL的请求。

### 2.1 @RequestMapping
在Spring MVC框架中，使用@RequestMapping标注可以将URL与处理方法绑定起来，看一下上面的控制器例子

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
用@Controller标注DemoController类，用@RequestMapping分别标注里面的方法，当应用程序运行后，在浏览器中访问http://localhost:8080/，请求会被Spring MVC框架分发到index()方法进行处理。同理，http://localhost:8080/user会交给getUser()方法进行处理。

### 2.2 @PathVariable
如果需要传参数呢？路由中的{}就是参数，以http://localhost:8080/user/1 访问就会将1作为入参即id传入方法getUser()，http://127.0.0.1:8080/username/AAA 同理

### 2.3 不同的请求类型
在Web应用中常用的HTTP方法有四种：

- PUT方法用来添加的资源
- GET方法用来获取已有的资源
- POST方法用来对资源进行状态转换
- DELETE方法用来删除已有的资源

这四个方法可以对应到CRUD操作（Create、Read、Update和Delete），每一个Web请求都是属于其中一种，在Spring MVC中如果不特殊指定的话，默认是GET请求。

实际上 **@RequestMapping("/")** 是 **@RequestMapping("/", method = RequestMethod.GET)** 的简写，即可以通过method属性，设置请求的HTTP方法。

比如PUT /hello请求，对应@RequestMapping("/hello", method = RequestMethod.PUT)

Spring MVC最新的版本中提供了一种更加简洁的配置HTTP方法的方式，增加了四个标注：

- @PutMapping
- @GetMapping
- @PostMapping
- @DeleteMapping

### 2.4 @ResponseBody
加了这个标注，返回值会被直接显示在浏览器上，大致就是.NET里面的Response.Write()，如果在这里返回一个实体，会以json的格式显示，要想显示页面，这里就要返回相应的HTML格式的代码，但是这样写不利于浏览与维护，所以就需要路由到一个HTML的页面


> Tips
> 1.  自动保存：IDEA里面是自动保存的，你每一次输入都会有保存操作，一开始还会习惯性Ctrl+S，但慢慢就习惯了，如果有需要新的引用，也只要输入就可以，IDEA会自动引用，和添加依赖一样
> 2. 区分大小写：IDEA本身对大小写的区分很严格，如果你用大写S开头，自动提示里面就不会出现小写s开头的提示。与C#不同，Java的类型通常是以大写开头
