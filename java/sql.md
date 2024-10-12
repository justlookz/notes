# JDBC 

JDBC (Java database connection) είναι βιβλιοθήκη στην java
για τλβα τρέχει sql ετοίματα η java

Εδώ θέλουμε τις εξής κλάσεις

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
// or
import java.sql.PrepareStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
```

Στην πορεία ενεργοποιούμε την βιβλιοθήκη για να τρέξει η σύνδεση
με την sql

```java
Class.forName("com.mysql.jdbc.Driver");
```

Συνδεόμαστε σε αυτήν

```java
Connection con;
Statement stmt;
try {
    con = DriverManager.getConnection("db_url", "user", "password");
    stmt = con.getStatement();
} catch (SQLException se) {
    System.err.println(se.toString());
}
```

για να κάνουμε ένα αίτημα χρησιμοποιούμε

```java
stmt.executeQuery("sql_qyery here");
```

τέλος κλείνουμε την σύνδεση με

```java
con.close();
```

```java
class ExampleJDBC {
    Connection con;
    Statement stmt;
    ResultSet ra;
    String db_url = "jdbc.mysql://localhost/db_name";
    String user = "User";
    String password = "password";

    try {
        Connection con = DriverManager.getConnection(db_url, user, password);

        stmt = con.getStatement();

        int count = stmt.executeUpdate(
            "create table people ("
            + "id int, name varchar(16), age int "
            + "primary key (id);"
        );

        int count2 = stmt.executeUpdate(
            "insert into person ("
            + "0, George, 19);"
        )

        ResultSet rs = stmt.executeQuery(
            "select name, age from person;"
        )

        while(rs.next()) {
            System.out.println(
                "Name: " + rs.getString(1)
                + " | Age: " + rs.getInt(2)
            );
        }
    } catch (SQLException se) {
        System.err.println(se.toString());
    } finally {
        if (rs != null) rs.close();
        if (con != null) con.close();
    }
}
```
