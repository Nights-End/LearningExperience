# Java开发入门

## 1 SSM框架

### 1.1 Spring Framework

Spring是一个开源的Java／Java EE全功能栈（full-stack）的应用程序框架，以Apache License 2.0开源许可协议的形式发布，也有.NET平台上的移植版本。该框架基于 Expert One-on-One Java EE Design and Development（ISBN 0-7645-4385-7）一书中的代码，最初由Rod Johnson和Juergen Hoeller等开发。

Spring Framework提供了一个简易的开发方式，这种开发方式，将避免那些可能致使底层代码变得繁杂混乱的大量的属性文件和帮助类。 简单来说，Spring是一个轻量级的 **控制反转（IoC）** 和 **面向切面（AOP）** 的容器框架。

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

2. **Apache Maven**，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（Project Object Model，POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤（生命周期）。
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
```Jva
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
- index()方法直接返回一个字符串，那么相当于直接将字符串"Hello World"作为HTTP请求的响应返回给浏览器，于是我们在浏览器中可以看到相应的返回值。

### 1.3 Mybatis

MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。MyBatis是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAO）MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的POJOs（Plain Old Java Objects，普通的 Java对象）映射成数据库中的记录。
