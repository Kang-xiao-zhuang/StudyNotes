# JDBC和数据库连接池

**写在前面**

**学习地址：[【韩顺平讲JDBC】](https://www.bilibili.com/video/BV1zv41157NC)**

**感谢韩老师的视频！十分感谢**

# 1. JDBC概述

## 1.1 基本介绍

- JDBC为访问不同的数据库提供了统一的接口，为使用者屏蔽了细节问题
- 使用JDBC,可以连接任何提供JDBC驱动程序的数据库系统，从而完成对数据库的各种操作

## 1.2 原理图

![](https://pic.imgdb.cn/item/60c8046c844ef46bb28ab3af.jpg)

## 1.3 好处

JDBC是用Java提供一套用于数据库操作的接口API，Java程序员只需要面向接口编程即可，不同的数据库厂商，需要针对这套接口，提供不同的实现。

![](https://pic.imgdb.cn/item/60c80569844ef46bb296377e.jpg)

# 2. JDBC入门

首先创建一个数据库和一张表

```sql
CREATE DATABASE `mybatis`;

USE `mybatis`;

CREATE TABLE `user`(

 `id` INT(20)NOT NULL PRIMARY KEY,
 `name` VARCHAR(30)DEFAULT NULL,
 `pwd` VARCHAR(30)DEFAULT NULL
)ENGINE=INNODB DEFAULT CHARSET=utf8;
```

## 2.1 演示一把

```java
package com.zhuang.jdbc;

import com.mysql.jdbc.Driver;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;

/**
 * @Classname Jdbc01
 * @Description 示范如何使用Jdbc
 * @Date 2021/6/15 8:50
 * @Created by dell
 */

public class Jdbc01 {
    public static void main(String[] args) throws Exception {
        //注册驱动
        Driver driver = new Driver();
        //sql语句
        /**
         *  jdbc:mysql: 规定好的协议，通过jdbc的方式连接mysql
         *  localhost 表示主机
         *  3306 表示mysql监听的端口
         *  mybatis表示哪个数据库
         */
        String url = "jdbc:mysql://localhost:3306/mybatis";
        //将账号密码 放入到Properties
        Properties properties = new Properties();
        //账号密码
        properties.setProperty("user", "root");
        properties.setProperty("password", "root");
        Connection connect = driver.connect(url, properties);

        //执行sql
        String sql = "insert into user value ('10','张三','123')";
        //statement用于执行静态SQL语句并返回其生成的结果的对象
        Statement statement = connect.createStatement();
        int row = statement.executeUpdate(sql);
        System.out.println(row > 0 ? "成功" : "失败");

        statement.close();
        connect.close();
    }
}
```



## 2.2 连接数据库的5种方式

```java
package com.zhuang.jdbc;

import com.mysql.jdbc.Driver;

import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

/**
 * @Classname JdbcConn
 * @Description java 连接 mysql 的 5种方式
 * @Date 2021/6/15 9:07
 * @Created by dell
 */

public class JdbcConn {
    public static void main(String[] args) throws Exception {
        connect01();
        connect02();
        connect03();
        connect04();
        connect05();
    }

    private static void connect01() throws Exception {
        //创建Driver对象
        Driver driver = new Driver();
        String url = "jdbc:mysql://localhost:3306/mybatis";
        //将账号密码 放入到Properties
        Properties properties = new Properties();
        //账号密码
        properties.setProperty("user", "root");
        properties.setProperty("password", "root");
        Connection connect = driver.connect(url, properties);
        System.out.println(connect);
    }

    private static void connect02() throws Exception {
        //使用 DriverManager 替代 driver 进行统一管理
        //使用反射加载 Driver 类 , 动态加载，更加的灵活，减少依赖性
        Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");

        Driver driver = (Driver) aClass.newInstance();

        String url = "jdbc:mysql://localhost:3306/mybatis";
        //将账号密码 放入到Properties
        Properties properties = new Properties();
        //账号密码
        properties.setProperty("user", "root");
        properties.setProperty("password", "root");
        Connection connect = driver.connect(url, properties);
        System.out.println("方式 2=" + connect);
    }

    private static void connect03() throws Exception {
        //使用反射加载 Driver 类 , 动态加载，更加的灵活，减少依赖性
        Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");

        Driver driver = (Driver) aClass.newInstance();
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println("方式3=" + connection);
    }

    private static void connect04() throws Exception {
        //使用反射加载了 Driver 类
        //在加载 Driver 类时，完成注册
        /*
        源码: 1. 静态代码块，在类加载时，会执行一次. 2. DriverManager.registerDriver(new Driver());
        3. 因此注册 driver 的工作已经完成
        static {
        try {
        DriverManager.registerDriver(new Driver());
        } catch (SQLException var1) {
        throw new RuntimeException("Can't register driver!");
        }
        }
        */
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println("方式4=" + connection);
    }

    private static void connect05() throws Exception {
        //通过 Properties 对象获取配置文件的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\jdbc.properties"));
//获取相关的值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        Class.forName(driver);
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println("方式 5 " + connection);
    }
}
```

# 3. ResultSet

## 3.1 基本介绍

1. 表示数据库结果集的数据表，通常通过执行查询数据库的语句生成
2. `ResultSet`对象保持一个光标指向其当前的数据行，最初，光标位于第一行
3. next方法将光标移动到下一行，并且由于在ResultSet对象中没有更多行时返回false，因此可以用`while`循环来遍历结果集

## 3.2 代码示例

```java
package com.zhuang.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 * @Classname ResultSet_
 * @Description ResultSet结果集
 * @Date 2021/6/15 12:53
 * @Created by dell
 */

public class ResultSet_ {
    public static void main(String[] args) throws Exception{
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        //得到 statement对象
        Statement statement = connection.createStatement();
        //sql语句
        String sql = "select id,name,pwd from user";
        // 调用查询方法 返回结果集
        ResultSet resultSet = statement.executeQuery(sql);
        while (resultSet.next()){
            int id = resultSet.getInt(1);
            String name = resultSet.getString(2);
            String pwd = resultSet.getString(3);
            System.out.println(id + "\t" + name + "\t" + pwd );
        }
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

# 4. Statement

## 4.1 基本介绍

1. `Statement`对象，用于执行静态SQL语句并返回其生成的结果的对象
2. 在建立连接后，需要对数据库进行访问，执行命名或是SQL语句，可以通过
- `Statement` **存在SQL注入问题**
- `PreparedStatement` **预处理**
- `CallableStatement` **存储过程** 
4. Statement对象执行SQL语句，存在SQL注入风险
5. SQL注入是利用某些系统对用户输入的数据进行充分的检查，而在用户输入数据中非法的SQL语句段或命令，恶意攻击数据库
6. 要防范SQL注入，用`PrepareStatement`取代`Statement`即可

## 4.2 代码演示

首先创个表

```sql
-- 管理员表
CREATE TABLE admin ( 
NAME VARCHAR(32) NOT NULL UNIQUE, pwd VARCHAR(32) NOT NULL DEFAULT '') CHARACTER SET utf8;

-- 添加数据
INSERT INTO admin VALUES('tom', '123'); -- 查找某个管理是否存在
SELECT *
FROM admin
WHERE NAME = 'tom' AND pwd = '123'

-- SQL
-- 输入用户名 为 1' or
-- 输入万能密码 为 or '1'= '1
SELECT *
FROM admin
WHERE NAME = '1' OR' AND pwd = 'OR '1'= '1'
```

**Statement_**

```java
package com.zhuang.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Scanner;

/**
 * @Classname Statement_
 * @Description Statement演示
 * @Date 2021/6/15 13:17
 * @Created by dell
 */

public class Statement_ {
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(System.in);
        //让用户输入管理员名和密码
        System.out.print("请输入管理员的名字: ");
        //next(): 当接收到 空格或者 '就是表示结束
        String adminName = scanner.nextLine();
        //如果希望看到 SQL 注入，这里需要用 nextLine
        System.out.print("请输入管理员的密码: ");
        String adminPwd = scanner.nextLine();
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        //得到 statement对象
        Statement statement = connection.createStatement();
        //sql语句
        String sql = "select name,pwd from admin " +
                "where name ='" + adminName + "' and pwd = '" + adminPwd + "'";
        ResultSet resultSet = statement.executeQuery(sql);

        if (resultSet.next()) {
            System.out.println("查询成功");
        } else {
            System.out.println("查询失败");
        }
        connection.close();
        resultSet.close();
        statement.close();
    }
}
```

# 5. PrepareStatement

## 5.1 基本介绍

1. `PrepareStatement`执行的SQL语句中参数用`?`表示，调用`PrepareStatement`对象的setXXX()方法来设置这些参数

- setXXX()方法 有两个参数，第一个参数是设置SQL语句中的参数的索引(从1开始)，第二个参数是设置 SQL语句中的参数的值

2. 调用`excuteQuery()`返回`Result`对象
3. 调用`excuteUpdate()`执行更新，包括增加，修改，删除

## 5.2 预处理的好处

1. 不再使用+拼接SQL语句，减少语法错误
2. 有效解决SQL注入问题
3. 大大减少编译次数，效率较高



## 5.3 代码演示

```java
package com.zhuang.jdbc;

import java.sql.*;
import java.util.Scanner;

/**
 * @Classname PreparedStatement_
 * @Description PreparedStatement使用
 * @Date 2021/6/15 13:32
 * @Created by dell
 */

public class PreparedStatement_ {
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(System.in);
        //让用户输入管理员名和密码
        System.out.print("请输入管理员的名字: ");
        //next(): 当接收到 空格或者 '就是表示结束
        String adminName = scanner.nextLine();
        //如果希望看到 SQL 注入，这里需要用 nextLine
        System.out.print("请输入管理员的密码: ");
        String adminPwd = scanner.nextLine();
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        //得到 statement对象
        Statement statement = connection.createStatement();
        //sql语句
        String sql = "select * from admin where NAME = ? and pwd = ?";
        //preparedStatement 对象实现了 PreparedStatement 接口的实现类的对象
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        //给 ? 赋值
        preparedStatement.setString(1, adminName);
        preparedStatement.setString(2, adminPwd);
        ResultSet resultSet = preparedStatement.executeQuery();
        if (resultSet.next()) {
            System.out.println("查询成功");
        } else {
            System.out.println("查询失败");
        }
        connection.close();
        resultSet.close();
        statement.close();
    }
}
```

## 5.4 CRUD操作

**插入操作**

```java
package com.zhuang.jdbc;

import java.sql.*;
import java.util.Scanner;

/**
 * @Classname PreparedStatement_
 * @Description PreparedStatement使用
 * @Date 2021/6/15 13:32
 * @Created by dell
 */

public class PreparedStatement_ {
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(System.in);
        //让用户输入管理员名和密码
        System.out.print("请输入管理员的名字: ");
        //next(): 当接收到 空格或者 '就是表示结束
        String adminName = scanner.nextLine();
        //如果希望看到 SQL 注入，这里需要用 nextLine
        System.out.print("请输入管理员的密码: ");
        String adminPwd = scanner.nextLine();
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        //得到 statement对象
        Statement statement = connection.createStatement();
        //sql语句
      //  String sql = "select * from admin where NAME = ? and pwd = ?";
        String sql = "insert into admin values(?, ?)";
        //preparedStatement 对象实现了 PreparedStatement 接口的实现类的对象
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        //给 ? 赋值
        preparedStatement.setString(1, adminName);
        preparedStatement.setString(2, adminPwd);
        int i = preparedStatement.executeUpdate();
        System.out.println(i>0? "成功":"失败");
        connection.close();
       // resultSet.close();
        statement.close();
    }
}
```

**删除操作**

```java
package com.zhuang.jdbc;

