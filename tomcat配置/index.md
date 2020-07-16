# Tomcat的配置


## Tomcat的配置

- 部署项目方式的
  1. 直接将项目放到webapps目录下即可
     - 按照文件路径来访问
     - **简化部署**：将项目打成一个.war的包，再将war包放置到webapps目录下，tomcat会自动生成一个相应的文件夹，删除时直接删除war包也会自动删除相应的文件夹
  2. 配置conf/server.xml文件
     - 在<Host>标签体中配置
       - <Context docBase="项目存放的路径" path="虚拟路径名称">
  3. 在conf\Catalina\localhost创建任意名称的xml文件，在xml文件中编写内容
     - <Context docBase="项目路径">
     - 此时的虚拟目录就是xml的文件名
- 静态项目和动态项目
  - 目录结构
    - java动态项目
      - 项目根目录
        - WEB-INF目录
          - web.xml：web项目的核心配置文件
          - classes：放置字节码文件的目录
          - lib：放置依赖的jar包
- 将Tomcat集成到idea中
  - run->configurations->Templates里面选择tomcatserver->local或者remote
  - 另外在Deployment里面设置Applicant context为/，这样就可以不加路径直接访问项目
- 设置更新自动重新部署项目(热部署)
  - run->edit configurations->tomcats->Server
    - On Update action:选择**Update Resource**
    - On frame deactivation:选择**Update Resource**

### IDEA与tomcat的相关配置

1. idea会为每一个tomcat部署的项目单独建立一份配置文件
   - 查看控制台`log:Using CATALINA_BASE:   "C:\Users\cyunt\.IntelliJIdea2019.1\system\tomcat\_tomcatDemo"`
2. 工作空间项目和tomcat部署的web项目
   - tomcat真正访问的是**tomcat部署的web项目**，tmocat部署的web项目对应着**工作空间项目的web目录下的所有资源**
   - WEB-INF目录下的资源不能被浏览器直接访问
