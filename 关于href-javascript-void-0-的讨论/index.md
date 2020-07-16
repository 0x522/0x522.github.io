# 关于href="javascript:void(0);"的讨论


## javascript:void(0);含义

我们经常会使用到 javascript:void(0) 这样的代码，那么在 JavaScript 中 javascript:void(0) 代表的是什么意思呢？

javascript:void(0) 中最关键的是 void 关键字， void 是 JavaScript 中非常重要的关键字，该操作符指定要计算一个表达式但是不返回值。

`<a></a>`超链接的功能：

- 1.可以被点击，有一个下划线样式
- 2.点击后跳转到href指定的url

如果要保留1功能，去掉2功能，需要将href="javascript:void(0);"    

javascript是伪协议，表示这个href的值要使用js代码，而void 是 JavaScript 中非常重要的关键字，该操作符指定要计算一个表达式但是不返回值。

javascript:void(0);仅仅表示一个死链接。



## href="#"与href="javascript:void(0);"的区别

##### *URL*  超链接的 URL。

可能的值：

- 绝对 URL - 指向另一个站点（比如 href="www.google.com"）
- 相对 URL - 指向站点内的某个文件（href="index.htm"）
- 锚 URL - 指向页面中的锚（href="#top"）

```javascript
<a href="" > 中 href 为空，会怎样？
答：点击会刷新页面，相当于访问当前URL。
```

**#** 包含了一个位置信息，默认的锚是**#top** 也就是网页的上端。

而javascript:void(0); 仅仅表示一个死链接。

在页面很长的时候会使用 **#** 来定位页面的具体位置，格式为：**# + id**。

如果你要定义一个死链接请使用 javascript:void(0); 。



## js关于锚(a)的几种调用方法

```javascript
1、a href="javascript:js_method();"

   这是常用的方法，但是这种方法在传递this等参数的时候很容易出问题，而且javascript:协议作为a的href属性的时候不仅会导致不必要的触发window.onbeforeunload事件，在IE里面更会使gif动画图片停止播放。W3C标准不推荐在href里面执行javascript语句

2、a href="javascript:void(0);" οnclick="js_method()"

   这种方法是很多网站最常用的方法，也是最周全的方法，onclick方法负责执行js函数，而void是一个操作符，void(0)返回undefined，地址不发生跳转。而且这种方法不会像第一种方法一样直接将js方法暴露在浏览器的状态栏。

3、a href="javascript:;" οnclick="js_method()"

   这种方法跟跟2种类似，区别只是执行了一条空的js代码。

4、a href="#" οnclick="js_method()"

   这种方法也是网上很常见的代码，#是标签内置的一个方法，代表top的作用。所以用这种方法点击后网页后返回到页面的最顶端。

5、a href="#" οnclick="js_method();return false;"

   这种方法点击执行了js函数后return false，页面不发生跳转，执行后还是在页面的当前位置。
```
**综合上述，在a中调用js函数最适当的方法推荐使用：**

```javascript
<a href="javascript:void(0);" οnclick="js_method()"></a>
<a href="javascript:;" οnclick="js_method()"></a>
<a href="#" οnclick="js_method();return false;"></a>
```


