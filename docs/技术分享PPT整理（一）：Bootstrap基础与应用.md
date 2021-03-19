最近在复习的时候总感觉有些知识点总结过，但是翻了一下博客没有找到，才想起来有一些内容是放在部门的技术分享里的，趁这个时候跳了几篇相对有价值的梳理一下，因为都是PPT，所以内容相对零散，以要点和图片为主。

第一篇是我在工作上刚刚能稳步前行时完成的，Boostrap在我们的工作中占有一定的比重，为此我专门进行了深入学习，标题比较宽泛，但内容绝不是复述官网的流水账，Bootstrap虽然简单，而且也有新的布局模式诸如Flex、Grid此类，但其核心的开发思想————移动设备优先仍然对我们现在的开发有所指导，这篇分享也是着重于这一点。

## 简介

### 起源

Bootstrap原名Twitter Blueprint，是由Twitter推出的一个用于快速开发Web应用程序和网站的前端框架。开发的初衷顾名思义，作为开发的蓝图和范本与我们现在的需求没有太大的区别，就是因为需要一个通用的框架来省去一些重复的开发内容。其主要特点有：

- 移动设备优先
- 友好的学习曲线
- 响应式设计
- 卓越的兼容性
- 12列响应式栅格结构

### 四个主要版本

#### 1. 1.0.0

Twitter推出的原始版本。

#### 2. 2.3.2

开始支持响应式网页设计(RWD)。页面布局可以根据显示网页的设备（桌面、平板电脑、手机）来进行动态调整。

#### 3. 3.3.7

即第三版的最终版本，是现在使用较为广泛的版本，顺应时代的潮流，将移动设备优先作为设计方针，更加强调了响应式设计。放弃了对IE7的支持。

#### 4. 4.x.x

至今为止仍在更新，我个人还不太熟悉。放弃了对IE9的支持。

### 重置样式

大家开发中应该有这样的经验：即使是相同的样式和标签，在不同的浏览器上也会呈现出不同的样子，对于Bootstrap这种通用框架来说，跨浏览器的一致性是非常重要的，所以开发者通过引入重置样式 **Normalize.css** 来让各个浏览器的CSS样式有一个统一的基准，这个基准主要的内容是“清零”：

- 移除body的margin声明
- 设置body的背景色为白色
- 为排版设置了基本的字体、字号和行高
- 设置全局链接颜色，且当链接处于悬浮:hover状态时才会显示下划线样式

## 如何开始

Bootstrap的使用十分简单，这里以3.3.7为例，[官网](https://www.bootcss.com/)上已经有非常清晰的引入说明了，这里不再赘述，主要说明是说明一些要点。

### HTML5文档类型

Bootstrap 使用到的某些 HTML 元素和 CSS 属性需要将文档类型 **DOCTYPE** 设置为HTML5。在你项目中的每个页面都要参照下面的格式进行设置：

``` HTML
<!DOCTYPE html>
<html>
  ....
</html>
```

不同的文档类型可能会导致页面渲染出不同的结果，使用同样的代码，但显示出来的结果却又些许不同，可能问题就出在这里，Bootstrap官方指定使用的HTML5版本。

### 移动设备优先

移动设备优先是 Bootstrap 3 的最显著的变化。为了让 Bootstrap 开发的网站对移动设备友好，确保适当的绘制和触屏缩放，需要在网页的 **head** 之中添加 **viewport meta** 标签。

>在之前的 Bootstrap 版本中（直到 2.x），您需要手动引用另一个 CSS，才能让整个项目友好的支持移动设备。
现在不一样了，**Bootstrap 3 默认的 CSS 本身就对移动设备友好支持。**
Bootstrap 3 的设计目标是移动设备优先，然后才是桌面设备。这实际上是一个非常及时的转变，因为现在越来越多的用户使用移动设备。
为了让 Bootstrap 开发的网站对移动设备友好，确保适当的绘制和触屏缩放，需要在网页的 head 之中添加 viewport meta 标签，如下所示：

```HTML
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

**width** 属性控制设备的宽度。假设您的网站将被带有不同屏幕分辨率的设备浏览，那么将它设置为 device-width 可以确保它能正确呈现在不同设备上。**initial-scale=1.0** 确保网页加载时，以 1:1 的比例呈现，不会有任何的缩放。

在移动设备浏览器上，通过为 viewport meta 标签添加 user-scalable=no 可以禁用其缩放（zooming）功能。通常情况下，maximum-scale=1.0 与 user-scalable=no 一起使用。这样禁用缩放功能后，用户只能滚动屏幕，就能让您的网站看上去更像原生应用的感觉。这种方式并不推荐所有网站使用，视情况而定！

```HTML
<meta name="viewport" content="width=device-width,
				initial-scale=1.0,
				maximum-scale=1.0,
				user-scalable=no">
```

### 基本的全局显示

Bootstrap 3 使用 body {margin: 0;} 来移除 body 的边距。除此之外还统一设置了一些样式来保证一致性：

```CSS
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 14px;
  line-height: 1.428571429;
  color: #333333;
  background-color: #ffffff;
}
```

- 第一条规则设置 body 的默认字体样式为 "Helvetica Neue", Helvetica, Arial, sans-serif（这里使用的都是无衬线字体，一个小小的知识点，感兴趣的可以自己查下，印刷在报纸和书刊上的内容通常推荐使用衬线字体如宋体，因为其突出的衬线部分更方便大家识别文字,而网页设计大多推荐无衬线字体如大家熟悉的微软雅黑，在清晰的电子屏幕上这种字体更加让人舒适）。
- 第二条规则设置文本的默认字体大小为 14 像素。
- 第三条规则设置默认的行高度为 1.428571429。这个值的来源是20px/14px，详情见下图。
- 第四条规则设置默认的文本颜色为 #333333。
- 最后一条规则设置默认的背景颜色为白色。

![](https://i.loli.net/2021/03/17/4VPDC7EWFcudTBr.png)


### 容器（Container）

Bootstrap 需要为页面内容和栅格系统包裹一个 .container 容器。我们提供了两个作此用处的类。**.container**（固定宽度）或**.container-fluid**（100%宽度）注意，由于 padding 等属性的原因，这两种容器类不能互相嵌套。

```HTML
<div class="container">
  ...
