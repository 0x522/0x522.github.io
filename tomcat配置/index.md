# Tomcat的配置


## Tomcat 的配置

- 部署项目方式的
  1. 直接将项目放到 webapps 目录下即可
     - 按照文件路径来访问
     - **简化部署**：将项目打成一个.war 的包，再将 war 包放置到 webapps 目录下，tomcat 会自动生成一个相应的文件夹，删除时直接删除 war 包也会自动删除相应的文件夹
  2. 配置 conf/server.xml 文件
     - 在<Host>标签体中配置
       - <Context docBase="项目存放的路径" path="虚拟路径名称">
  3. 在 conf\Catalina\localhost 创建任意名称的 xml 文件，在 xml 文件中编写内容
     - <Context docBase="项目路径">
     - 此时的虚拟目录就是 xml 的文件名
- 静态项目和动态项目
  - 目录结构
    - java 动态项目
      - 项目根目录
        - WEB-INF 目录
          - web.xml：web 项目的核心配置文件
          - classes：放置字节码文件的目录
          - lib：放置依赖的 jar 包
- 将 Tomcat 集成到 idea 中
  - run->configurations->Templates 里面选择 tomcatserver->local 或者 remote
  - 另外在 Deployment 里面设置 Applicant context 为/，这样就可以不加路径直接访问项目
- 设置更新自动重新部署项目(热部署)
  - run->edit configurations->tomcats->Server
    - On Update action:选择**Update Resource**
    - On frame deactivation:选择**Update Resource**

### IDEA 与 tomcat 的相关配置

1. idea 会为每一个 tomcat 部署的项目单独建立一份配置文件
   - 查看控制台`log:Using CATALINA_BASE: "C:\Users\cyunt\.IntelliJIdea2019.1\system\tomcat\_tomcatDemo"`
2. 工作空间项目和 tomcat 部署的 web 项目
   - tomcat 真正访问的是**tomcat 部署的 web 项目**，tmocat 部署的 web 项目对应着**工作空间项目的 web 目录下的所有资源**
   - WEB-INF 目录下的资源不能被浏览器直接访问

