# Session


## Session

- **概念**：服务器端会话技术，**在一次会话**的多次请求间共享数据，将数据保存在服务器端的对象中。**HttpSession**
  - HttpSession 对象：
    - Object getAttribute(String name)
    - void setAttribute(String name, object value)
    - void removeAttribute(String name)
- **获取 session 对象**
  - HttpSession session = request.getSession();
- **Session 原理**
  - Session 的实现是依赖于 cookie 的，当第一次请求时，会创建一个 session 对象，并给他一个 JsessionID，然后再次请求时 getSession 方法会寻找这个 ID 返回给 session 对象。所以在一次会话的多次请求中，使用的 session 是同一个。
- **细节**
  1. 当客户端关闭后，服务端不关闭，两次获取 session 是否为同一个？
     - 默认情况下，不是。
     - 可以手动设置使客户端关闭后再次访问服务器获取的 session 是同一个
       - 创建一个 cookie，
         - `Cookie cookie = new Cookie("JSession",session.getId());`
       - 设置 cookie 对象的最大存活时间 1 小时
         - `cookie.setMaxAge(60*60)`
  2. 客户端不关闭，服务器关闭后，两次获取的 session 是同一个吗？
     - 不是同一个，但是要确保数据不丢失。tomcat 自动完成以下工作：
       - session 的钝化：
         - 在服务器正常关闭之前，将 session 对象序列化到硬盘上。
       - session 的活化：
         - 在服务器启动后，将 session 文件转化为内存中的 session 对象即可。
  3. session 的失效时间？什么时候被销毁？
     1. 服务器关闭
     2. session 对象调用`invalidate（）`
     3. session 默认失效时间 30 分钟
        - 如何修改失效时间
          - 找到 web.xml 里面的`<session-config>`标签修改`session-timeout`。

