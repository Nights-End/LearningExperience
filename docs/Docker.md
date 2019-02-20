# Docker的使用初探（一）：常用指令说明

前几个星期实践的了，再不记录一下真的就忘干净了

Docker即容器技术，具体的介绍已经有很多，不打算赘述了，说一些优点

## 为什么要用Docker

对我个人来说，最大的优点就是在一台电脑上可以部署不同的环境而不用担心它们产生冲突，最常见的冲突就是端口占用，使用Docker技术可以很方便地规避这一问题，而且便于管理，我不用在本地处理多个环境

更广泛一点好处还有很多

#### 更高效的利用系统资源
下文会看到，即使我在一台普通电脑上同时运行多个环境，也不会占用宿主机过多的资源，正常的开发仍然可以进行，更不用说专门的服务器主机

#### 更快速的启动时间
传统的虚拟机技术启动应用服务往往需要数分钟，而 Docker 容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。

#### 一致的运行环境
开发过程中一个常见的问题是环境一致性问题。由于开发环境、测试环境、生产环境不一致，导致有些 bug 并未在开发过程中被发现。而 Docker 的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性，从而不会再出现 **这段代码在我机器上没问题啊** 这类问题。

#### 持续交付和部署
对开发和运维（DevOps）人员来说，最希望的就是一次创建或配置，可以在任意地方正常运行。   
使用 Docker 可以通过定制应用镜像来实现持续集成、持续交付、部署。开发人员可以通过 Dockerfile 来进行镜像构建，并结合 持续集成(Continuous Integration) 系统进行集成测试，而运维人员则可以直接在生产环境中快速部署该镜像，甚至结合 持续部署(Continuous Delivery/Deployment) 系统进行自动部署。

#### 更轻松的迁移
由于 Docker 确保了执行环境的一致性，使得应用的迁移更加容易。Docker 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。因此用户可以很轻易的将在一个平台上运行的应用，迁移到另一个平台上，而不用担心运行环境的变化导致应用无法正常运行的情况。

#### 更轻松的维护和扩展
Docker 使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单，基于基础镜像进一步扩展镜像也变得非常简单。此外，Docker 团队同各个开源项目团队一起维护了一大批高质量的 官方镜像，既可以直接在生产环境使用，又可以作为基础进一步定制，大大的降低了应用服务的镜像制作成本。

## Docker的安装与简单使用

基础的东西就不说了，不同操作系统的安装具体看这个
[Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/container/run.html)。我在文章里主要记录一些要点，以及在windows系统上的具体操作

### 国内镜像加速

和npm一样，国内网络问题，拉去镜像速度比较慢，所以要配置国内镜像，可以使用Docker官方的或者其他云服务商提供的