import java.sql.*;
import java.util.Scanner;

/**
 * @Classname PreparedStatement_
 * @Description PreparedStatement使用
 * @Date 2021/6/15 13:32
 * @Created by dell
 */

public class PreparedStatement_ {
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(System.in);
        //让用户输入管理员名和密码
        System.out.print("请输入管理员的名字: ");
        //next(): 当接收到 空格或者 '就是表示结束
        String adminName = scanner.nextLine();
        //如果希望看到 SQL 注入，这里需要用 nextLine
        System.out.print("请输入管理员的密码: ");
        String adminPwd = scanner.nextLine();
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/mybatis";
        String user = "root";
        String password = "root";
        Connection connection = DriverManager.getConnection(url, user, password);
        //得到 statement对象
        Statement statement = connection.createStatement();
        //sql语句
        //  String sql = "select * from admin where NAME = ? and pwd = ?";
        //String sql = "insert into admin values(?, ?)";
        //preparedStatement 对象实现了 PreparedStatement 接口的实现类的对象
        String sql = "delete from admin where name = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        //给 ? 赋值
        preparedStatement.setString(1, adminName);
     //   preparedStatement.setString(2, adminPwd);
        // ResultSet resultSet = preparedStatement.executeQuery();
        if (resultSet.next()) {
            System.out.println("查询成功");
        } else {
            System.out.println("查询失败");
        }
        connection.close();
        resultSet.close();
        statement.close();
    }
}
```

# 6. Jdbc API

![](https://pic.imgdb.cn/item/60c848b1844ef46bb2f7e5a0.jpg)

# 7. JDBCUtils

编写一个工具类

```java
package com.zhuang.jdbc;


