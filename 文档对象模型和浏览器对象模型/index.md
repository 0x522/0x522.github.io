# 浏览器对象模型和文档对象模型


# ECMAScript，DOM和BOM 

**我们都知道， javascript 有三部分构成，ECMAScript，DOM和BOM，根据宿主（浏览器）的不同，具体的表现形式也不尽相同，ie和其他的浏览器风格迥异。** 



## BOM

**BOM是Browser Object Model，浏览器对象模型。**

**BOM将浏览器的各个组成部分封装成对象。**

BOM包括：

- Navigator 浏览器对象
- window 窗口对象
- Location 地址栏对象
- History 历史记录对象
- Screen 显示器屏幕对象



### Window对象

#### 特点:

- Window对象不需要创建可以直接使用，使用方法：window.方法名();
- window引用可以省略。

#### 常用方法:

  **1.与弹出有关的方法**

  - alert() 显示带有一段消息和一个确认按钮的警告框。
  - confirm() 显示带有一段消息以及确认按钮和取消按钮的对话框，返回值是boolean。
    - 如果用户点击确定按钮，则方法返回true
    - 如果用户点击取消按钮，则方法返回false
  - prompt() 显示可提示用户输入的对话框，返回值是获取用户输入的值。

  **2.与打开关闭有关的方法**

  - open() 打开一个新窗口，参数可以传url字符串，返回一个新的window对象。
  - close() 关闭一个窗口，由哪个对象调用的close方法就关闭哪个对象。

  **3.与定时器有关的方法**

  - setTimeout(参数1，参数2) **一次性定时器**，参数1：可以传一个js代码片段或者函数对象，参数2：延迟的毫秒数。返回值返回一个唯一编号。
  - clearTimeout() **取消定时器**，参数是setTimeout()的返回值，可以根据每个一次性定时器的唯一返回值来取消某一个一次性定时器。
  - setInterval(参数1，参数2) **循环定时器**，在setTimeout()的基础上增加了循环功能，每（参数2）毫秒执行（参数1）。返回值返回一个唯一编号。
  - clearInterval() **取消定时器**，参数是setInterval()的返回值，可以根据每个循环定时器的唯一返回值来取消某一个一次性定时器。

#### 属性：

**1.获取其他BOM对象**

 - history
 - location
 - screen
 - Navigator

**2.获取DOM对象**

- document



### Location对象

**地址栏对象**

#### 创建:

1. window.location

2. location(省略window.写法)

#### 常用方法

- reload() 重新加载当前的文档、刷新			

#### 常用属性

-  href 属性是一个可读可写的字符串，可设置或返回当前显示的文档的完整 URL 
   - 可以使用`location.href = "www.baidu.com";`来给href属性设置值。



### History对象

**历史记录对象**

#### 创建：

1. window.history
2. history

#### 常用方法：

- back()  加载history列表中的前一个URL。
- forward()   加载history列表中的后一个URL。
- go(参数)  加载history列表中的某个具体页面。
  - 参数如果是正数，则前进参数个历史记录页面
  - 参数如果是负数，则后退参数个历史记录页面

#### 常用属性：

- length  返回当前窗口列表中的URL数量。



## DOM

**DOM是Document Object Model，文档对象模型**

**DOM将标记语言文档的各个组成部分，封装成对象。可以使用这些对象，对标记语言文档进行CRUD的动态操作。**

***W3C DOM 标准被分为 3 个不同的部分：***

- **核心 DOM** 	针对任何结构化文档的标准模型

  - 核心DOM包括：		

    - Document：文档对象
    - Element：元素对象
    - Attribute：属性对象
    - Text：文本对象
    - Comment：注释对象
    - Node：节点对象，**是其他5个对象的父对象**

    

- **XML DOM**     针对任何XML文档的标准模型



- **HTML DOM**   针对任何HTML文档的标准模型



### 核心DOM模型：

### Document：文档对象

#### 创建：

1. window.document
2. document

#### 方法：

- 获取Element对象
  - getElementById() 	根据id属性值获取元素对象。id属性值一般来说唯一。
  - getElementsByTagName()     根据元素名称获取元素对象，返回值是一个数组。
  - getElementsByClassName()     根据class的属性值获取元素对象。返回值是一个数组。
  - getElementsByName()    根据name属性值获取元素对象。返回值是一个数组。
- 创建其他DOM对象
  - createAttribute(name)
  - createComment()
  - createElement()   常用 `var table = document.createElement("table");//创建了一个table对象`
  - createTextNode()



### Element：元素对象

#### 创建：

- 使用document来获取和创建

#### 常用方法：

- removeAttribute(属性参数)  删除指定的属性。
- setAttribute(参数1，参数2)  给目标对象设置一个属性。参数1是要设置的属性，参数2是属性参数。



### Node：节点对象

**Node对象是整个DOM的主要数据类型。**

**节点对象代表文档树中的一个单独的节点。**

#### 特点：所有DOM对象都可以被认为是一个节点

**DOM树：** ![HTML DOM Node Tree](https://www.w3school.com.cn/i/ct_htmltree.gif) 

#### 常用方法：

- CRUD dom树
  - appendChild()	向节点的子节点列表的结尾添加新的子节点。
  - removeChild()    删除并返回当前节点的指定子节点。
  - replaceChild()     用新节点替换一个子节点。

#### 属性：

- parentNode  返回当前节点的父节点。



### HTML DOM

- 标签体的设置和获取：使用innerHTML

  ```javascript
  table.innerHTML += "<tr>\n" +
              "        <td>"+id+"</td>\n" +
              "        <td>"+name+"</td>\n" +
              "        <td>"+gender+"</td>\n" +
              "        <td><a href=\"javascript:void(0);\" onclick=\"delTr(this);\" >删除</a></td>\n" +
              "    </tr>";
  //向table表格对象中添加一些html内容
  ```

- 使用html元素对象的属性

- 控制样式

  - **使用style属性来设置点击后的样式。**

  ```javascript
  <script>
      var div1 = document.getElementById("div1");
      div1.onclick = function () {
          div1.style.width = "100px";
          div1.style.border = "1px solid red";
          div1.style.fontSize = "20px";
      };
  </script>
  ```

  - **提前定义好css样式，通过元素的className属性拉力设置点击后的样式。**

  ```javascript
  <style>
          .d1 {
              border: blue 1px solid;
              width:200px;
              height:100px;
          }
  </style>
  <script>
      var div2 = document.getElementById("div2");
      div2.onclick = function () {
          div2.className = "d1";
  
      };
  </script>
  ```

  
