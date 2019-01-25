# .NET Core跨平台部署

## 1. Windows-IIS

大家对于在IIS上部署.NET站点已经驾轻就熟了，部署.NET Core也没有什么本质区别，但是这其中仍然有一些细节是不同的，下面记录了一些我在部署时遇到的问题

### 1.1 安装.NET Core Windows Server Hosting

要在IIS上运行ASP.NET Core，必须安装[.NET Core Windows Server Hosting](https://go.microsoft.com/fwlink/?linkid=848766)

安装完成后最好重启IIS

如果没有安装该组件就直接打开部署的网站会出现 **500.19 相关的配置数据无效**

### 1.2 配置应用程序池

Core的IIS站点应用程序池的.NET CLR版本要选择 **无托管代码**

![选择无托管代码](http://ww1.sinaimg.cn/large/aa003451ly1fxx0ebct6jj208h08qwei.jpg)

### 1.3 使用发布文件

我最开始测试的时候，仍然使用Web根目录作为网站的物理路径，但是网站无法访问，报HTTP403错误——Web 服务器被配置为不列出此目录的内容，也是就是这个文件夹下没有可以访问的文件，在查阅网上的资料后发现其他人都是使用了发布文件夹作为物理路径，生成发布版本设置相应路径后.NET Core的示例站点即可正常访问

![站点设置](http://ww1.sinaimg.cn/large/aa003451gy1fxx18gcxzuj20ei0c03yq.jpg)

发布文件夹结构

![发布文件夹结构](http://ww1.sinaimg.cn/large/aa003451gy1fxx197219yj205w0643yg.jpg)

成功访问

![成功访问](http://ww1.sinaimg.cn/large/aa003451gy1fxx1kbuhe9j20y60kqgoo.jpg)

## 2 Linux

微软官方给出了不同系统的部署方法[Tutorial Guide](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial)，由于Linux有不同的版本，所以这里选择CentOS作为示例，有以下几个步骤

### 2.1 添加.NET产品依赖

在安装.NET之前，你需要注册微软的Key，注册产品仓库，并且安装需要的依赖，在每台机器上只需要做一次。

直接执行以下命令：

```shell
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
```

### 2.2 安装.NET SDK

更新可供安装的产品，然后安装.NET SDK

输入以下命令：

```shell
sudo yum update
sudo yum install dotnet-sdk-2.2
```

中间有两次手动确认，然后等待安装完成即可

### 2.3 创建你的应用

通过输入命令就可以创建一个官方的示例.NET Core程序

```shell
dotnet new console -o myApp
cd myApp
```

第一条命令新建应用，第二条进入应用文件夹

通过 **ls** 命令我们可以看到该文件夹下只有两个文件，obj是文件夹

![Linux下的文件结构](http://ww1.sinaimg.cn/large/aa003451gy1fxx2mlzf8vj208800wt8i.jpg)

默认的主文件Program.cs的内容如下：

```csharp
using System;

namespace myApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

### 2.4 运行应用

```shell
dotnet run
```

![Hello World](http://ww1.sinaimg.cn/large/aa003451gy1fxx53ni7msj209m00wgle.jpg)

### 2.5 创建web应用

使用mkdir命令新建一个文件夹mvc，然后进入目录

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0tftn2qzj20ae01kdfx.jpg)

创建网站

```shell
dotnet new mvc
```

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0tg2nkaqj20p608swhl.jpg)

然后发布这个网站程序

```shell
dotnet restore
dotnet publish -c release
```

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0tgblv0mj20gd05qac8.jpg)

默认的发布目录是在/bin/release/netcoreapp2.x/publish/里，可以新建一个目录拷贝进去

```shell
scp -r /root/mvc/bin/release/netcoreapp2.2/publish/* /root/www/firstapp
```

### 2.6 从外网访问web应用

完成发布后，已经可以通过执行dotnet命令来启动网站了，但是只能在内网访问，显然这不是我们想要的，要想从外网访问，我们需要反向代理服务器，这里选择Nginx

使用yum命令远程安装

```shell
sudo yum install epel-release
yum install nginx
```

启动

```shell
systemctl start nginx #启用Nginx
systemctl enable nginx #设置开机启动
```

这时候已经可以直接通过服务器的IP地址的80端口访问Nginx的测试页了，需要注意的是如果使用阿里云服务器，需要在安全组配置中开放80端口才能够访问

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0xewme1yj21hb0ca415.jpg)

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0xf76ahqj21aw0f5jsr.jpg)

接下来根据需要进行一些端口的配置，dotnet默认的访问端口为5000，但是我测试的时候好像是在linux上被占用了，所以对 **Program.cs** 进行修改，使其可以通过其他端口访问，这里使用8080

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseUrls("http://*:8080")
        .UseStartup<Startup>();
}
```

改完之后需要重新生成发布，开始我使用的是微软官方的示例程序，这里为了对比端口，我在自己Windows系统下新建了一个2.1的示例程序，使用VS2017进行程序修改，发布后通过xftp再上传到Linux服务器上

然后去修改Nginx的配置，默认的路径应该是/etc/nginx/nginx.conf，在server节点下的location节点加一句 proxy_pass http://localhost:8080; 就可以

```shell
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _;
    root         /usr/share/nginx/html;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
		    proxy_pass http://localhost:8080;
    }

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}
```

修改完成后测试并重启Nginx服务

```shell
sudo nginx -t         #测试配置
sudo nginx -s reload  #重新加载配置
```

配置完成之后，启动网站访问服务器IP地址的8080端口即可

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0xfii7exj20iy04mgnh.jpg)

但是启动网站这里存在一个问题，如果像上面那样没有使用cd命令进入网站目录启动，样式和脚本等文件的路径就会出现错误，导致页面显示不正常所以要在网站目录启动

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0xfszsy8j20gb03pgms.jpg)

基本的网站部署就到这里，下一次讲讲用Docker如何进行.NET Core的部署与开发

# Docker与.NET Core的结合

[TOC]

在二者的结合上，微软官方给予了很大的支持，从官方发布的一些文章和VS 2017在建立.NET Core项目时自带的Docker选项都可以看出来，这也与Core的跨平台特性有很大的关系，而Docker正是可以选择以Linux或Windows环境部署

## 添加Dockefile

上一篇文章介绍了如何拉取Core的官方镜像，但是我们终究要将Docker应用到我们的开发环境中，如何将我们自己的项目生成为镜像并部署到Docker上呢？第一步就添加Dockerfile这个文件，在VS2017中大致是三种方法，它们稍有区别，但最终也都是建立了一个Dockerfile文件

### 1. 在创建项目时添加

在新建Core项目时,勾选“启用Docker支持”选项，新建的项目会自动添加dockerfile文件，文件的具体内容在下文进行分析

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dl0lz51j20m80fk0to.jpg)

### 2. 手动添加

已经在使用的项目可以通过“右键-添加-Docker支持”，这样也可以新建Dockerfile文件

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dl0m9hcj20j10i9t9w.jpg)

### 3. 容器业务流程协调控制程序支持

这种方法相对于前两种比较特殊，它不再是单单增加一个Dockerfile文件，而是如名称一般是一整条生产链，用于配合持续集成工具的开发-调试-生成-发布一条龙服务。添加的方式与第二种相同，右键项目添加就能看到这个拗口的名字

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dl0mi81j20ig08t0td.jpg)

使用这种方式除了生成Dockerfile文件还会在解决方案中添加一个名为Docker Compose的业务流程协调程序，在新建时可以选择，但是默认自带的只有这个。里面包含两个文件，一个是 `.dockerignore` 这个和git类似，里面记录的文件不会被打包成镜像发布，另一个是 `docker-compose.yml` ，用于配置这个业务流程的信息，如镜像名称和Dockerfile文件的路径等

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dl0p92qj207f02bmx0.jpg)

## Dockefile语法

大概是有以下这些，挑几个用到的说一下

```
FROM
MAINTAINER
RUN
CMD
EXPOSE
ENV
ADD
COPY
ENTRYPOINT
VOLUME
USER
WORKDIR
ONBUILD
```

- ### FROM

`FROM <image>`

说明使用的镜像，如果本地没有会自动拉取对应名称的镜像，没有指定标签的情况默认就是latest

`FORM`指令是Dockerfile文件的第一行，但可以不唯一，根据需要可以有多个

以默认的Core项目为例，这里拉取的就是Core的官方镜像，上篇文章也有用到，分别是运行时和SDK

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnpmnzj20f405va9y.jpg)

- ### WORKDIR

`WORKDIR <工作目录路径>`

显而易见就是镜像被安装的路径，如果路径不存在，Docker会自动创建

- ### COPY

`COPY <源路径> <目标路径>`

将文件和目录复制到容器的文件系统。文件和目录需位于相对于 Dockerfile 的路径中。

- ### RUN

`RUN <Shell/exec>`

在当前镜像上要执行的命令，可以使用shell或者exec的格式

- ### EXPOSE

`EXPOSE <端口>`

服务端容器对外映射的本地端口

- ### ENTRYPOINT

使用格式 `RUN` 一样，但是这个命令是在容器启动后执行的命令，不会被 `RUN` 命令覆盖，一个Dockerfile里面只能有一个，如果有多个则只执行最后一条

其余的指令还没怎么用到，用法还不太清楚

## Docker项目调试

我们在前面提到了三种创建Dockerfile文件的方法，实际上是两种情况，针对这两种情况，打包镜像的方法也不同

### 仅添加文件的方式

使用 `docker build -t <name> <path>` 指令。这种情况更加泛用，无论是不是用VS创建的都可以使用这种指令打包镜像

进入Dockerfile文件所在的路径后执行命令即可

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnqkvvj20lb05nt8o.jpg)

为了演示，我先删除的core的官方sdk，由于在Dockerfile里面我们写入了使用了 `FROM` 命令，所以执行命令后我们发现Docker自动下载了镜像，并且打包了我们的项目，但是最后有一句 *image operating system "windows" cannot be used on this platform* ，因为我这边Docker使用的是Linux模式，这里我们构建的是Windows的容器镜像，所以需要切换一下，系统右下角托盘图标右键“switch to ...”，

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnrumaj207t09h3zm.jpg)

为了体现Dockerfile指令的效果，我们删除之前创建的镜像再执行一次Build指令，这次Docker没有下载Core的sdk，因为之前已经下载过了。不过我在这里遇到了网络问题，发现之前的镜像拉取也失败了，所以换了一个镜像加速地址，然后简化了一下dockerfile文件，然后重复上面的操作就行了

```
FROM microsoft/dotnet:2.1-aspnetcore-runtime
WORKDIR /app
COPY . .
EXPOSE 80
ENTRYPOINT ["dotnet", "CoreDockerDemo1.dll"]
```

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnrf6dj20l30dmglv.jpg)

可以看到dockerfile里面的指令被依次执行，完成之后我们使用 `docker image ls` 就可以看到我们构建的镜像了，之后用上面的方法可以创建Docker即可

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnqxu4j20on04jdfs.jpg)

### 容器业务流程协调控制程序支持

使用这种方式就不需要自己手动构建了，只要在VS里的调试按钮点一下即可。由于我们前面添加过这套协调控制程序，所以现在这个项目里可以直接选择docker进行调试

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfntm62j20h102x0sq.jpg)

在这之前要对 `docker-compose.yml` 文件进行配置，基本上与dockerfile类似，而且更加直观，对应输入名称等就好了

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfntqfzj208y03o0sj.jpg)

我在第一次生成时出现了“未启用卷共享”的错误，这里我们需要在Docker的设置中的Shared Drives标签中把程序生成构建的磁盘选中，然后点击“Apply”按钮应用设置，然后docker会自动重启

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfntv8kj20g202jmx5.jpg)

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnuuwbj20ni0g4aax.jpg)

设置完成后再点击VS中的运行，web应用就会自动编译生成并创建镜像和容器，然后启动网站。第一次启动时可能会询问是否授权SSL证书，进行授权即可

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dfnqtpqj215k04cjrc.jpg)

---
有了这些，就可以利用Docker给开发工作带来一些便捷，如果后面还要继续深入的话，就是将Docker与持续集成结合起来应用到网站服务器环境上
