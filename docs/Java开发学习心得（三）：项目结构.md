## 3 项目结构

经过前面一系列学习，差不多对Java的开发过程有了一定的了解，为了能保持一个良好的项目结构，考虑到接下来要进行开发，还需要学习一下Java的项目结构

下面以两个项目结构为参照

图1

![](http://ww1.sinaimg.cn/large/aa003451gy1fy375m0y8aj207p0jzweu.jpg)

图2

![](https://image-static.segmentfault.com/250/234/2502346953-595f054721e34)

第一个是我自己学习时的Demo，一边学一边建文件，应该有些错误的地方，第二张是从网上看到的一个结构比较清晰的图片

图1的结构完整一点，就以图1为主一点点讲起

### 3.1 src

因为我用的是Maven，大方向上项目两大文件夹分别是src和target，以及一个pom.xml配置文件，src目录里是项目工程的源码文件，配置文件和资源文件等，其下一级是main和test这两个文件夹

#### 3.1.1 main

main文件夹下是主要的工程源文件，然后下面又是两个文件夹：java和resources,其实上面也讲了，而且顾名思义，java里面是源码文件，resources里面是资源文件

##### 3.1.1.1 java

这之下的文件结构可以参考上面的图2了，大体上就是model-mapper-service-controller，其他还有公共类和表现层等，这里在下面单独分一个章节来说

##### 3.1.1.2 resources

![](http://ww1.sinaimg.cn/large/aa003451gy1fy37tuu317j205x02lt8i.jpg)

资源文件夹默认就是这个样子，application.properties是用来填写各种配置的，比如数据库连接的配置信息、日志组件的配置信息等，有些人会改成yml后缀名，里面的格式就不尽相同了，在前面也讲过，这里就不再赘述。

除此之外，各种组件的配置文件也在这个文件夹下

还有一个主要的作用是存放静态文件资源，包括需要访问的jsp、html、css、js、图片等，还有代码模板

1. 项目配置文件：resources/application.yml
2. 静态资源目录：resources/static/
    ——用于存放html、css、js、图片等资源
3. 视图模板目录：resources/templates/
    ——用于存放jsp、thymeleaf等模板文件
4. mybatis映射文件：resources/mapper/（mybatis项目）
5. mybatis配置文件：resources/mapper/config/（mybatis项目）

#### 3.1.2 test

如题所述，单元测试用的

### 3.2 target

target是有存放项目构建后的文件和目录，jar包、war包、编译的class文件等

target里的所有内容都是maven构建的时候生成的

jar 包就是 java archive file java 的一种文档格式。jar文件非常类似zip

WAR是Sun提出的一种Web应用程序格式，与JAR类似，也是许多文件的一个压缩包。这个包中的文件按一定目录结构来组织：
通常其根目录下包含有Html和Jsp文件或者包含这两种文件的目录，另外还会有一个WEB-INF目录，这个目录很重要。通常在WEB-INF目录下有一个web.xml文件和一个classes目录，web.xml是这个应用的配置文件，而classes目录下则包含编译好的Servlet类和Jsp或Servlet所依赖的其它类（如JavaBean）。

通常这些所依赖的类也可以打包成JAR放到WEB-INF下的lib目录下，当然也可以放到系统的CLASSPATH中，但那样移植和管理起来不方便

target里的所有内容都是maven构建的时候生成的

## 4 Java的工程文件结构

传统的三层不用再解释了，在Java里对应数据访问层的是DAO，进行数据交互，对应业务逻辑层的是Service层，这里写逻辑代码，然后就是UI层

### 4.1 几种对象的解释

上面提到DAO，这里简单说说一些相关的简写，在一些源码里看到了，初学时有点懵

1. PO(persistant object): 持久对象，可以看成是与数据库中的表相映射的java对象。最简单的PO就是对应数据库中某个表中的一条记录，多个记录可以用PO的集合。PO中应该不包含任何对数据库的操作。实体
2. VO(value object):值对象，通常用于业务层之间的数据传递，和PO一样也是仅仅包含数据而已。但应是抽象出的业务对象,可以和表对应,也可以不,这根据业务的需要。可能就是Get/Set？
3. DAO(Data Access Object)：数据访问对象，用于访问数据库，里面包含对数据库的各种操作，配合VO进行CRUD
4. DTO(Data Transfer Object)：数据传输对象，是一组需要跨进程或网络边界传输的聚合数据的简单容器。它不应该包含业务逻辑，并将其行为限制为诸如内部一致性检查和基本验证之类的活动。我一开始以为是业务实体，但是看说明不是一个东西，那个是BO。

DTO的用法：
比如我们一张表有100个字段，那么对应的PO就有100个属性。但是我们界面上只要显示10个字段，客户端用WEB service来获取数据，没有必要把整个PO对象传递到客户端，这时我们就可以用只有这10个属性的DTO来传递结果到客户端，这样也不会暴露服务端表结构.到达客户端以后，如果用这个对象来对应界面显示，那此时它的身份就转为VO。DTO还有减少请求的次数、简化传输对象、避免代码重复等作用。
5. ORM(O/R Mapping,Object Relational Mapping)：对象关系映射

### 4.2 实际的项目文件应该如何设计

可以参考图2

1. bean也就是实体了，对应数据库的字段
2. DAO，前面也说过了，不过里面写法可以细分很多种，使用Mybatis就是Mapper接口文件或者是xml的写法，如果还使用了JPA，还会有个repository类文件
3. Service，这块好像写法的争议比较多，可以看看这篇文章[有多少人在滥用 service+serviceImpl，又有多少人在误用myBatis](https://my.oschina.net/HuQingmiao/blog/636161)
我在很多地方都看到了一个ServiceImpl接口，一个service类的写法，一直不理解为什么要多次一举，但还是照着做了，上面那篇文章里说到这是一种过时的写法，使用接口文件是为了解决可能连接各种数据库而有多个数据访问层的问题，这样只需要实现对应的结构就行了，而有了Mybatis之后这种问题就不存在了，所以不需要再写impl接口文件，使用 **service+dao+mapper.xml** 是一种更好的选择
4. Controller，这里只讨论MVC了，普通java web的写法还不会，然后从controller应该就直接到资源文件里的HTML页面了
5. 上面是按顺序自下而上的，除此之外一些公共层：工具类库（utils）、配置类（config）、数据传输对象（dto）、视图包装对象（vo）

还可以看看这篇[spring boot 项目开发常用目录结构](https://blog.csdn.net/Auntvt/article/details/80381756)
