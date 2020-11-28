# 请求响应原理原理


## Request 和 Response

**假设一个场景：我们使用客户端的浏览器输入 url 去访问服务器(tomcat)，叫做一个请求，请求包含请求消息，服务器会最终会给我们一个响应，这里面的响应消息是人为设置的，然而这里面的具体操作如下：**

1. tomcat 服务器会根据请求的 url 中的资源路径(这里是虚拟目录，人为配置的资源路径)，创建对应的 Servlet 实现类对象
2. tomcat 服务器，会创建 request 和 response 对象，request 对象中封装请求消息数据
3. tomcat 将 request 和 response 两个对象传递给**service**方法，并且调用**service**方法，进行具体的操作
4. 我们可以通过 request 对象获取请求消息数据，通过 response 对象设置响应消息数据
5. 服务器在给浏览器做出响应之前，会从 response 对象中拿已经设置好的响应消息数据

- **request 和 response 对象的原理**

  1. request 和 response 对象是由服务器创建的
  2. request 对象是来获取请求消息，response 对象是来设置响应消息

- **request 对象继承的体系结构**

  - ServletRequest 接口
  - HttpServletRequest 接口是 ServletRequest 的子接口，由 tomcat 服务器来实现
    - 具体是在 tomcat 源码下的**java.org.apache.catalina.connector.RequestFacade 类实现了 HttpServletRequest**

- **request 功能**

  - 获取请求消息数据

    1. **获取请求行的数据**

       - **例：GET(请求方式) /tomcat/demo2?name=zhangsan HTTP/1.1**
       - 方法：
         1. 获取请求方式：获取 GET、POST 或者其他请求方式
            - `String getMethod()`
         2. 获取虚拟目录，这里是`/tomcat`：
            - `String getContextPath()`
         3. 获取 Servlet 路径，这里是`/demo2`：
            - `String getServletPath()`
         4. 获取**get 方式**的请求参数,这里是`name=zhangsan`：
            - `String getQueryString()`
         5. 获取请求的 URI/URL(**URL 是 URI 的子集，URL 可以是 URL，URI 不一定是 URL，URL 可以实现某个资源的具体 locate**)
            - `String getRequestURI()` ，这里是 `/tomcat/demo2`
            - `StringBuffer getRequestURL()`，这里是 `https://localhost:80/tomcat/demo2`
         6. 获取协议以及版本 `HTTP/1.1`
            - `String getProtocol()`
         7. 获取客户机的 ip 地址
            - `String getRemoteAddr()`

    2. **获取请求头的数据**

       **方法：**

       - `String getHeader(String name)` 通过请求头的名称获取请求头的值，**参数值不区分大小写**

         **常用请求头：**

         1. user-agent：使用的浏览器版本
         2. referer：发出请求的 URL

       - `Enumeration<String> getHeaderNames()` 获取所有的请求头名称

    3. **获取请求体的数据**

       - 请求体：只有 Post 请求方式，才有请求体，在请求体中封装了 Post 请求的请求参数
       - 获取方法：
         1. 获取流对象
            - `BufferedReader getReader()` 获取字符输入流，只能操作字符数据
            - `ServletInputStream getInputStream()` 获取字节输入流，可以操作所有类型的数据
         2. 再从流对象中拿数据

    4. 其他功能

       1. 获取请求参数通用的方法，**兼容 get 和 post 两种请求方式**

          1. `String getParameter(String name)` 根据参数名称获取参数值

          2. `String[] getParameterValues(String name)` 根据参数名称获取参数值的数组

          3. `Enumeration<String> getParameterNames()` 获取所有请求参数的名称

          4. `Map<String,String[]> getParameterMap()` 获取所有参数的 Map 集合

             - 使用`keySet()`可以把 Map 里的**key**封装成**Set**，再通过`get()`来获取每一个**key**对应的**value[]**
             - 使用`entrySet()`

             **中文乱码问题**

             - 在 doPost 方法中的获取请求参数之前使用`request.setCharacterEncoding("utf-8");`可以避免乱码

       2. 请求转发：一种在服务器内部的资源跳转方式

          步骤：

          1. 通过 Request 对象获取请求转发器对象：RequestDispatcher `getRequestDispatcher (String path)`
          2. 通过 RequestDispatcher 对象来进行转发：`forward(ServletRequest request,Servlet Response response)`

          特点：

          1. 浏览器地址栏路径不发生变化
          2. 只能转发到当前服务器内部资源中
          3. 转发是一次请求

       3. 共享数据：

          - 域对象：一个有作用范围的对象，可以在范围内共享数据
          - request 域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
          - 方法：
            1. `setAttribute(String name,Object obj)`：存储数据
            2. `Object getAttribute(String name)`：通过键获取值
            3. `removeAttribute(String name)`：通过键移除键值对

       4. 获取 ServletContext ：

          `request.getServletContext()`