import java.io.FileInputStream;
import java.sql.*;
import java.util.Properties;

/**
 * @Classname JDBCUtils
 * @Description JDBCUtils工具类
 * @Date 2021/6/16 20:48
 * @Created by dell
 */

public class JDBCUtils {
    /**
     * 定义4个相关属性
     */
    private static String user;
    private static String password;
    private static String url;
    private static String driver;

    static {
        try {
            Properties properties = new Properties();
            properties.load(new FileInputStream("src\\jdbc.properties"));
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            url = properties.getProperty("url");
            driver = properties.getProperty("driver");
        } catch (Exception e) {
            // 将编译异常转成运行异常
            // 可以选择捕获异常或者选择默认处理异常
            throw new RuntimeException(e);
        }
    }

    // 连接数据库 返回连接
    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(url, user, password);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    //关闭相关资源
        /*
        1. ResultSet 结果集
        2. Statement 或者 PreparedStatement
        3. Connection
        4. 如果需要关闭资源，就传入对象，否则传入 null
        */
    public static void close(ResultSet set, Statement statement, Connection connection) {
        //判断是否为 null
        try {
            if (set != null) {
                set.close();
            }
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        } catch (Exception e) {
            //将编译异常转成运行异常抛出
            throw new RuntimeException(e);
        }
    }
}
```

**使用下工具类进行查询，修改，增加**

```java
package com.zhuang.jdbc;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * @Classname JDBCUtils_Use
 * @Description JDBCUtils_Use演示工具类的使用
 * @Date 2021/6/16 20:57
 * @Created by dell
 */

