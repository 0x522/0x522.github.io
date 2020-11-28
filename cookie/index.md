# Cookie


### 会话技术

- 会话：一次会话中包含多次请求和响应。
  - 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止，算一次会话。
- 功能：在一次会话的范围内的多次请求间，共享数据。
- 方式：
  - 客户端会话技术：Cookie
  - 服务器端会话技术：Session

## Cookie

- **概念**：客户端会话技术，将数据保存到客户端
- 使用步骤：
  1. 创建 Cookie 对象，绑定数据
     - `new Cookie(String name,String value)`
  2. 发送 Cookie 对象
     - `response.addCookie(Cookie cookie)`
  3. 获取 Cookie 对象，拿到数据
     - `Cookie[] request.getCookies()`
- **原理**：
  - 基于响应头 set-cookie 和请求头 cookie 实现
  - 在一次会话中，当客户端请求服务器端的资源，服务器端会将设置好的 cookie 对象响应给客户端一个响应头`set-cookie：a = b`，浏览器根据 http 协议会在客户端的浏览器中键值 a = b 对并保存,然后再下一次请求服务器资源的时候，会携带一个请求头`cookie：a = b`,在服务器中就可以获取请求头中的数据进行操作
- **细节**
  1. 一次可不可以发送多个 cookie？
     - 可以
     - 可以创建多个 cookie 对象，使用 response 调用多次 addCookie 方法发送 cookie 即可
  2. cookie 在浏览器中保存多长时间？
     1. 默认情况下，当浏览器关闭后，cookie 数据被销毁，cookie 保存在浏览器内存中
     2. 持久化存储：
        - `setMaxAge(int seconds)`
          - 参数
            1. 正数：将 cookie 数据写到硬盘的文件中。持久化存储，cookie 存活的时间。
            2. 负数：默认值一次会话后销毁 cookie
            3. 零：删除 cookie 信息
  3. cookie 能不能存中文？
     - 在 tomcat 8 之前不能存中文，需要将中文数据转码---一般采用 URL 编码（%E3）
  4. cookie 获取范围多大？
     - 假设在一个 tomcat 服务器中，部署了多个 web 项目，那么在这些项目中 cookie 能不能共享
       - 默认情况下 cookie 不能共享
       - `cookie对象.setPath(String path)`:设置 cookie 的获取范围。默认情况下，设置当前的虚拟目录，共享就设置为/，是最大共享范围
     - 不同的 tomcat 服务器间 cookie 共享问题
       - `setDomain(String path)`:如果设置一级域名相同，那么多个服务器间 cookie 可以共享
       - 例如 setDomain(".baidu.com"),那么 tieba.baidu.com 和 news.baidu.com 中的 cookie 可以共享

