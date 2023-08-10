# 连接MySQL

初始化

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBConnection {
    private static final String URL = "jdbc:mysql://localhost:3306/your_database";
    private static final String USERNAME = "your_username";
    private static final String PASSWORD = "your_password";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USERNAME, PASSWORD);
    }

    public static void closeConnection(Connection con) {
        if (con != null) {
            try {
                con.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

查询

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class QueryExample {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        try {
            // 获取数据库连接
            conn = DBConnection.getConnection();

            // 创建 Statement 对象
            stmt = conn.createStatement();

            // 执行查询
            String sql = "SELECT * FROM your_table";
            rs = stmt.executeQuery(sql);

            // 处理查询结果
            while (rs.next()) {
                // 从结果集中获取数据
                int id = rs.getInt("id");
                String name = rs.getString("name");
                // 其他字段...

                // 在这里处理数据...
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 关闭资源
            try {
                if (rs != null) {
                    rs.close();
                }
                if (stmt != null) {
                    stmt.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }

            // 关闭数据库连接
            DBConnection.closeConnection(conn);
        }
    }
}
```