public class JDBCUtils_Use {
    public static void main(String[] args) {
        //   testSelect();
        testDML();
    }


    public static void testSelect() {
        //1. 得到连接
        Connection connection = null;
        //2. 组织一个 sql
        String sql = "select * from user where id = ?";
        PreparedStatement preparedStatement = null;
        ResultSet set = null;
        //3. 创建 PreparedStatement 对象
        try {
            connection = JDBCUtils.getConnection();
            //com.mysql.jdbc.JDBC4Connection
            System.out.println(connection.getClass());
            preparedStatement = connection.prepareStatement(sql);
            //给?号赋值
            preparedStatement.setInt(1, 2);
            //执行, 得到结果集
            set = preparedStatement.executeQuery();
            //遍历该结果集
            while (set.next()) {
                int id = set.getInt("id");
                String name = set.getString("name");
                String pwd = set.getString("pwd");
                System.out.println(id + "\t" + name + "\t" + pwd);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            //关闭资源
            JDBCUtils.close(set, preparedStatement, connection);
        }
    }

    private static void testDML() {
        Connection connection = null;
        String sql = "update user set name=? where id=?";
        PreparedStatement preparedStatement = null;

        try {
            //获取连接
            connection = JDBCUtils.getConnection();
            //创建preparedStatement对象
            preparedStatement = connection.prepareStatement(sql);
            //赋值
            preparedStatement.setString(1, "周星驰");
            preparedStatement.setString(2, "10");
            //执行
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.close(null, preparedStatement, connection);
        }
    }
}
```

# 8. 事务

## 8.1 基本介绍

1. JDBC程序中当一个`Connection`对象创建时，默认情况下是自动提交事务，每次执行一个SQL语句，如果执行成功，就会向数据库自动提交，而不能回滚
2. JDBC程序中为了让多个SQL语句作为一个整体，需要**使用事务**
3. 调用`Connection`的`setAutoCommit(false)`可以取消自动提交事务
4. 在所有的SQL语句都成功执行后，调用`Connection`的`commit()`，方法提交事务
5. 在其中某个操作失败后，调用`Connection`的`rollback()`，方法回滚事务

# 9. 数据库连接池

1. 传统的JDBC数据库连接使用 DriverManager来获取，每次向数据库建立连接的时候都要将 Connection加载到内存中，再验证IP地址，用户名和密码(0.05s ~1s时间)。需要数据库连接的时候，就向数据库要求一个，频繁的进行数据库连接操作将占用很多的系统资源，容易造成服务器崩溃。

2. 每一次数据库连接，使用完后都得断开,如果程序出现异常而未能关闭，将导致数据库内存泄漏，最终将导致重启数据库。
3. 传统获取连接的方式,不能控制创建的连接数量，如连接过多，也可能导致内存泄漏，MySQL崩溃。
4. 解决传统开发中的数据库连接问题，可以采用数据库连接池技术(connection pool)。

## 9.1 C3P0连接池

使用`jdbc.properties`和`c3p0-config.xml`测试连接

```xml
<c3p0-config>
    <!-- 数据源名称代表连接池 -->
  <named-config name="mybatis">
<!-- 驱动类 -->
  <property name="driverClass">com.mysql.jdbc.Driver</property>
  <!-- url-->
   <property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/mybatis</property>
  <!-- 用户名 -->
      <property name="user">root</property>
      <!-- 密码 -->
   <property name="password">root</property>
   <!-- 每次增长的连接数-->
    <property name="acquireIncrement">5</property>
    <!-- 初始的连接数 -->
    <property name="initialPoolSize">10</property>
    <!-- 最小连接数 -->
    <property name="minPoolSize">5</property>
   <!-- 最大连接数 -->
    <property name="maxPoolSize">50</property>

