# 请求响应原理原理


## Request和Response

**假设一个场景：我们使用客户端的浏览器输入url去访问服务器(tomcat)，叫做一个请求，请求包含请求消息，服务器会最终会给我们一个响应，这里面的响应消息是人为设置的，然而这里面的具体操作如下：**

1. tomcat服务器会根据请求的url中的资源路径(这里是虚拟目录，人为配置的资源路径)，创建对应的Servlet实现类对象
2. tomcat服务器，会创建request和response对象，request对象中封装请求消息数据
3. tomcat将request和response两个对象传递给**service**方法，并且调用**service**方法，进行具体的操作
4. 我们可以通过request对象获取请求消息数据，通过response对象设置响应消息数据
5. 服务器在给浏览器做出响应之前，会从response对象中拿已经设置好的响应消息数据



- **request和response对象的原理**

  1. request和response对象是由服务器创建的
  2. request对象是来获取请求消息，response对象是来设置响应消息

- **request对象继承的体系结构**

  - ServletRequest接口
  - HttpServletRequest接口是ServletRequest的子接口，由tomcat服务器来实现
    - 具体是在tomcat源码下的**java.org.apache.catalina.connector.RequestFacade类实现了HttpServletRequest**

- **request功能**

  - 获取请求消息数据

    1. **获取请求行的数据**
       
       - **例：GET(请求方式)    /tomcat/demo2?name=zhangsan  	HTTP/1.1**
       - 方法：
         1. 获取请求方式：获取GET、POST或者其他请求方式
            - `String getMethod()`
         2. 获取虚拟目录，这里是`/tomcat`：
            - `String getContextPath()`
         3. 获取Servlet路径，这里是`/demo2`：
            - `String getServletPath()`
         4. 获取**get方式**的请求参数,这里是`name=zhangsan`：
            - `String getQueryString()` 
         5. 获取请求的URI/URL(**URL是URI的子集，URL可以是URL，URI不一定是URL，URL可以实现某个资源的具体locate**)
            - `String getRequestURI()` ，这里是 `/tomcat/demo2`
            - `StringBuffer getRequestURL()`，这里是 `https://localhost:80/tomcat/demo2`
         6. 获取协议以及版本 `HTTP/1.1`
            - `String getProtocol()`
         7. 获取客户机的ip地址
            - `String getRemoteAddr()`
       
    2. **获取请求头的数据**
    
       **方法：**
    
       - `String getHeader(String name)` 通过请求头的名称获取请求头的值，**参数值不区分大小写**
    
         **常用请求头：**
    
         1. user-agent：使用的浏览器版本
         2. referer：发出请求的URL
    
       - `Enumeration<String> getHeaderNames()` 获取所有的请求头名称
    
    3. **获取请求体的数据**
    
       - 请求体：只有Post请求方式，才有请求体，在请求体中封装了Post请求的请求参数
       - 获取方法：
         1. 获取流对象
            - `BufferedReader getReader()` 获取字符输入流，只能操作字符数据
            - `ServletInputStream getInputStream()`  获取字节输入流，可以操作所有类型的数据
         2. 再从流对象中拿数据
    
    4. 其他功能
    
       1. 获取请求参数通用的方法，**兼容get和post两种请求方式**
    
          1. `String getParameter(String name)`  根据参数名称获取参数值
    
          2. `String[] getParameterValues(String name)` 根据参数名称获取参数值的数组
    
          3. `Enumeration<String> getParameterNames()` 获取所有请求参数的名称
    
          4. `Map<String,String[]> getParameterMap()` 获取所有参数的Map集合
    
             - 使用`keySet()`可以把Map里的**key**封装成**Set**，再通过`get()`来获取每一个**key**对应的**value[]**
             - 使用`entrySet()`
    
             **中文乱码问题**
    
             - 在doPost方法中的获取请求参数之前使用`request.setCharacterEncoding("utf-8");`可以避免乱码
    
       2. 请求转发：一种在服务器内部的资源跳转方式
    
          步骤：
          
          1. 通过Request对象获取请求转发器对象：RequestDispatcher `getRequestDispatcher (String path)`
          2. 通过RequestDispatcher对象来进行转发：`forward(ServletRequest request,Servlet Response response)`
          
          特点：
          
          1. 浏览器地址栏路径不发生变化
          2. 只能转发到当前服务器内部资源中
          3. 转发是一次请求
          
       3. 共享数据：
       
          - 域对象：一个有作用范围的对象，可以在范围内共享数据
          - request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
          - 方法：
            1. `setAttribute(String name,Object obj)`：存储数据
            2. `Object  getAttribute(String name)`：通过键获取值
            3. `removeAttribute(String name)`：通过键移除键值对
       
       4. 获取ServletContext ：
       
          `request.getServletContext()`
    
