# Servlet


## Servlet

- **基本概念**：运行在服务器端的小程序

  - Servlet就是一个接口，定义了java类被浏览器访问到的规则(被tomcat识别的规则)
  - 用户需要自定义一个类，实现Servlet接口，重写方法

- **第一个Servlet**

  - 创建JavaEE项目

  - 定义一个类，实现Servlet接口

    - ```java
      public class servletImp implements Servlet
      ```

  - 实现接口中的抽象方法

  - 配置Servlet

    - web.xml里面加入servlet标签

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

- **Servlet执行原理**

  1. 当服务器接收到客户端浏览器的请求后，会解析请求url路径，获取访问的Servlet的资源路径
  2. 查找web.xml文件，是否有对应的<url-pattern>标签体内容
  3. 如果有，则在找到对应的<servlet-class>里面的全类名
  4. tomcat会将字节码文件加载进内存，并创建其对象
  5. 调用Servlet的方法

- **Servlet中的方法**

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

- **Servlet生命周期**

  - **被创建**：执行init方法，只执行一次
    - **Servlet的init方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的**
      - 多个用户同时访问时，可能会出现安全问题
      - 解决方案：尽量不在Servlet中定义成员变量，应该在方法中定义局部变量，如果已经定义了成员变量，不要对变量修改值
    - **Servlet什么时候被创建？**
      - 默认情况下，第一次被访问时，Servlet被创建
      - 可以配置执行Servlet的创建时机，在web.xml中的<servlet>标签中指定<load-on-startup>标签的内容来指定Servlet的创建时机
        1. 第一次被访问时被创建：<load-on-startup>中的值为负数，默认值是-1
        2. 在服务器启动时被创建：<load-on-startup>中的值为大于1的整数
  - **提供服务**：执行service方法，执行多次
    - 每次访问Servlet时，Service方法都会被调用一次
  - **被销毁**：执行destroy方法，执行一次
    - Servlet被销毁时执行(销毁之前)，服务器关闭时，Servlet被销毁
    - 只有服务器正常关闭时，才会执行destroy方法

- **Servlet3.0支持注解配置**

  - Servlet3.0,可以支持注解配置，使用注解配置来代替繁琐的xml文档配置
  - 定义一个类，实现Servlet接口
  - 重写方法
  - 在类上使用@WebServlet注解，进行配置
    - @WebServlet("资源路径")
  
- **Servlet体系结构**

  - Servlet有两个抽象的实现类：**GenericServlet和HttpServlet**
    - GenericServlet将Servlet接口中除了service方法以外的其他方法重写并没有做具体实现，只有service方法被定义成了抽象方法，所以只要在自定义的类中重写service方法即可
    - HttpServlet是GenericServlet的子类，是对http协议的一种封装，用来简化操作
      1. 定义类集成HttpServlet
      2. 重现doGet和doPost方法

- **Servlet相关配置**

  1. url-pattern:Servlet访问路径
     - 一个Servlet可以**定义多个访问路径**：`@WebServlet({"/d2","/de2","/demo2"})`
     - 路径的定义规则：
       1. **/xxx**
       2. **/xxx/xcomxx**      **多层路径**：**可以使用*通配符**：`/*`或者`/user/*`**访问优先级最低**
       3. ***.a(后缀名随意设置)**     **带.a后缀名的路径**：任意路径加.a后缀来访问
          - **注意：/ *.a报错**

