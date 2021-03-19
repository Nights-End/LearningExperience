最近做了一个前后端分离的商城项目来熟悉开发的整个流程，最后希望能有个正式的部署流程，于是试着把项目放在云服务器上，做了一下发现遇到了不少问题，借此记录一下整个部署的过程。

使用的技术栈如标题所说大体上是Vue+Spring boot，但还是要提一下详细的版本，因为在解决问题的过程中发现由于开发环境的不同会产生诸多影响，查找问题时如果没有版本作为前提经常会出现很多不必要的误解，甚至是误操作，非常浪费时间。详细版本情况如下：

- Vue 2.6.11
- Vue-cli 4.5.0
- Spring boot 2.1.1
- MySQL 8.0.13 （尤其注意）
- Nginx

## 1 上传工具

部署的第一步要把文件上传至云服务器，并且后续还要在云服务器上进行操作，方法很多，我这里选择了Xshell和Xftp这两个工具，均出自NETSARANG这家公司，需要注意的是在国内的官网上隐藏了免费家庭版的申请地址，可以通过直接访问 [Xshell、Xftp免费许可](https://www.netsarang.com/zh/free-for-home-school/) 进行下载，需要填写姓名和邮箱。

### 1.1 Xftp

首先在Xftp中新建连接，填写主机地址和用户名密码，连接成功就可以看到左侧选项卡为本地的文件目录，右侧为云服务器的文件目录，只要从左侧或右侧将文件拖动到另一边就可以实现相应文件的上传和下载，下方传输选项卡会显示具体进度，图形化的界面相当直观，不做过多的说明了。

![](https://i.loli.net/2021/03/05/er1kHOZnSBGtKjq.png)

![](https://i.loli.net/2021/03/05/NYLfRTQ7hO8JmPW.png)

关于Linux的目录规范我没有查到太多需要的信息，所以我这里是在根目录新建了一个名为  **app** 的文件夹来存放网站应用，关于网站的打包我们在下一节说明，这里我们掌握如何上传文件就可以。

### 1.2 Xshell

Xshell的连接建立方法与Xftp差不多，填写主机地址，连接后再输入用户名密码，连接后就可以在云服务器上进行操作了。

![](https://i.loli.net/2021/03/05/P9aIkOpMStYuBf4.png)

![](https://i.loli.net/2021/03/05/oYAz8mKQG2averh.png)

通过这两个工具与云服务器建立联系后我们就可以开始着眼于网站应用自身的问题了。

## 2 前端

相对来说前端的部署没有遇到太多问题，所以我们先从前端开始

### 2.1 Vue

由于我使用了Vue-cli，所以项目完成后通过执行命令 `npm run build` 程序就会自动打包到项目目录下的 **dist** 文件夹，打包后的目录结构如下：

![](https://i.loli.net/2021/02/06/DMv5Ek4eC6FbgYy.png)

之后通过Xftp将这个文件夹上传至云服务器上，之前我已经新建了一个文件夹来存放，因为我们还可能有其他项目，并且后台也需要一个文件夹，所以建立具体文件夹结构如下：

![](https://i.loli.net/2021/03/05/UtFRmoTXHgsY6SP.png)

新建文件夹可以使用Xftp的图形界面快速建立，也可以在Xshell中使用指令，看个人的喜好和习惯，如果需要经常使用Linux系统，建议使用指令多加练习。

在Windows端进行开发的时候，在控制台使用 `npm run serve` 指令就可以快速启动一个本地网站，但是这样的网站只能在内网被访问，肯定是达不到我们最初在外网访问的目的，所以这里要借助反向代理服务器，我选择的是被广泛使用的Nginx。

### 2.2 Nginx

1. 选定安装目录

``` shell
cd /usr/local/src
```

![](https://i.loli.net/2021/03/05/bJDRgdvrkW6i1Ql.png)

2. 在服务器上安装Nginx之前先安装若干依赖：

``` shell
yum install gcc
yum install pcre-devel
yum install zlib zlib-devel
yum install openssl openssl-devel
```

这些依赖的作用可以参考这一篇 [Linux上安装Nginx依赖环境和库、Nginx安装，Nginx服务命令](https://www.cnblogs.com/dreamyy/p/11135872.html)，我没有逐一试过缺少某个依赖会有什么问题，在安装Nginx之前就已经安装好这些依赖了。

3. 下载Nginx安装包并解压

可以从官网选择需要的版本。

``` shell
wget http://nginx.org/download/nginx-1.13.7.tar.gz  #下载
tar -xvf nginx-1.13.7.tar.gz                        #解压
```

4. 执行配置并安装

``` shell
cd nginx-1.13.7       #切换目录
./configure           #执行配置
make && make install  #编译安装（默认安装在/usr/local/nginx）
```

5. 安装完成后查看版本无误启动

``` shell
/usr/local/nginx/sbin/nginx -v  #查看版本

cd /usr/local/nginx/sbin        #cd到nginx的安装位置的sbin目录下
./nginx                         #启动nginx  
```

这里可以先访问一下服务器的外网IP试试，出现 **Welcome to nginx** 就说明已经启动成功了。

Nginx安装完成后修改配置文件以对应我们的网站应用，修改文件的方法也可以选择使用Xftp或者在Linux上用指令操作

```
location / {
    root   /app/osd_mall/vue_app/dist;
    index  index.html index.htm;
}

location /api {
    proxy_pass http://localhost:28019/api/v1;
}
```

这里映射了两个地址，第一个就是网站前端部分的主体，应该没有太多的差异，主要的问题应该集中在第二个地址上，由于我做的是一个前后端分离的项目，后端接口也是包含在项目里的，那么必然存在跨域问题，开发时使用了Vue的Proxy从前端处理，代码如下：

![](https://i.loli.net/2021/03/05/zJEFRsUYnDSNQOf.png)

然后在axios里配置对应的名称即 **api**，这样就可以解决跨域的问题，但这只是本地开发时使用的方法，在Linux服务器上通过Nginx反向代理可以更安全地处理跨域问题，所以上图中的代码就不再需要了，打包前需要注释或者删除。

``` json
axios.defaults.baseURL = process.env.NODE_ENV == 'development' ? '/api' : '/api'
```

![](https://i.loli.net/2021/03/05/exN1gPuczMItf7O.png)

配置修改完成后测试配置文件，没有问题后重启Nginx服务

``` shell
./nginx -t            #测试配置文件
./nginx -s reload     #重启Nginx
```

这时已经可以通过我们的外网IP访问前端页面了，但是由于还没有配置后端接口，所以基本是没有数据和图片的。显而易见，上图中的本地地址 http://localhost:28019/api/v1 就是我们的后端接口地址，那么我们接下来就对后端的部署进行说明。

## 3 后端

相对于前端来说，后端遇到的问题可谓是层出不穷，中间有很多波折，好在最后都解决了，其中有几个就是开头强调的版本问题。

### 3.1 Java

首先要安装Java环境，从官网下载需要的版本，然后和之前一样，通过Xftp上传：

![](https://i.loli.net/2021/03/05/Vn1suCq2d7UjYyg.png)

创建目录，解压：

``` shell
mkdir /usr/java
tar zvxf server-jre-8u271-linux-x64.tar.tar.gz -C /usr/java
```

为了能在全局使用java命令，配置环境变量：
``` shell
# 修改环境配置文件
vi /etc/profile

# 编辑配置文件，在里面添加如下三行
export JAVA_HOME=/usr/java/jdk1.8.0_271
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

# 使环境变量生效
source /etc/profile

# 验证是否配置成功，查看java版本
java -version
```

![](https://i.loli.net/2021/03/05/7yraGNpOlX48ADe.png)

### 3.2 Spring boot

要将Spring boot项目打包部署有很多选择，详情可以看这篇文章 [java项目部署Linux服务器几种启动方式总结经验](https://www.cnblogs.com/both-eyes/p/12221946.html)

我这里选择的是使用jar包部署，通过Maven打包的步骤就不说明了。再次通过Xftp将jar文件上传至服务器，我的截图里有一个dev版本的是保留了控制台日志的版本，正常只需要一个就可以。

![](https://i.loli.net/2021/03/05/rLi4fGbK9ZCSXmg.png)

### 3.3 Screen

上面介绍的文章里有提到java项目会运行在前台，当我们的Xshell窗口关闭后，项目其实也是会关闭的，但如果让连入的机器也保持开机也并不合理，所以使用nohub指令将jar程序一直挂载在后台。这里再介绍另一种方法，那就是Screen工具。

> **Screen**是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。

Screen在一些流行的发行版上已经预安装了，你可以使用下面的命令检查是否已经在你的服务器上安装了。

``` shell
screen -v
```

如果没有可以通过自带的包管理器简单地安装。

``` shell
yum -y install screen
```

``` shell
screen -S yourname      #创建一个名为yourname的session
screen -ls              #列出当前所有的session
screen -r yourname      #回到yourname这个session
screen -d yourname      #远程detach某个session
screen -d -r yourname   #结束当前session并回到yourname这个session
```

使用Screen就如同切换窗口一般，这样在服务器性能允许的情况下就可以同时挂载多个应用，同时也相对方便管理。

### 3.4 MySQL

#### 1. 安装

刚开始部署的时候，我几乎忽略了MySQL，之后也没想到这是最花时间的部分，但如果遵循正确的步骤其实并不困难。

大部分的工具我们都可以通过包管理器yum来安装，上文的Screen正是如此，我本以为MySQL也是如此，不过查了一下发现最初确实如此，但自从MySQL被收购之后就存在版权问题了，所以CentOS7已经不再支持MySQL，而是在内部集成了MariaDB，两者颇有渊源，结果就是文件会有冲突，所以还要先卸载MariaDB才行。

``` shell
# 列出所有被安装的rpm package
rpm -qa | grep mariadb
# 卸载（注意“版本号”根据当前系统显示的版本信息的为准）
rpm -e mariadb-libs-5.5.37-1.el7_0.x86_64
# 如果提示依赖检测失败可以强制卸载
rpm -e --nodeps mariadb-libs-5.5.37-1.el7_0.x86_64

# 安装依赖
yum install vim libaio net-tools
```

之后我们要决定是通过repo源直接从服务器下载安装还是自己去官网下载一个再上传到服务器上，最初我是按一些教程的步骤直接从repo源下载，但由于选择的版本不对，最后程序无法正确建立连接，反复尝试之后选择从[社区版官网](https://dev.mysql.com/downloads/mysql/)下载需要的版本。

![](https://i.loli.net/2021/03/05/McDyfszhUXn1iJS.png)

进入网站默认是最新版本，要找之前的版本点击Archives，在新的页面选择对应的版本。然后是选择操作系统，CentOS系统就是Red Hat的社区发行版，内核是相同的，或者用通用版本（Generic）也可以。

![](https://i.loli.net/2021/03/05/ruslDmGpLa6cYPH.png)

下载bundle包，解压出其中如图所示的五个文件上传至服务器，依次安装mysql-community-common、mysql-community-libs、mysql-community-client、mysql-community-server、mysql-community-release。

``` shell
# 依次类推
rpm -ivh mysql-community-common-8.0.13-1.el7.x86_64.rpm

# 如果出现“依赖检测失败”问题再后面添加 --force --nodeps强制安装
rpm -ivh mysql-community-common-8.0.13-1.el7.x86_64.rpm --force --nodeps
```

现在常见的MySQL版本一般是5.7和8.0之后的若干小版本，根据使用版本的不同，后续的配置有很大的不同，我使用的版本是8.0.13，接下来也是以8.0版本为例。

#### 2. 重置密码

在这一步遇到了诸如权限问题、版本不对、字段不同等情况，因为部署时完全没想到有如此多的问题，没有留下截图，这里说一下整体的思路：首先要登录MySQL，安装完成后可能会有一个随机密码，记录下来，用这个密码登录进数据库，如果有权限问题就先给MySQL文件夹授权，登录之后通过数据表修改默认密码，指令如下：

``` shell
# 登录，-p后面没有空格，直接输入密码，5.7版本密码默认为空，8.0使用随机密码
mysql -u root -p密码
# 切换数据库
use mysql;
# 修改密码（不能用123456这种简单的密码，需要大小写和数字，如果还是想用简单的密码，看下文）
update user set password=password('123456') where user='root';
# 退出
exit;
```

设置完成后就可以用新密码登录了，如果设置有问题或者没有记录随机密码还有通过修改本地文件来重置密码的方式，这里就不介绍了。

如果只是个人开发，不想用太过复杂的密码怎么办？可以通过修改密码规则来解决这一问题，同样的，5.7和8.0版本也存在区别：

``` shell
# 5.7
set global validate_password_policy=0;
set global validate_password_length=1;

# 8.0
set global validate_password.policy=0;
set global validate_password.length=1;

# 查看密码验证变量
SHOW VARIABLES LIKE 'validate_password%';
```

密码等级表

| Policy     | Tests Performed     |
| :------------- | :-------------  |
| 0 or LOW          | Length       |
| 1 or MEDIUM       | Length; numeric, lowercase/uppercase, and special characters       |
| 2 or STRONG       | Length; numeric, lowercase/uppercase, and special characters; dictionary file       |

#### 3. 远程连接

每次要查数据库都要登录Linux就会很不方便，而且界面不友好，如果我们想用本地的Navicat访问远程的数据库要怎么设置呢？

首先要关闭防火墙或者开放数据库的3306端口（最好还是开放端口），

``` shell
# 查看防火墙状态
systemctl status firewalld

# 开启防火墙服务
systemctl start firewalld

# 开放指定端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent

# 重新载入
firewall-cmd --reload
```

如果使用的是云服务器，还要记得在平台上开放指定端口：

![](https://i.loli.net/2021/03/05/4kQzWX5GYeZRtVU.png)

设置完防火墙之后仍然不能远程登录，因为我们没有授权用户，先查询一下用户表：

``` sql
select user,host from user;
```

![](https://i.loli.net/2021/03/05/oXEKUj7lcNgaLxT.png)

可以看到除了remote都是localhost，remote就是我新建的专门用于远程登录的用户，host代表的是该用户可以在哪个地址下登录，“%”代表所有地址都可以。5.7之前的版本可以通过授权直接隐式创建用户，8.0之后不可以了。直接修改root用户的host也是可以的，但是这样登录时会有些小区别，具体忘记了，这里我推荐新建一个用户。

``` sql
/* 创建用户，这里的密码同样也受前面设置的密码等级限制 */
CREATE USER '用户名'@'%' IDENTIFIED BY '密码';

/* 授权 */
grant all privileges on *.* to '用户名'@'%';
```

执行后使用Navicat远程登录如果报错 **1251 client does not support authentication**，说明MySQL8与Navicat的加密方式不同，修改加密方式：

``` sql
alter user 用户名 identified with mysql_native_password by '密码';
```

这样就可以从本地直接访问远程的数据库了。

#### 4.卸载MySQL

如果你在安装时不幸出现问题，有各种各样的问题需要重装MySQL，那么可以根据这篇文章 [Linux下彻底卸载mysql详解](https://www.cnblogs.com/nicknailo/articles/8563456.html)来卸载MySQL。

#### 5.程序建立数据库连接

到这里就是最后一步了，我在这里出现问题后试了各种方法都没有成功建立连接，用户名密码都没有错的情况下可能需要检查以下几个方面：

- 数据库版本与程序的驱动版本是否一致
- 数据库连接串是否正确
- 账号是否有访问权限

引用的插件版本要与服务器上安装的数据库版本一致

![](https://i.loli.net/2021/03/05/nAqVExNPDMQeRuS.png)

5.7和8.0的连接串有不少区别，8.0需要很多额外词条，这里留下我的连接串

```
jdbc:mysql://localhost:3306/onesideddice_mall?useUnicode=true&serverTimezone=Asia/Shanghai&characterEncoding=utf8&autoReconnect=true&allowPublicKeyRetrieval=true&useSSL=false&allowMultiQueries=true
```

#### 6.导入数据

这一步很简单了，有各种方法，这里记录一下导入服务器上的sql文件这种方式：

``` sql
create database abc;      /* 创建数据库 */
use abc;                  /* 使用已创建的数据库 */
set names utf8;           /* 设置编码 */
source /home/abc/abc.sql  /* 导入备份数据库 */
```

### 3.5 Nginx图片服务器

在前端部分我们已经在Nginx中配置过后端接口了，为什么这里还要再提一次呢？其实理论上到这一步之前，就已经完成网站的部署了，如果中间各种对接没有其他问题，网站已经可以正常运行了，但是如果你的站点中有大量的图片，则不得不思考图片要以什么方式存储，相对路径如何设置等。在部署完成后在图片展示上我遇到了很多问题，其中比较显著的就是图片的相对路径转换问题，在观察了一些网站的后，我发现他们的图片通常单独存放在图片服务器上，这样图片的路径相对稳定又方便设置，于是我就想到用Nginx再配置一个图片服务器出来，尽可能地利用现有的服务器资源，当然也可以选择七牛云这种专门的图片服务器，这样图片的访问应该会更加稳定快速。

1. 配置Nginx

在Nginx的配置里添加一个新的Server，方便与我们的网站应用区分，配置如下：

```
server {
    listen       8000;
    server_name  localhost;

    location / {
        root   /app/osd_mall/images;
        autoindex on;
    }
}
```

- root则是将images映射到/home/ftpadmin/hatlth/images/
- autoindex on便是打开浏览功能。

设置修改完成后记得测试并重载Nginx。

2. 开放端口

和之前开放数据库端口相同。

![](https://i.loli.net/2021/03/05/kTsaFJg3hPtN9M1.png)

3. 修改文件访问权限

``` shell
chown ftpadmin /app/osd_mall/images
chmod 777 -R /app/osd_mall/images
```

4. 修改程序

根据自己的站点修改路径，由于使用了统一的图片服务器，现在可以设置一个配置项来专门管理图片地址了，相对于之前要方便很多，本地环境也可以使用这个线上的图片服务器。

## 4 结束

到这里，基本的部署就完成了，实现了从本地Windows端到Linux服务端的转换，在解决问题的过程中没有想到有如此多的问题，记录的时候遗漏了很多细节，但主要的问题应该都得以解决，希望能帮助遇到问题的人，也方便自己日后回顾浏览。
