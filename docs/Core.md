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

```Linux
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
```

### 2.2 安装.NET SDK

更新可供安装的产品，然后安装.NET SDK

输入以下命令：

```Linux
sudo yum update
sudo yum install dotnet-sdk-2.2
```

中间有两次手动确认，然后等待安装完成即可

### 2.3 创建你的应用

通过输入命令就可以创建一个官方的示例.NET Core程序

```Linux
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

```linux
dotnet run
```

![Hello World](http://ww1.sinaimg.cn/large/aa003451gy1fxx53ni7msj209m00wgle.jpg)

### 2.5 创建web应用

使用mkdir命令新建一个文件夹mvc，然后进入目录

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0tftn2qzj20ae01kdfx.jpg)

创建网站

```
dotnet new mvc
```

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0tg2nkaqj20p608swhl.jpg)

然后发布这个网站程序

```
dotnet restore
dotnet publish -c release
```

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0tgblv0mj20gd05qac8.jpg)

默认的发布目录是在/bin/release/netcoreapp2.x/publish/里，可以新建一个目录拷贝进去

```
scp -r /root/mvc/bin/release/netcoreapp2.2/publish/* /root/www/firstapp
```

### 2.6 从外网访问web应用

完成发布后，已经可以通过执行dotnet命令来启动网站了，但是只能在内网访问，显然这不是我们想要的，要想从外网访问，我们需要反向代理服务器，这里选择Nginx

使用yum命令远程安装

```
sudo yum install epel-release
yum install nginx
```

启动

```
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

```
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

```
sudo nginx -t         #测试配置
sudo nginx -s reload  #重新加载配置
```

配置完成之后，启动网站访问服务器IP地址的8080端口即可

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0xfii7exj20iy04mgnh.jpg)

但是启动网站这里存在一个问题，如果像上面那样没有使用cd命令进入网站目录启动，样式和脚本等文件的路径就会出现错误，导致页面显示不正常所以要在网站目录启动

![](http://ww1.sinaimg.cn/large/aa003451gy1fy0xfszsy8j20gb03pgms.jpg)

基本的网站部署就到这里，下一次讲讲用Docker如何进行.NET Core的部署与开发
