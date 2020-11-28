# Servlet


## Servlet

- **基本概念**：运行在服务器端的小程序

  - Servlet 就是一个接口，定义了 java 类被浏览器访问到的规则(被服务器识别的规则)
  - 用户需要自定义一个类，实现 Servlet 接口，重写方法

- **第一个 Servlet**

  - 创建 JavaEE 项目

  - 定义一个类，实现 Servlet 接口

    - ```java
      public class servletImp implements Servlet
      ```

  - 实现接口中的抽象方法

  - 配置 Servlet

    - web.xml 里面加入 servlet 标签

    ```xml
    <servlet>
            <servlet-name>demo1</servlet-name>
            <servlet-class>com.servlet.servletImp</servlet-class>
    </servlet>
    <servlet-mapping>
            <servlet-name>demo1</servlet-name>
            <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
    ```

- **Servlet 执行原理**

  1. 当服务器接收到客户端浏览器的请求后，会解析请求 url 路径，获取访问的 Servlet 的资源路径
  2. 查找 web.xml 文件，是否有对应的<url-pattern>标签体内容
  3. 如果有，则在找到对应的<servlet-class>里面的全类名
  4. tomcat 会将字节码文件加载进内存，并创建其对象
  5. 调用 Servlet 的方法

- **Servlet 中的方法**

  - ```java
    /**
         * 初始化方法，在servlet创建时会执行一次
         * @param servletConfig
         * @throws ServletException
         */
    public void init(ServletConfig servletConfig) throws ServletException
    {

    }



    /**
         * 每一次servlet被访问时，执行，执行多次
         * @param servletRequest
         * @param servletResponse
         * @throws ServletException
         * @throws IOException
         */
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException
    {

    }



    /**
         * 销毁方法，服务器正常关闭时，执行，执行一次
         */
    public void destroy()
    {

    }



     /**
         * serclet配置对象,需要实现
         * @return
         */
    public ServletConfig getServletConfig()
    {
            return null;
    }



    /**
         * 获取servlet信息的方法，需要实现
         * @return
         */
    public String getServletInfo() {
            return null;
    }
    ```

- **Servlet 生命周期**

  - **被创建**：执行 init 方法，只执行一次
    - **Servlet 的 init 方法，只执行一次，说明一个 Servlet 在内存中只存在一个对象，Servlet 是单例的**
      - 多个用户同时访问时，可能会出现安全问题
      - 解决方案：尽量不在 Servlet 中定义成员变量，应该在方法中定义局部变量，如果已经定义了成员变量，不要对变量修改值
    - **Servlet 什么时候被创建？**
      - 默认情况下，第一次被访问时，Servlet 被创建
      - 可以配置执行 Servlet 的创建时机，在 web.xml 中的<servlet>标签中指定<load-on-startup>标签的内容来指定 Servlet 的创建时机
        1. 第一次被访问时被创建：<load-on-startup>中的值为负数，默认值是-1
        2. 在服务器启动时被创建：<load-on-startup>中的值为大于 1 的整数
  - **提供服务**：执行 service 方法，执行多次
    - 每次访问 Servlet 时，Service 方法都会被调用一次
  - **被销毁**：执行 destroy 方法，执行一次
    - Servlet 被销毁时执行(销毁之前)，服务器关闭时，Servlet 被销毁
    - 只有服务器正常关闭时，才会执行 destroy 方法

- **Servlet3.0 支持注解配置**

  - Servlet3.0,可以支持注解配置，使用注解配置来代替繁琐的 xml 文档配置
  - 定义一个类，实现 Servlet 接口
  - 重写方法
  - 在类上使用@WebServlet 注解，进行配置
    - @WebServlet("资源路径")

- **Servlet 体系结构**

  - Servlet 有两个抽象的实现类：**GenericServlet 和 HttpServlet**
    - GenericServlet 将 Servlet 接口中除了 service 方法以外的其他方法重写并没有做具体实现，只有 service 方法被定义成了抽象方法，所以只要在自定义的类中重写 service 方法即可
    - HttpServlet 是 GenericServlet 的子类，是对 http 协议的一种封装，用来简化操作
      1. 定义类集成 HttpServlet
      2. 重现 doGet 和 doPost 方法

- **Servlet 相关配置**

  1. url-pattern:Servlet 访问路径
     - 一个 Servlet 可以**定义多个访问路径**：`@WebServlet({"/d2","/de2","/demo2"})`
     - 路径的定义规则：
       1. **/xxx**
       2. **/xxx/xcomxx** **多层路径**：**可以使用\*通配符**：`/*`或者`/user/*`**访问优先级最低**
       3. **\*.a(后缀名随意设置)** **带.a 后缀名的路径**：任意路径加.a 后缀来访问
          - **注意：/ \*.a 报错**