</div>
```

通过如下的代码，把 container 的左右外边距（margin-right、margin-left）交由浏览器决定。由于内边距（padding）是固定宽度，默认情况下容器是不可嵌套的。

```CSS
.container {
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
```

container类也使用伪元素来维持结构。设置 display 为 table，会创建一个匿名的 table-cell 和一个新的块格式化上下文。:before 伪元素防止上边距崩塌，:after 伪元素清除浮动。如果 conteneditable 属性出现在 HTML 中，由于一些 Opera bug，围绕上述元素创建一个空格。这可以通过使用 content: " " 来修复。

```CSS
.container:before,
.container:after
{
  display: table;
  content: " ";
}
```

如下代码创建了一个伪元素，并确保了所有的容器包含所有的浮动元素。

```CSS
.container:after
{
  clear: both;
}
```

### 响应式网页设计（Responsive web design，通常缩写为RWD）

也称自适应网页设计。 是一种网页设计的技术做法，该设计可使网站在**不同的设备**（从桌面电脑显示器到移动电话或其他移动产品设备）上浏览时对应不同分辨率皆有适合的呈现，减少用户进行缩放、平移和滚动等操作行为。

采用 RWD 设计的网站使用CSS3 **Media queries**，即一种对@media规则的扩展，以及**流式的基于比例的网格**和自适应大小的图像以适应不同大小的设备：

### 媒体查询

使用@media，你可以针对不同的媒体类型定义不同的样式，特别是如果你需要设置设计响应式的页面，媒体查询是非常有用的，当你重置浏览器的大小时，页面也会根据浏览器的宽度和高度重新渲染页面。

```CSS
/**
媒体查询的基本格式
@media mediatype and|not|only (media feature) { CSS-Code; }
媒体查询在Bootstrap中的应用，使用了缩写，意为当屏幕可视窗口的宽度大于768像素时，container类的宽度为750px*/
@media (min-width: 768px) { .container { width: 750px; } }
```

媒体查询是Boostrap的核心栅格系统的基石，在下文中我会详述栅格系统，但在这之前我们要注意媒体查询不支持IE8以下的版本，这也是最开始我们提到3.3.7版本放弃对IE7支持的原因。但是借助JS插件Respond.js可以让IE6-8支持媒体查询的宽度条件，从而是栅格系统可以在低版本IE上正常应用，所以该插件也经常和Bootstrap一起使用，相似的还有html5shiv.js，这个插件可以让低版本的IE浏览器识别H5标签。

## 栅格系统

### 实现原理

栅格系统是Bootstrap中的核心，正是因为栅格系统的存在，Bootstrap才能有着如此强大的响应式布局方案。下面是官方文档中的解说：

>Bootstrap提供了一套**响应式、移动设备优先的流式栅格系统**，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。包含了用于简单的布局选项的预定义类，也包含了用于生成更多语义布局的功能强大的混合类。

```
/* 超小屏幕（手机，小于 768px） */
/* 没有任何媒体查询相关的代码，因为这在 Bootstrap 中是默认的（还记得 Bootstrap 是移动设备优先的吗？） */
/* 小屏幕（平板，大于等于 768px） */
@media (min-width: @screen-sm-min) { ... }
/* 中等屏幕（桌面显示器，大于等于 992px） */
@media (min-width: @screen-md-min) { ... }
/* 大屏幕（大桌面显示器，大于等于 1200px） */
@media (min-width: @screen-lg-min) { ... }
```

Bootstrap的基础CSS代码**默认从小屏幕设备**（比如移动设备、平板电脑）开始，然后**使用媒体查询扩展到大屏幕设备**（比如笔记本电脑、台式电脑）上的组件和网格。这段说明正是Bootstrap移动设备优先特点的印证，从移动设备开始设计，逐步向上判断并添加内容，这也应该是跨端网页设计应该参考的方式。

### 基本结构

1. 行（row）

数据行（.row）必须包含在容器.container（固定宽度）或.container-fluid（100%宽度）中，以便为其赋予合适的排列（aligment）和内填充（padding）。如：

```HTML
<div class="container"><!-- 水平居中，两边有margin，最小屏幕时，充满父元素 -->
    <div class="row"></div>
</div> <!-- 或者 -->
<div class="container-fluid"><!-- 默认一直充满整个父元素 -->
    <div class="row"></div>
</div>
```

```CSS
.navbar > .container .navbar-brand,
.navbar > .container-fluid .navbar-brand
{ margin-left: -15px; }

.row { margin-right: -15px; margin-left: -15px; }
```

2. 列（column）

在数据行（.row)中可以添加列（column），但列数之和不能超过平分的总列数（在超过时，多余部分会换行显示），默认12（使用Less或者Sass可以进行自定义设置）。

```CSS
.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
.col-xs-3, .col-sm-3, .col-md-3, .col-lg-3, .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4,
.col-xs-5, .col-sm-5, .col-md-5, .col-lg-5, .col-xs-6, .col-sm-6, .col-md-6, .col-lg-6,
.col-xs-7, .col-sm-7, .col-md-7, .col-lg-7, .col-xs-8, .col-sm-8, .col-md-8, .col-lg-8,
.col-xs-9, .col-sm-9, .col-md-9, .col-lg-9, .col-xs-10, .col-sm-10, .col-md-10, .col-lg-10,
.col-xs-11, .col-sm-11, .col-md-11, .col-lg-11, .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12
{ position: relative; min-height: 1px; padding-right: 15px; padding-left: 15px; }
```

所有列的基本样式相同，宽度width不同，分别为不同的比例的百分数，如：

```CSS
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
.col-xs-9 {
  width: 75%;
}
```

xs、sm、md、lg分别对应超小、小、中等、大屏，详细说明见下图：

![](https://i.loli.net/2021/03/17/W6xBU7sdu5wQZTV.png)

Boostrap官网上有很清晰的演示，使用开发者工具将屏幕调整到手机版大小可以直接看到栅格的变化，随着屏幕的变大和缩小，浏览器会自行判断应用xs还是sm样式：

![](https://i.loli.net/2021/03/17/eSOLfZHG2IzYtuw.png)

直接看Bootstrap的源码就可以发现媒体查询在里面所起到的作用，前面我们说过Bootstrap是移动设备优先的，所以xs的代码直接写在外面，sm的代码则放在媒体查询：屏幕宽度大于等于768px的代码块中，依次类推，md和lg也是一样的。

```CSS
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
/**省略其他*/

@media (min-width: 768px) {
  .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
    float: left;
  }
  .col-sm-12 {
    width: 100%;
  }
  .col-sm-11 {
    width: 91.66666667%;
  }
  /** 省略其他 */
}
```

3. 页面上的具体内容应当放置于列（column）内，并且只有列（column）可以作为数据行.row容器的直接子元素。

## 结语

这篇博客的内容很简单，基本都是入门级内容，后面也用来给部门新人培训用，当时的背景是大家都会用Bootstrap，但是用法各式各样，很多官方提到的点都没有注意，所以也借技术分享的机会说明一下，后面还有一些应用型的内容就没有写进来了，都是简单地应用API来实现功能，这里着重的还是这只**移动设备优先**的思想，开发初期就有这种从少到多，逐级添加的概念后，后面我在第一次遇到移动开发时也没有过多地返工，希望也能对大家有用。