   <!-- 可连接的最多的命令对象数 -->
    <property name="maxStatements">5</property> 
    
    <!-- 每个连接对象可连接的最多的命令对象数 -->
    <property name="maxStatementsPerConnection">2</property>
  </named-config>
</c3p0-config>
```

```java
package com.zhuang.jdbc;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import java.io.FileInputStream;
import java.sql.Connection;
import java.util.Properties;

/**
 * @Classname C3P0_
 * @Description C3P0连接池
 * @Date 2021/6/17 20:50
 * @Created by dell
 */

public class C3P0_ {
    public static void main(String[] args) throws Exception {
        testC3P0_01();
        testC3P0_02();
    }

    private static void testC3P0_01() throws Exception {
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
        // 通过配置文件获取相关连接信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\jdbc.properties"));
        //读取相关的属性值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String driver = properties.getProperty("driver");

        //给数据源 comboPooledDataSource 设置相关的参数
        comboPooledDataSource.setDriverClass(driver);
        comboPooledDataSource.setJdbcUrl(url);
        comboPooledDataSource.setUser(user);
        comboPooledDataSource.setPassword(password);

        //设置初始化连接数
        comboPooledDataSource.setInitialPoolSize(10);
        //最大连接数
        comboPooledDataSource.setMaxPoolSize(50);
        //测试连接池的效率, 测试对 mysql 5000 次操作
        long start = System.currentTimeMillis();
        for (int i = 0; i < 5000; i++) {
            Connection connection = comboPooledDataSource.getConnection();
            //System.out.println("连接 OK");
            connection.close();
        }
        long end = System.currentTimeMillis();
        //c3p0 5000 连接 mysql 耗时=391
        System.out.println("c3p0 5000 连接 mysql 耗时=" + (end - start));

    }

    private static void testC3P0_02() throws Exception {
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource("mybatis");
        //测试 5000 次连接 mysql
        long start = System.currentTimeMillis();
        System.out.println("开始执行....");
        for (int i = 0; i < 500000; i++) {
            Connection connection = comboPooledDataSource.getConnection();
            connection.close();
        }
        long end = System.currentTimeMillis();
        System.out.println("c3p0 的第二种方式(500000) 耗时=" + (end - start));
    }
}
```

## 9.2 Druid德鲁伊连接池

```java
package com.zhuang.jdbc;

import com.alibaba.druid.pool.DruidDataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

/**
 * @Classname Druid_
 * @Description Druid连接池
 * @Date 2021/6/17 20:59
 * @Created by dell
 */

public class Druid_ {
    public static void main(String[] args) throws Exception {
        testDruid();
    }

    private static void testDruid() throws Exception {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl("jdbc:mysql://127.0.0.1:3306/mybatis");
        dataSource.setUsername("root");
        dataSource.setPassword("root");

        Connection connection = dataSource.getConnection();
        String sql = "select * from user";
        PreparedStatement prepareStatement = connection.prepareStatement(sql);
        ResultSet resultSet = prepareStatement.executeQuery();
        while (resultSet.next()) {
            Object id = resultSet.getObject(1);
            Object username = resultSet.getObject(2);
            Object password = resultSet.getObject(3);
            System.out.println(id + ":" + username + ":" + password);
        }
        resultSet.close();
        connection.close();
        dataSource.close();
    }
}
```



**写在最后**

- 熟练运用JDBC和Mysql数据库对以后进阶打好基础:muscle:

