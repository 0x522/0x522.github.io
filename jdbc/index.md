# JDBC


## 什么是 JDBC

**JDBC 规范定义接口，具体的实现由各大数据库厂商来实现。**

- JDBC 是 Java 访问数据库的标准规范，真正怎么操作数据库还需要具体的实现类，也就是数据库驱动。
  
- 每个数据库厂商根据自家数据库的通信格式编写好自己数据库的驱动。所以我们只需要会调用 JDBC 接口中的方法即可， 数据库驱动由数据库厂商提供。
  
- 使用 JDBC 的好处：
  1) 程序员如果要开发访问数据库的程序， 只需要会调用 JDBC 接口中的方法即可， 不用关注类是如何实现的。
  2) 使用同一套 Java 代码，进行少量的修改就可以访问其他 JDBC 支持的数据库  
  
  

- | 会使用到的包 | 说明                                                         |
  | ------------ | ------------------------------------------------------------ |
  | java.sql     | 所有与 JDBC 访问数据库相关的接口和类                         |
  | javax.sql    | 数据库扩展包，提供数据库额外的功能。如：连接池               |
  | 数据库的驱动 | 由各大数据库厂商提供，需要额外去下载，是对 JDBC 接口实现的类 |

- | 接口或类              | 作用                                                         |
  | --------------------- | ------------------------------------------------------------ |
  | DriverManager 类      | 1) 管理和注册数据库驱动 2) 得到数据库连接对象                |
  | Connection 接口       | 一个连接对象，可用于创建 Statement 和 PreparedStatement 对象 |
  | Statement 接口        | 一个 SQL 语句对象，用于将 SQL 语句发送给数据库服务器。       |
  | PreparedStatemen 接口 | 一个 SQL 语句对象，是 Statement 的子接口                     |
  | ResultSet 接口        | 用于封装数据库查询的结果集，返回给客户端 Java 程序           |
  
  ## IDEA中JDBC具体实现	
  
  ```java
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.SQLException;
  import java.sql.Statement;
  
  public class JdbCDemo {
      public static void main(String[] args) {
          Statement statement = null;
          Connection conn = null;
          try {
              //1.注册驱动
              Class.forName("com.mysql.jdbc.Driver");
              //2.定义sql语句
              String sql = "insert into account values(null,'王五',4000)";
              //3.获取Connection对象
              conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db1", "root", "root");
              //Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/db1", "root", "root");
              //4.获取执行sql的Statement对象
              statement = conn.createStatement();
              //5.执行sql
              int count = statement.executeUpdate(sql);
              //count 表示影响的行数
              if (count > 0) {
                  System.out.println("successful!");
              } else
                  System.out.println("failed!");
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //判断空指针异常
              if (statement != null) {
                  try {
                      //关闭资源
                      statement.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
              if (conn != null) {
                  try {
                      conn.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
          }
      }
  }
  ```
  



## 使用数据库连接池c3p0和Druid

​        **数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。这项技术能明显提高对数据库操作的性能。** 

```java
import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

//JDBCutils是一个工具类，里面包含了静态代码块(用来加载配置文件)/getConnection()和close方法,以下代码可能有未调用的演示类
public class JDBCutils {
    private static DataSource ds;
//加载配置文件
    static {
        try {
            Properties pro = new Properties();
            ClassLoader classLoader = JdbcUtils.class.getClassLoader();
            InputStream is = classLoader.getResourceAsStream("druidconfig.properties");
            pro.load(is);
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //获取数据库连接
    public static Connection getConnection() throws SQLException {
            return ds.getConnection();
    }

    //释放资源
    public static void close(Statement statement, Connection connection) {
        if(statement != null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(connection != null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    public static void close(ResultSet resultSet, Statement statement, Connection connection) {
        if(resultSet != null){
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        JDBCutils.close(null,statement,connection);
        /*if(statement != null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(connection != null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }*/
    }
    public static DataSource getDataSource(){
        return ds;
    }
}




//c3p0:

public class c3p0Demo1 {//演示类
    public static void main(String[] args) throws SQLException {
        Connection connection = null;
        //1.创建数据库连接池对象
        DataSource ds = new ComboPooledDataSource();
        //2.获取连接对象
        connection = ds.getConnection();
        System.out.println(connection);
    }
}

public class c3p0Demo2 {
    public static void main(String[] args) throws SQLException {
        //指定配置文件中的配置对象为otherc3p0，建立连接池对象
        DataSource ds = new ComboPooledDataSource("otherc3p0");
        for (int i = 0; i < 12; i++) {
            //otherc3p0的配置文件中有<property name = "maxPoolSize">8</property>，最大数据接连接池对象为8个，此处循环12次，观察能不能输出连接对象
            Connection connection = ds.getConnection();
            System.out.println(i+1+":"+connection);
            if(i == 5){//判断归还连接对象给数据库连接池
                connection.close();
            }
        }
    }
}



//Druid:

public class DruidDemo1 {
    public static void main(String[] args) throws Exception {
        ClassLoader classLoader = DruidDemo1.class.getClassLoader();
        URL resource = classLoader.getResource("druidconfig.properties");
        String path = resource.getPath();
/*      InputStream inputStream = classLoader.getResourceAsStream("druidconfig.properties");*/
        Properties pro = new Properties();
        pro.load(new FileReader(path));
        DataSource ds = DruidDataSourceFactory.createDataSource(pro);
        Connection conn = ds.getConnection();
        System.out.println(conn);
    }
}

public class DruidDemo2 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            //调用JDBCutils中的getConnection的方法
            connection = JDBCutils.getConnection();
            String sql = "insert into account values(null,?,?)";
            //这里使用PreperStatement，是一个预编译的执行sql类，可以用?代替预设置的值，可以避免sql注入问题，一定要在执行sql语句之前为PrepareStatement类的对象设置?的值
            preparedStatement = connection.prepareStatement(sql);
            //参数1表示?的位置，参数2是给它的值
            preparedStatement.setString(1, "zhaoliu");
            preparedStatement.setDouble(2,2000);
            preparedStatement.executeUpdate();
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
           	//调用JDBCutils工具类中的close方法,吧资源归还给数据库连接池
            JDBCutils.close(preparedStatement,connection);
        }
    }
}
```


