
Markdown编辑器的使用与开发入门
----------

## 1 是什么（What）

### 1.1 Markdown是一种轻量级标记语言

> Markdown是一种轻量级标记语言，创始人为约翰·格鲁伯（英语：John
> Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML（或者HTML）文档”。
>—— [维基百科](https://zh.wikipedia.org/wiki/Markdown)

### 1.2 以纯文本形式(易读、易写、易更改)编写文档

Markdown是一种轻量级标记语言，它以<span style="color:red">纯文本形式</span>(易读、易写、易更改)编写文档，并最终以HTML格式发布。
Markdown也可以理解为将以MARKDOWN语法编写的语言转换成HTML内容的工具。

### 1.3 “易于阅读、易于撰写的纯文字格式，并选择性的转换成有效的XHTML”

John Gruber 在 2004 年创造了 Markdown 语言，在语法上有很大一部分是跟亚伦·斯沃茨（Aaron Swartz）共同合作的。这个语言的目的是希望大家使用

>易于阅读、易于撰写的纯文字格式，并选择性的转换成有效的XHTML（或是HTML）。

其中最重要的设计是可读性，也就是说这个语言应该要能直接在字面上的被阅读，而不用被一些格式化指令标记（像是RTF与HTML）。 因此，它是现行电子邮件标记格式的惯例。

## 2 为什么（Why）

### 2.1 它是易读（看起来舒服）、易写（语法简单）、易更改纯文本

在使用编辑软件，如Word时，常常边写作边排版，经常纠结于怎么没这个字体，怎么行高不对了，等等。

但是对于写作来说，你是在进行一种”创作“，你需要一种思维的连贯，而不是让你写一篇格式规范的论文。

相较于 Word 类型编辑器的“所见即所得”而言，Markdown 文件本身是纯文本，并没有格式，但通过 Markdown 语法符号能提供更加准确的 HTML 类型格式控制同时又没有 HTML 那么难以书写。

我们在word中点击鼠标来达到加粗、倾斜、增大字体的目的，在Markdown中被相应的特殊符号替代。Markdown用于解放鼠标，仅仅使用键盘就能排版出非常漂亮的文字、博客等

 - 无需像HTML费时排版，易读
 - 让人专注于输入而非格式的东西，易写
 - 语法简单，提高写作效率，易更改

### 2.2 兼容HTML，可以转换为HTML格式发布

Markdown是标记语言，HTML也是标记语句，Markdown是不是要取代HTML？答案是NO。Markdown的设计理念是，能让文档更容易读、写和改。HTML是发布格式，Markdown是书写格式。Markdown最终会转为HTML格式在网页上显示。

其实浏览器并不能识别 Markdown 的语法，但许多 blog、wiki 平台以及 github 都内置了 Markdown 编辑器，编辑器会先把 Markdown 文本转换成 HTML 格式的网页，然后再把 HTML 网页交给浏览器来显示。除了上述的内置编辑器外，还有许多能解析 Markdown 语法的编写工具，这些工具一般都提供浏览器预览和导出成 HTML 或 PDF 文件的功能。

Markdown 的语法由一些符号定义，这些符号夹杂在文本中，用于控制文本的显示方式。比如两个星号可以给文字加粗**加粗**，这两个星号在 Markdown 编辑器处理后就变成了 HTML 中的<strong>加粗</strong>标签。

### 2.3 跨平台使用

在线MarkDown编辑：

- StackEdit
- Mahua
- Dillinger

Win平台：

- MarkDownPad
- Sublime Text 2
- MDEditor

Linux平台：

- ReText
- Sublime Text 2
当然在Win和Linux上，Vim 和 Emacs 都是神器级的编辑软件，当然支持

Mac平台：

- Mou
- Sublime Text 2
Mac 中很多此类软件。

MarkDown For Chrome插件

### 2.4 越来越多的网站支持Markdown

目前很多网站博客都支持Markdown格式的书写，在Github上会看到项目下有一个README.MD的项目描述文件。

- GitHub
- Stack Overflow
- Reddit
- 简书
- 知乎 
- 有道云笔记
- 博客园
- CSDN
- VS 2017
- 等等

## 3 怎么做（How）

Markdown 的语法需要编辑器才能实现，因此编辑器可以按照自己的需求添加或者修改某个语法的含义。因此，基本上所有编辑器解析 Markdown 语法的方式都有些许差异，但大体上可以分成三类：

 1. Classic Markdown：最基础的 Markdown 语法，所有的编辑器都支持
 2. Extra Markdown：扩展的 Markdown 语法，增加了表格等元素，不一定都能支持
 3. Github Markdown：github 文档使用的 Markdown 语法，包含 Extra Markdown 的所有内容，还增加了代码高亮、emoji表情等语法，目前许多 blog 平台（cnblogs，csdn）都支持这种语法

### 3.1 Markdown的基本标签

Markdown语法主要分为如下几大部分：
**标题**，**段落**，**区块引用**，**代码区块**，**强调**，**列表**，**分割线**，**链接**，**图片**，**反斜杠 `\`**。

#### 3.1.1 标题

两种形式：  
1. 使用`=`和`-`标记一级和二级标题。
 一级标题   
 `=========`   
 二级标题    
 `---------`

效果：

 一级标题   
 =========   
 二级标题
 ---------  

2. 使用`#`，可表示1-6级标题。
 \# 一级标题   
 \## 二级标题   
 \### 三级标题   
 \#### 四级标题   
 \##### 五级标题   
 \###### 六级标题    

效果：
 # 一级标题   
 ## 二级标题   
 ### 三级标题   
 #### 四级标题   
 ##### 五级标题   
 ###### 六级标题

#### 3.1.2 段落

段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用**两个以上**空格加上回车（引用中换行省略回车）。

#### 3.1.3 区块引用

在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用，如：
 \> 区块引用  
 \>> 嵌套引用  

效果：
> 区块引用  
>> 嵌套引用

#### 3.1.4 代码区块

代码可以用行内代码或者代码块来表示。

1. **行内代码**使用一个或多个重音符号来表示代码区域，起始和结束的重音符号个数必须相同。

Use the \`printf()\` function.

效果：
Use the  `printf()`  function.

普通段落：

$(document).ready(() => {
  $('pre code').each((i, block) => {
    hljs.highlightBlock(block);
  });
}); 

2. **Classic 代码块**的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。在转换成 HTML 时，每行行首的 4 个空格或 1 个制表符缩进会被移除，其余的空格或制表符会被保留。另外，一般在代码块中的 Markdown 语法符号不会被转换。如    

代码区块：

    $(document).ready(() => {
		$('pre code').each((i, block) => {
		    hljs.highlightBlock(block);
		    **highlight**
		});
	});
	
3. **Github 代码块**使用三个或以上重音符号（\``` \```）放在代码块的前一行和后一行。在前一行重音符的后面加上语言名称（注意要小写），可以按照该语言的语法对代码块内容高亮。如果要在代码块中显示三个重音符，用四个重音符来表示代码块起止即可。支持的编程语言参见[the languages YAML file](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)，如果要使用没有语法高亮的代码块，用`plain`标记。


\```javascript
$(document).ready(() => {
	$('pre code').each((i, block) => {
	    hljs.highlightBlock(block);
	    **highlight**
	});
});
\```


效果：

```javascript
$(document).ready(() => {
	$('pre code').each((i, block) => {
	    hljs.highlightBlock(block);
	    **highlight**
	});
});
```

```
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

**注意**:需要和普通段落之间存在空行。

#### 3.1.5 强调

在强调内容两侧分别加上`*`或者`_`，如：
> \*斜体\*，\_斜体\_    
> \*\*粗体\*\*，\_\_粗体\_\_

效果：
 *斜体*，_斜体_    
 **粗体**，__粗体__

#### 3.1.6 列表

使用`·`、`+`、或`-`标记无序列表，如：

 \- （\+\*） 第一项
 
 \- （\+\*） 第二项
 
 \- （\+\*） 第三项

**注意**：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。

效果：
+ 1
	+ 1.1
	+ 1.2
+ 2
+ 2

* 1
* 2

有序列表的标记方式是将上述的符号换成数字,并辅以`.`，如：
 1\. 第一项   
 2\. 第二项    
 3\. 第三项    

效果：
 1. 第一项
	 1. 一
	 2. 二
 2. 第二项
 3. 第三项

#### 3.1.7 分割线

分割线最常使用就是三个或以上`*`，还可以使用`-`和`_`。

***

#### 3.1.8 链接

链接可以由两种形式生成：**行内式**和**参考式**。    
1. **行内式**：
 \[博客园\]\(http:://www.cnblogs.com/ "链接title"\)。

效果：
[博客园](http://www.cnblogs.com/ "链接title")。

2. **参考式**：
 \[博客园1\]\[1\]    
 \[博客园2\]\[2\]    
 \[1\]:http:://www.cnblogs.com
 \[2\]:http:://www.cnblogs.com

效果：
 [博客园1][1]    
 [博客园2][2]

[1]: http:://www.cnblogs.com/ "链接title"
[2]: http:://www.cnblogs.com/ "链接title"

**注意**：上述的`[1]:https:://github.com/younghz/Markdown "Markdown"`不出现在区块中。

#### 3.1.9 图片

添加图片的形式和链接相似，只需在链接的基础上前方加一个`！`。

!\[描述](https://img-blog.csdn.net/2018081921564647?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTU5NDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

效果：
![描述](https://img-blog.csdn.net/2018081921564647?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTU5NDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 3.1.10 反斜杠`\`

相当于**反转义**作用。使符号成为普通符号。

### 3.2 扩展语法

### 3.3 常用工具


## 4 试一下（Try）

### 4.1 初衷

最初，我们先用试着用word来写，再转换成HTML文件保存，但是这样存在几个问题：1、转换出来的文件标签和样式十分的杂乱和冗余，有太多无用的标签，后期要修改样式也十分不容易；2、图片无法保存，在word文档中插入的图片路径是固定的物理路径，或是与文档一起存储的，当文档转换成HTML文件并放在网站服务器上使用时，图片的路径就发生了变化，如果再去调整图片就十分麻烦；3、关于图片还有种解决的办法，word可以保存为htm格式，这种格式是将图片和文字一同保存在一个文件的方法，但是最后的结果是只有IE可以识别这种格式，在Chrome里都是乱码，所以最后同样作罢。

### 4.1 解析

### 4.1 预览

### 4.1 图片上传


