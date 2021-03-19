不知道这个小知识点用得多不多，曾经在书上看到过，所以有一些印象，前段时间顺手写出类似如下的代码

```javascript
var result;
if (parseInt('abc')==NaN) {
  return "相等";
} else {
  return "不等";
}
```

断点调试之后发现无论如何都不相等，方法parseInt()返回的结果确实是NaN，但是与右侧的NaN比较返回的结果却是false，这时候才突然想起来NaN有不等于自身的特性，所以简单收集一下资料做个整理

## 原因

了解原因之前我们先明确一个问题，什么时候回出现NaN，理论上来说有两种情况

- 表达式计算
- 类型转换

我们逐个分析

### 表达式计算

当运算中使用了 + - * / 等运算符的时候，js会自动进行转换，讲参与计算的变量转换成js的基本类型之一的Number类型，如果转换失败就会返回NaN。比如说：

```javascript
console.log(12 + 'a'); //NaN
```

### 类型转换

比较典型的就是最初的例子里使用的parseInt()，除此之外还有parseFloat()和Number()，传入一个非数变量结果就是NaN，很好理解，顺便一提parse系列方法与Number()有些不同。

```javascript
parseInt('123abc'); // 123
Number('123abc'); // NaN
```

## 总结

从上面两种产生NaN的情况来看，NaN是一种异常的结果，也就是“not a number”，虽然它也是一个变量，但它是描述性变量，'a'不是一个数字（not a number），'b'也不是一个数字（not a number），但是'a'和'b'并不相等，所以NaN != NaN也就成立了。
