---
title: 理解sql注入
tags:
  - sql
---

# MySQL

我们首先在数据库中建立一张 job 表，存放职位信息。

```sql
mysql> create table jobs(
    -> id int not null primary key,
    -> job_desc char(20),
    -> level int );
    Query OK, 0 rows affected (0.03 sec)
```

插入几条记录后查询


```
mysql> select * from jobs;
+----+------------------+-------+
| id | job_desc         | level |
+----+------------------+-------+
|  1 | manager          |    10 |
|  2 | product manager  |     9 |
|  3 | product designer |     8 |
+----+------------------+-------+
3 rows in set (0.00 sec)
```

现在我们根据 id 来查询工作信息

```
mysql> select * from jobs where id = 1;
+----+----------+-------+
| id | job_desc | level |
+----+----------+-------+
|  1 | manager  |    10 |
+----+----------+-------+
1 row in set (0.00 sec)
```

加入我们要获取 job 表的所有数据并且保留 where 语句，那我们只要使得 where 语句恒真就行了，如下

```
mysql> select * from jobs where id = 1 or 1 = 1;
+----+------------------+-------+
| id | job_desc         | level |
+----+------------------+-------+
|  1 | manager          |    10 |
|  2 | product manager  |     9 |
|  3 | product designer |     8 |
+----+------------------+-------+
3 rows in set (0.00 sec)
```

# 使用 statement 进行数据操作

## code

```java
import org.apache.commons.dbutils.DbUtils;
import java.sql.*;

public class sql_injection {
    private static String driverClassName = "com.mysql.jdbc.Driver";
    private static String url = "jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8";
    private static String user = "root";
    private static String password = "root";

    private static Connection Connect(){
        Connection conn = null;
        //load driver
        try {
            Class.forName(driverClassName);
            System.out.println("load driver...");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
//    connect db
        try {
            conn = DriverManager.getConnection(url,user,password);
            System.out.println("get connection...");
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return conn;
    }
    public static void main(String args[]){
        Connection conn = null;
        try {
            conn = Connect();
            Statement stmt = conn.createStatement();
            String sql = "select * from jobs where id = "+"1";
            ResultSet  resultSet= stmt.executeQuery(sql);
            System.out.println("resultSet:"+resultSet);
            ResultSetMetaData metaData = resultSet.getMetaData();
            int columns = metaData.getColumnCount();
            for (int i=1;i<=columns;i++){
                System.out.print(metaData.getColumnName(i));
                System.out.print("\t\t\t");
            }
            System.out.println();
            while (resultSet.next()){
                for (int i = 1;i<=columns;i++){
                    System.out.print(resultSet.getString(i));
                    System.out.print("\t\t\t");
                }
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}

```

执行后得到的结果为：

select \* from jobs where id = ?
resultSet:com.mysql.jdbc.JDBC4ResultSet@288051
id job_desc level
1 manager 10

如果将 where 语句改为：

```
sql = "select * from jobs where id = "+"1 or 1 = 1";
```

观察执行后的结果

select \* from jobs where id = ?
id job_desc level
1 manager 10
2 product manager 9
3 product designer 8

得到了数据库中的所有数据，所以 statements 不能防止 sql 注入攻击。

# 使用 PreparedStatement

## code

```Java
String sql = "select * from jobs where id = ?";
System.out.println(sql);
PreparedStatement preparedStatement = conn.prepareStatement(sql);
preparedStatement.setString(1, "1 or 1 = 1");
ResultSet resultSet = preparedStatement.executeQuery();
```

观察执行结果

    select * from jobs where id = ?
    id			job_desc			level
    1			manager				10

只取到了一条数据
所以 PreparedStatement 安全性更高,可以防止 sql 注入攻击
