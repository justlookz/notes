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
Connection conn;
Statement stmt;
try {
    conn = DriverManager.getConnection("db_url", "user", "password");
    stmt = conn.getStatement();
} catch (SQLException se) {
    System.err.println(se.toString());
}
```

Για να κάνουμε ένα αίτημα χρησιμοποιούμε

```java
stmt.executeQuery("sql_query here");
```

για αλλαγες στην βάση έχουμε το

```java
stmt.executeUpdate("sql_query");
```

Γίνονται και με το prepareStatement

```java
PrepareStatement pstm = conn.prepareStatement("queries with ?");
pstm.setString(1, "value"); // if string
pstm.setInt(2, an_int); // if int
pstm.setDouble(3, a_double);
int count = pstm.executeUpdate(); // return number of changes
// or
ResultSet rs = pstm.executeQuery(); // Return table rows
// etc
```


τέλος κλείνουμε την σύνδεση με

```java
conn.close();
```

## Example code
Ένα ολοκληρωμένο παράδειγμα είναι το εξής

```java
class ExampleJDBC {

    // Μεταβλητές

    Connection con;
    Statement stmt;
    ResultSet ra;
    String db_url = "jdbc.mysql://localhost/db_name";
    String user = "User";
    String password = "password";

    // Δημιουργία σύνδεσης με την βιβλιοθήκη
    Class.forName("com.mysql.jdbc.Driver");

    try {
        // Δημιουργία σύνδεσης με την βάση
        Connection con = DriverManager.getConnection(db_url, user, password);

        stmt = conn.getStatement();


        // Δημιουργία του πίνακα people
        int count = stmt.executeUpdate(
            "create table people ("
            + "id int, name varchar(16), age int "
            + "primary key (id);"
        );

        // Εισαγωγή δεδομένων
        int count2 = stmt.executeUpdate(
            "insert into person (id, name, age)"
            + "values ("
            + "0, George, 19);"
        )

        // και με PreparedStatement
        PrepareStatement pstm = conn.prepareStatement(
            "insert into person (id, name, age) values (?, ?, ?)"
        );

        pstm.setInt(1, 1); // id
        pstm.setString(2, "Mary"); // name
        pstm.setInt(3, 21); // age

        pstm.executeUpdate();


        // Ανάγνωση δεδομένων

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
        // Εαν δεν υπάρχει σύνδεση, τότε είναι null
        if (rs != null) rs.close();
        if (con != null) conn.close();
    }
}
```
