之前在项目中写了定时器来做循环播放，但是总是会有越走越快的问题，开始是以为前后的HTML代码拼接的有问题，时间紧急的情况下反复改了很多也没什么效果，后来发现是js定时器的问题，在这里记录一下。

### （setinterval）多次初始化

使用js定时器（setinterval）首要的问题就是要记得清除，即调用（clearInterval）方法，由于没有使用定时器的经验，我一开始是没有清除定时器，程序每一次初始化的时候都调用一次定时器，之前的定时器实例没有被销毁，新的定时器又开始执行，就会出现1s变0.5s，0.5s变0.25秒的情况，从观感上来看就是定时器“越走越快”了。


这个过程可以用几行简单的代码模拟一下

``` HTML
<label id="lblShowNum"></label>
<input type="button" id="btnStart" value="启动" />
<input type="button" id="btnClear" value="清除" />
```

```JavaScript
window.onload = function () {
    var i = 0;
    var timer;
    document.getElementById("btnStart").onclick = function () {        
        timer = setInterval(
            function () {
                i++;
                document.getElementById("lblShowNum").innerText = i;
            }, 1000);
    }
    document.getElementById("btnClear").onclick = function () {
        clearInterval(timer);
    }
}
```

如果只点击一次“启动”按钮，定时器会正常运行，点击“清除”按钮就可以暂停定时器，但是每一次点击“启动”按钮，都会提高数字的增速，而清除功能也不再起作用，这就是因为在每一次点击“启动”的时候都有新的定时器被创建。

### 清除（clearInterval）的失效

但为什么清除的方法会失效呢？在代码中我们定义了一个变量timer去接收定时器，对timer操作是不是就能清除定时器了呢？并不是是这样，首先看下setinterval()返回值的说明

>一个可以传递给 Window.clearInterval() 从而取消对 code 的周期性执行的值。

这里可以看出这个返回值并不是定时器本身，它只是一个用于传递的返回值，如果想当然的把它当做定时器，以为每次初始化赋值就是新的定时器就错了，我最开始就是这样想的。

每一次给timer赋值都是在创建新的定时器对象，而且之前的定时器也并没有被清除，所以这时候调用clearInterval(timer)清除的只是最后一个被创建的定时器对象罢了。

使用上面的例子就可以简单的用肉眼观察效果，先点击一次启动观察速度，再点击第二次，会看到速度有明显的提升，这时候使用清除功能，速度就会回到第一次启动的状态，但是多次点击清除是没有用的，如果想看准确的结果可以将时间打印出来进行比较。

### 解决方法

看到这里，答案呼之欲出了，很简单，在每次初始化定时器之前先执行清除操作，保证之前的定时器被清除了就不会发生越走越快的情况，所以其实并不是定时器越走越快，而是有多个定时器在执行，定时器里面的程序执行的频率提高了。

```JavaScript
window.onload = function () {
    var i = 0;
    var timer;
    document.getElementById("btnStart").onclick = function () {       
        clearInterval(timer);
        timer = setInterval(
            function () {
                i++;
                document.getElementById("lblShowNum").innerText = i;
            }, 1000);
    }
    document.getElementById("btnClear").onclick = function () {
        clearInterval(timer);
    }
}
```
