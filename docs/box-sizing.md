# 关于padding被计算在width中问题——box-sizing相关

前一阵子遇到一个小问题，在同样的样式（主要是宽高边距之类的）条件下，DIV在移动端和PC端的宽度不一样，排除了绝大多数样式的问题，但是有个比较陌生，就是box-sinzing，其实经常看到，只不过没怎么注意过，连具体的值都不知道有哪些，在开发者工具里面试了一下，果然和这个样式有关，因此查了一些资料并记录一下。

## 盒子模型

首先，盒子模型大家都知道，W3C标准的Box Model由四部分组成——content、padding、border、margin

>Every box is composed of four parts (or areas), defined by their respective edges: the content edge, padding edge, border edge, and margin edge.

如果我们给一个应用了标准盒模型的div设置一个宽度，那么，实际上我们设置的是上文提到的content的宽度，也就是下图这个样子

**图一**

- Element空间高度 = content height + padding + border + margin

还有个不那么“标准”的盒模型，就是IE6以下的浏览器所使用的，在这种情况下：

- Element空间宽度 = content Width + margin (Width包含了元素内容宽度、边框宽度、内距宽度)

这个时候如果我们给div设置一个宽度，那就是为元素的内容+边框+内边距设置了一个总值，如图所示

**图二**

``` CSS
div {
    width: 200px;
    height: 100px;
    margin: 40px;
    padding: 20px;
    border: 10px solid blue;
}
```

## 与box-sizing有什么关系

通过box-sizing这个样式我们可以改变这种宽度计算方式，它的属性值有两个：content-box和border-box。默认值为content-box，也就是标准的盒子模型，此时的计算公式为

+ width = 内容的宽度
+ height = 内容的高度

另一个属性为border-box，它的width和height属性包括内容，内边距和边框，但不包括外边距。

这是当文档处于 Quirks模式 时Internet Explorer使用的盒模型。注意，填充和边框将在盒子内 , 例如, .box {width: 350px; border: 10px solid black;} 导致在浏览器中呈现的宽度为350px的盒子。内容框不能为负，并且被分配到0，使得不可能使用border-box使元素消失。
