在安装和使用这三种工具时，我们有很多方式可以选择，这些方法各有优劣，每个人都有自己用起来比较习惯的配置，所以我在这里记录下自己比较习惯的一种安装方式与其他一些可能的选项。

## NVM、NPM、Node.js的关系

假定我们的最终目的是为了安装并使用Node.js，那么我们有两种常规的选择：

- Node.js安装包
- NVM

第一种方式非常直接，搜索Node.js，在官网下载需要的的版本并进行安装就可以了，但是一般不推荐这种方式，因为Node.js的版本众多，开发时不同的项目可能会使用不同的版本，每次面对不同的项目都要重新安装，版本的切换十分麻烦。

![](https://i.loli.net/2020/11/20/QjU1kmzoZvYKrlD.png)

为了解决上面的问题，使用NVM是一个不错的选择，我们先看的NVM的全称：**Node Version Manager**，也就是说NVM是Node.js的版本管理器，通过NVM我们就可以安装多个不同版本的Node.js并在需要的时候进行切换，具体的方法在下面介绍。

NVM是Node.js管理器，那么NPM呢？还是看全称：**Node Package Manager**，也就是Node.js包管理器，用于管理Node的大量扩展API。在安装Node.js时就会自动安装相应版本的NPM。

## NVM

直接从GitHub上下载 [下载地址](https://github.com/coreybutler/nvm-windows/releases)

分成安装版（setup）和免安装版（noinstall）,区别不大但如果希望能在全局的各个文件位置都可以使用NVM、NPM和Node.js这些工具的指令，要注意使用免安装版需要自行设置环境变量

![](https://i.loli.net/2020/11/20/UemOab4Nn8Lc1st.png)

### 安装版（setup）

安装过程需要设置两个路径，就是环境变量相应的值，分别是NVM和Node.js的路径，建议路径中不要有空格，安装时会自动建立环境变量。

![](https://i.loli.net/2020/11/20/qsIGVYmcWrfTNoj.png)

![](https://i.loli.net/2020/11/20/Nj7nswxf3OmZ26D.png)

### 免安装版（noinstall）设置环境变量

使用免安装版时可以配置环境变量如下：

![](https://i.loli.net/2020/11/20/ErWL5luA1v7cJSZ.png)

**NVM_HOME** 就是NVM所在的目录，**NVM_SYMLINK** 则是Node.js的目录，但变量名看起来好像没什么关系，原因我们在下面切换Node.js版本的操作方法处说明。

### 验证nvm安装

安装完成后我们可以通过`nvm version`命令查看nvm的版本来验证是否已经成功安装。由于我们配置了环境变量，所以可以在任意目录中执行nvm命令。

![](https://i.loli.net/2020/11/20/WYj7ptmakdsbo61.png)

### 安装指定版本的Node.js

```
nvm install 版本号

// 举例
nvm install 12.19.1
```

命令很简单，但是在版本号的选择上似乎有些问题，最开始我们就说到Node.js版本众多，这里我到底该选择哪个版本呢？

我们回过头看下Node.js的首页,可以看到两种版本

- LTS（Long-Term Support）
- Current

![](https://i.loli.net/2020/11/20/OJvq1a9QeSWGFfn.png)

关于Node.js版本的成因和各种具体的说明已经有文章写得很明确了，[官网页面](https://nodejs.org/en/about/releases/)也有说明，有兴趣可以去了解一下，根据 **Recommended For Most Users** 和 **Latest Features** 可以得出一个简单的结论：**通常情况，为了稳定选择LTS版本，为了尝鲜选择Current版本**。

另外值得一提的就是Node.js采用奇偶版本号的形式，奇数为非稳定版（如9.11、15.2.1），偶数为稳定版（如10.23、12.19.1），通过这个也可以简单判断。

但既然我们使用了nvm，难道还要每次去Node,js官网看下版本再安装吗？当然不需要，通过指令`nvm list available`就可以查看近期的可用版本。

![](https://i.loli.net/2020/11/20/JRdUWlBGhxwV5Ef.png)

### 下载服务器

在开始安装之前还有一件事要注意，npm与Node.js的默认下载服务器均在国外，国内进行下载时往往有速度较慢的问题，我们可以通过配置为淘宝镜像进行解决。在安装目录下面我们可以找到名为 **settings.txt** 的文件，打开并在最后加上两行来将Node.js和npm的下载服务器地址替换为服务器在国内的淘宝镜像:

```
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

### 切换Node.js版本

得到版本号后执行上面的install指令进行安装。

![](https://i.loli.net/2020/11/22/OxqzMPCI9Q5UE7X.png)

此时可以先通过`nvm list`指令来查看已经安装的Node.js版本。

![](https://i.loli.net/2020/11/22/ny5vKhrpLfgkeWU.png)

可以看到已经有一个版本的Node.js被安装了，也许这时已经有人开始跃跃欲试地输入`node -v`来测试Node.js的安装情况了，但此时会发现`node`指令仍然没有被系统识别，显然我们还有什么步骤没有做，在安装步骤的最后，nvm也提醒我们使用`nvm use`指令。

输入`nvm use 版本号`，例如`nvm use 12.19.1`来切换Node.js到指定的版本。

![](https://i.loli.net/2020/11/22/zJaN2OXTPV6eCRr.png)

可以看到提示我们已经切换到对应的版本，之后可以通过`nvm list`来查看当前已经安装和正在使用的Node.js版本。正在使用的Node.js版本会用星号标出。

![](https://i.loli.net/2020/11/22/MNtH1aZUJmREqBV.png)

此时我们就可以用我们熟悉的命令来查看一下Node.js和npm的安装情况了。

![](https://i.loli.net/2020/11/22/4GQVO6gDzxFBq8s.png)

### 关于nvm版本切换的实质

在最开始使用nvm的时候我遇到了一些特殊的情况导致版本切换功能整个失效，借这个问题我观察了一下nvm的安装目录从而发现了端倪。大家应该还记得，在最初安装nvm的时候我们选择了两个路径，一个是nvm的路径，另一个是Node.js的路径，但是直到我们安装第一个版本的Node.js后，这个文件夹也是未使用的状态，此时再查看nvm的安装目录，可以看到有对应版本号的文件夹被创建，里面就是对应版本Node.js的文件，每一个版本对应一个文件夹。

![](https://i.loli.net/2020/11/22/XFKHbaJSUfxV3Z2.png)

那版本切换时怎么做到的呢？如果使用普通权限的命令提示行来进行版本切换操作，我们会发现系统提示cmd申请管理员权限，同意操作后再查看之前的nodejs文件夹：

![](https://i.loli.net/2020/11/22/SyPaGszrCmD2RH4.png)

图标有所变化，看到左下角的标记应该可以猜到这里可能是用了Windows系统中的快捷方式来实现这一操作，右键-属性查看一下果不其然：

![](https://i.loli.net/2020/11/22/dDxnalb5mHIThQS.png)

这个文件夹此时就变成了nvm目录下的对应版本文件夹的快捷方式，切换版本正是在进行创建或修改快捷方式这一操作。

而且我们可以回忆一下在最初安装时设置的系统变量，nodejs目录采用的SYMLINK可能就是System Link这一缩写，所以每次切换版本，Node.js的全局变量也已经设置好了，我们自然可以直接在各个文件位置使用node和npm指令。

![](https://i.loli.net/2020/11/20/ErWL5luA1v7cJSZ.png)

## NPM

关于NPM的配置我们还可以进行一些小的调整。输入`npm config ls`指令：

![](https://i.loli.net/2020/11/22/mWl8U4qAHTXeJ6L.png)

红线圈出的两个部分默认未修改的情况应该是在C盘的用户目录下，这两个路径是npm全局包的安装和缓存目录。在我的C盘目录比较拮据的情况下，将全局包安装在C盘显然不太合适，所以我选择将这两个路径改到之前nvm目录的附件，便于查看和管理，指令如下：

```
npm config set prefix "D:/web/package/npm_global"
npm config set cache "D:/web/package/npm_cache"
```

接下来可以安装一个全局包试一下:

```
npm install vue -g
```

安装完成，全局查看可以看到当前的全局目录和vue已经安装成功：

```
npm ls -g
```

![](https://i.loli.net/2020/11/22/fKzVXmSBZsE8i7A.png)

### 疑问

由于npm现在已经固定包的安装目录了，即使切换Node.js版本，npm的包安装目录也仍然是我们设置的文件夹，使用`npm ls -g`指令查看仍然会发现之前安装的包，不知道是否出现依赖于不同npm版本的包互相冲突的情况，目前我还没有太多关于Node.js版本切换的实践场景，准备日后遇到这个问题再进行一些实际的测试。
