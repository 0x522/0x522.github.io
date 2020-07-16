# cookie




### 会话技术

- 会话：一次会话中包含多次请求和响应。
  - 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止，算一次会话。
- 功能：在一次会话的范围内的多次请求间，共享数据
- 方式：
  - 客户端会话技术：Cookie
  - 服务器端会话技术：Session



## Cookie

- **概念**：客户端会话技术，将数据保存到客户端
- 使用步骤：
  1. 创建Cookie对象，绑定数据
     - `new Cookie(String name,String value)`
  2. 发送Cookie对象
     - `response.addCookie(Cookie cookie)`
  3. 获取Cookie对象，拿到数据
     - `Cookie[] request.getCookies()`
- **原理**：
  - 基于响应头set-cookie和请求头cookie实现
  - 在一次会话中，当客户端请求服务器端的资源，服务器端会将设置好的cookie对象响应给客户端一个响应头`set-cookie：a = b`，浏览器根据http协议会在客户端的浏览器中键值a = b对并保存,然后再下一次请求服务器资源的时候，会携带一个请求头`cookie：a = b`,在服务器中就可以获取请求头中的数据进行操作
- **细节**
  1. 一次可不可以发送多个cookie？
     - 可以
     - 可以创建多个cookie对象，使用response调用多次addCookie方法发送cookie即可
  2. cookie在浏览器中保存多长时间？
     1. 默认情况下，当浏览器关闭后，cookie数据被销毁，cookie保存在浏览器内存中
     2. 持久化存储：
        - `setMaxAge(int seconds)`
          - 参数
            1. 正数：将cookie数据写到硬盘的文件中。持久化存储，cookie存活的时间。
            2. 负数：默认值一次会话后销毁cookie
            3. 零：删除cookie信息
  3. cookie能不能存中文？
     - 在tomcat 8 之前不能存中文，需要将中文数据转码---一般采用URL编码（%E3）
  4. cookie获取范围多大？
     - 假设在一个tomcat服务器中，部署了多个web项目，那么在这些项目中cookie能不能共享
       - 默认情况下cookie不能共享
       - `cookie对象.setPath(String path)`:设置cookie的获取范围。默认情况下，设置当前的虚拟目录，共享就设置为/，是最大共享范围
     - 不同的tomcat服务器间cookie共享问题
       - `setDomain(String path)`:如果设置一级域名相同，那么多个服务器间cookie可以共享
       - 例如setDomain(".baidu.com"),那么tieba.baidu.com和news.baidu.com中的cookie可以共享
