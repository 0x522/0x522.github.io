# session


## Session

- **概念**：服务器端会话技术，**在一次会话**的多次请求间共享数据，将数据保存在服务器端的对象中。**HttpSession**
  - HttpSession对象：
    - Object getAttribute(String name)
    - void setAttribute(String name, object value)
    - void removeAttribute(String name)
- **获取session对象**
  - HttpSession session = request.getSession();
- **Session原理**
  - Session的实现是依赖于cookie的，当第一次请求时，会创建一个session对象，并给他一个JsessionID，然后再次请求时getSession方法会寻找这个ID返回给session对象。所以在一次会话的多次请求中，使用的session是同一个。
- **细节**
  1. 当客户端关闭后，服务端不关闭，两次获取session是否为同一个？
     - 默认情况下，不是。
     - 可以手动设置使客户端关闭后再次访问服务器获取的session是同一个
       - 创建一个cookie，
         -  `Cookie cookie = new  Cookie("JSession",session.getId());`
       - 设置cookie对象的最大存活时间 1小时
         - `cookie.setMaxAge(60*60)`
  2. 客户端不关闭，服务器关闭后，两次获取的session是同一个吗？
     - 不是同一个，但是要确保数据不丢失。tomcat自动完成以下工作：
       - session的钝化：
         - 在服务器正常关闭之前，将session对象序列化到硬盘上。
       - session的活化：
         - 在服务器启动后，将session文件转化为内存中的session对象即可。
  3. session的失效时间？什么时候被销毁？
     1. 服务器关闭
     2. session对象调用`invalidate（）`
     3. session默认失效时间 30分钟
        - 如何修改失效时间
          - 找到web.xml里面的`<session-config>`标签修改`session-timeout`。
