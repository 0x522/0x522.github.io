# XML


# XML

**XML**

- **概念：**Extensible Markup language 可扩展标记语言
  
  - 可扩展：标签都是自定义的
- **功能：**
  - 存储数据
    - 配置文件
    - 在网络中传输数据
- **XML和HTML的区别**
  
  1. xml标签都是自定义的，html标签都是预定义的
  2. xml的语法严格，html的语法松散
3. xml是存储数据的，html是展示数据的

- **语法**

  1. xml文档的后缀名是.xml
  2. xml文档的第一行必须写文档声明
  3. xml文档中有且仅有一个根标签
  4. 属性值必须使用引号引起来
  5. 标签必须正确关闭
  6. xml标签名称区分大小写

- **XML组成部分**

  1. 文档声明：

     1. 格式：<?xml 属性列表 ?>

     2. 属性列表：

        - version 版本号

        - encoding 编码方式：告知解析引擎当前文档使用的字符集

        - standalone 是否独立：

          ​	取值：yes：依赖其他文件   no：不依赖其他文件

  2. 指令：结合css来控制xml中标签的样式

  3. 标签：标签名称自定义，遵循万维网指定的规则

  4. 属性：id值唯一

  5. 文本：

     - CDATA区：在该区域中的数据会被原样展示
       - 格式：<![CDATA[数据]]>

  6. 约束：规定xml文档的书写规则

     - 作为框架的使用者

       1. 能够在xml中引入约束文档
       2. 能够读懂约束文档

     - 分类

       1. DTD：一种简单的约束技术
          - 引入dtd文档到xml文档中
            - 内部dtd：将约束规则定义在xml文档中
            - 外部dtd：将约束的规则定义在外部的dtd文件中
              - 本地：<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
              - 网络：<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" “dtd文件位置的URL">
       2. Schema；一种复杂的约束技术，可以限定标签的内容
          - 后缀名.xsd
          - 引入Schema文档到xml
            1. 填写xml文档的根元素
            2. 引入xsi前缀    `xmlns:xsi=""`
            3. 引入xsd文件命名空间  `xsi:schemaLocation="xsd文档的URL名    XXX.xsd"`
            4. 为每一个xsd约束声明一个前缀作为标识   `xmlns="xsd文档的URL名"`

     - 解析XML文档

       - 操作xml文档

         1. 解析(读取)：将文档中的数据读取到内存中
         2. 写入：将内存中的数据保存到xml文档中，持久化的存储

       - 解析xml的方式

         - DOM：将标记语言文档一次性加载进内存，生成一棵**DOM树**
           - 优点：操作方便，可以对文档进行CRUD的所有操作
           - 缺点：占内存多
         - SAX：逐行读取，基于事件驱动的
           - 优点：不占内存
           - 缺点：只能读取，读取完立即释放，不能CRUD

       - xml常见的解析器

         1. JAXP：sun公司提供的解析器，支持dom和sax两种思想
         2. DOM4J：一款非常优秀的解析器
         3. Jsoup；Jsoup是一款java的HTML解析器，也可以用来解析XML。它提供了一套非常省力的API，可通过DOM，css以及类似于JQuery的操作方法来取出和操作数据
         4. PULL：安卓操作系统内置的解析器，sax方式的

         **Jsoup的使用**

         1. 导入jar包
         2. 获取Document对象
         3. 获取对应的标签Element对象
         4. 获取数据

         ```java
         public class JsoupDemo {
             public static void main(String[] args) throws IOException {
                 //获取document对象，根据xml文档获取
                 //获取student.xml的path
                 System.out.println();
                 String path = JsoupDemo.class.getClassLoader().getResource("student.xml").getPath();
                 Jsoup.parse(path);
                 //解析xml文档，加载文档进内存，获取dom树
                 Document document = Jsoup.parse(new File(path), "utf-8");
                 //获取元素对象
                 Elements names = document.getElementsByTag("name");
                 Element name = names.get(0);
                 String ele = name.text();
                 System.out.println(ele);
             }
         }
         ```

         **student.xml**:

         ```xml
         <?xml version="1.0" encoding="utf-8" ?>
         <student>
             <student number="0001">
                 <name>zhangsan</name>
                 <age>18</age>
                 <sex>male</sex>
             </student>
             <student number="0002">
                 <name>lisi</name>
                 <age>20</age>
                 <sex>female</sex>
             </student>
         </student>
         ```

     - **对象的使用**

       1. Jsoup：工具类，可以解析html或xml文档，返回Document
          - parse：解析html或xml文档，返回Document
            - `parse(File in, String charsetName)` 解析xml或html文件
            - `parse(String html)` 解析xml或html字符串
            - `parse(URL url, int timeoutMillis)` 通过网络路径解析
          
       2. Document：文档对象。代表内存中的dom树
          - 获取Element对象
            - getElementById(String id) 根据id属性值获取唯一的element对象
            - getElementByTag(String tagName) 根据标签名称获取元素对象集合
            - getElementByAttribute(String key) 根据属性名称获取元素对象的集合
            - getElementByAttributeValue(String key,String value) 根据对应的属性名和属性值获取元素对象集合
          
       3. Elements：元素Element对象的**集合**，可以当作ArrayList<Element>来使用
       
       4. Element：元素对象
       
          1. 获取element对象(获取的是子标签对象)
             - getElementById(String id) 根据id属性值获取唯一的element对象
             - getElementByTag(String tagName) 根据标签名称获取元素对象集合
             - getElementByAttribute(String key) 根据属性名称获取元素对象的集合
             - getElementByAttributeValue(String key,String value) 根据对应的属性名和属性值获取元素对象集合
          2. 获取属性值
             - String attr(String key) 根据属性名称获取属性值，属性名不区分大小写
          3. 获取文本内容
             - String text() 获取文本内容，所有纯文本内容
             - String html() 获取标签体所有内容，包括子标签的标签和文本内容
       
       5. Node：节点对象
       
          - document和element的父类
       
     - **快捷的查询方式**
     
       - selector：选择器
     
         - 使用方法：Elements    select(String cssQuery)
           - document.select("标签名称/#id名称/.类名称");
     
       - Xpath：是xml路径语言，它是一种用来确定XML文档中某部分位置的语言
     
         - 使用Jsoup的Xpath需要额外导入jar包
     
         - 根据document对象，创建JXDocument对象，来自JsoupXpath包
     
         - ##### 语法详情见w3school文档