- [Docker 官方提供的中国 registry mirror ](https://docs.docker.com/registry/recipes/mirror/#use-case-the-china-registry-mirror)
- [七牛云加速器 ](https://kirk-enterprise.github.io/hub-docs/#/user-guide/mirror)

我使用的是Windows 10系统，配置方式是在系统右下角托盘Docker图标内右键菜单选择 Settings，打开配置窗口后左侧导航菜单选择 Daemon。在 Registry mirrors 一栏中填写加速器地址 [https://registry.docker-cn.com](https://registry.docker-cn.com)，之后点击 Apply 保存后 Docker 就会重启并应用配置的镜像地址了。

### 常用指令

安装完Docker之后再使用cmd命令打开命令控制行中就可以使用Docker命令了，目前我只做了一些简单的实践，记录一下我使用频率比较高的指令

#### 1. docker ps -a
首先要查看当前安装使用了哪些容器

![](http://ww1.sinaimg.cn/large/aa003451gy1fygyg5239dj21gg09ijt5.jpg)

这个命令会列出所有容器的状态，容器的ID、名称、使用的镜像、映射地址等，我个人觉得是使用频率最高的一个

#### 2. docker pull [NAME]

与npm类似，要想创建容器，需要先选择镜像拉取到本地，在[Docker Hub](https://hub.docker.com/explore/)找到需要的镜像，或者自己制作好镜像后，使用docker pull命令拉取到本地，以.NET Core的官方的示例镜像为例

```
docker pull microsoft/dotnet-samples
```

![](http://ww1.sinaimg.cn/large/aa003451gy1fygyg511t7j20la0a2jsk.jpg)

执行命令后等待拉取完成即可，再次拉取同一镜像即更新

#### 3. docker image ls

查看本机的镜像

![](http://ww1.sinaimg.cn/large/aa003451gy1fygyg50vyhj20qg05bdgj.jpg)

拉取后查看本地镜像的状态，会分别列出镜像来自的仓库（Repository）、标签（通常是版本号）、镜像ID、创建时间、镜像大小

#### 4. docker [image/container] rm [ID/NAME]

删除镜像或容器，rm指令的用法对于容器和镜像是相同的，主要不同的是就是镜像没有名称

如果要通过名称删除镜像，可以这样 **docker image rm <仓库名>:<标签>**

如果要通过ID删除镜像，可以输入完整的ID，也可以使用短ID，短ID就是只输入ID的前几个字符，一般三个以上就可以区分不同的镜像

需要注意的是，如果有依托于镜像的子容器就不能删除，并且容器正在运行时也不能删除，但是可以加上**-f**参数，这样Docker会发送SIGKILL信号给容器

另外，所有的container指令可以省掉，**docker rm [ID/NAME]** 就是删除指定容器

#### 5. docker run []

新建并启动一个容器，这个命令比较重要且常用的参数比较多，这里用微软官方镜像给的命令作为示例

```
docker run -it --rm -p 8000:80 --name aspnetcore_sample microsoft/dotnet-samples:aspnetapp
```

+ -t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上
+ -i 则让容器的标准输入保持打开
+ --rm要结合前面的it参数，主要还是测试用的，加入这个参数后，当我们使用Ctrl+C关闭终端之后，这个容器会自动删除
+ -p 端口映射，8000:80表示将容器的80端口映射到宿主机的8000端口上，也就是说访问我本机的8000端口就相当于访问我们部署环境的80端口了
+ --name 容器的名称
+ 最后的那个是镜像的名称，前面提过了，<仓库名>:<标签>

执行这个命令之后，docker首先会看有没有相应的镜像，如果没有就会自动执行拉取操作，我们前面拉取了微软官方的Core应用镜像，这里则启动一个Web示例镜像，这样会有一个拉取的操作，需要等待一会

![](http://ww1.sinaimg.cn/large/aa003451gy1fygyg4zvafj20xj0450t9.jpg)

这样就是创建成功并启动了，访问[http://localhost:8000](http://localhost:8000)就可以看到Core的示例网站

如果去掉 **--rm** 参数，退出后不会删除容器，通过下文的 **start** 命令可以再启动，如果不使用参数则不会再进入终端

如果有问题可以用前面说的第一个命令ps看下当前容器的状态

#### 6. docker start [ID/NAME]

当我们重启docker后，一般情况下容器会处于Exited退出状态，这个时候要通过start命令启动，ID前面说过，使用长ID短ID均可，名称就是创建容器是**--name**参数设定的

这里用上文已经关闭容器的例子

![](http://ww1.sinaimg.cn/large/aa003451gy1fygyg52ue1j21dc0aq0um.jpg)

返回容器ID一般就是启动成功，这样就可以继续通过80端口访问

#### 7. docker exec

进入容器，还有另一个指令 **docker attach** 两者有些不一样，这里会在下文说明原因

**docker exec** 后边可以跟多个参数，这里主要说明 **-i -t** 参数。

前面 **run** 命令也说过，这里有些类似，只用 **-i** 参数时，由于没有分配伪终端，界面没有我们熟悉的Linux命令提示符，但命令执行结果仍然可以返回。当 **-i -t** 参数一起使用时，则可以看到我们熟悉的 Linux 命令提示符。

但是不知道是不是命令使用错误，只能在后面跟一个 **ls**这样的简单命令，如果用 **cd [目录名称]** 这种有空格的命令就无法识别，所以使用 **bash** 命令进入linux终端就可以正常使用linux命令了

![](http://ww1.sinaimg.cn/large/aa003451gy1fygyg50z9qj21ez0alta6.jpg)

---

下一节会讲一讲如何在Docker上部署.NET Core，不再是官方的示例，而是如果从开发环境到部署的环境，本地如何通过Dockerfile构建镜像上传到仓库

# Docker的使用初探（二）：Docker与.NET Core的结合

在二者的结合上，微软官方给予了很大的支持，从官方发布的一些文章和VS 2017在建立.NET Core项目时自带的Docker选项都可以看出来，这也与Core的跨平台特性有很大的关系，而Docker正是可以选择以Linux或Windows环境部署

## 添加Dockefile

上一篇文章介绍了如何拉取Core的官方镜像，但是我们终究要将Docker应用到我们的开发环境中，如何将我们自己的项目生成为镜像并部署到Docker上呢？第一步就添加Dockerfile这个文件，在VS2017中大致是三种方法，它们稍有区别，但最终也都是建立了一个Dockerfile文件

<<<<<<< HEAD
1. ### 在创建项目时添加
=======
### 1. 在创建项目时添加
>>>>>>> 283c09d8b0d58bf1bd7b83d1e367d49cae46d732

在新建Core项目时,勾选“启用Docker支持”选项，新建的项目会自动添加dockerfile文件，文件的具体内容在下文进行分析

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dl0lz51j20m80fk0to.jpg)

<<<<<<< HEAD
2. ### 手动添加
=======
### 2. 手动添加
>>>>>>> 283c09d8b0d58bf1bd7b83d1e367d49cae46d732

已经在使用的项目可以通过“右键-添加-Docker支持”，这样也可以新建Dockerfile文件

![](http://ww1.sinaimg.cn/large/aa003451gy1fz5dl0m9hcj20j10i9t9w.jpg)

<<<<<<< HEAD
3. ### 容器业务流程协调控制程序支持
=======
### 3. 容器业务流程协调控制程序支持
>>>>>>> 283c09d8b0d58bf1bd7b83d1e367d49cae46d732

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
<<<<<<< HEAD
基本上有了这些，就可以利用Docker给开发工作带来一些便捷，如果后面还要继续深入的话，就是将Docker与持续集成结合起来应用到网站服务器环境上
=======
有了这些，就可以利用Docker给开发工作带来一些便捷，如果后面还要继续深入的话，就是将Docker与持续集成结合起来应用到网站服务器环境上
>>>>>>> 283c09d8b0d58bf1bd7b83d1e367d49cae46d732
