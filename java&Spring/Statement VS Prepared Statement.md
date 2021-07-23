# Statement vs Prepared Statement

자바에서 데이터베이스로 쿼리문을 전송할 때 사용할 수 있는 인터페이스

SQL 질의문을 전달하는 역할 수행



### Statement

* Statement 객체는 Statement 인터페이스를 구현한 객체를 Connection 클래스의 createStatement( ) 메소드를 호출함으로써 얻어진다.

* Statement 객체가 생성되면 executeQuery( ) 메소드를 호출하여 SQL문을 실행시킬 수 있다. 메소드의 인수로 SQL문을 담은 String객체를 전달한다.

* Statement는 정적인 쿼리문을 처리할 수 있다. 즉 쿼리문에 값이 미리 입력되어 있어야 한다.

```java
import java.sql.Statement;
import java.sql.Connection;
import java.sql.SQLException;
 
public class StatementTest 
{
    public static void main(String args[])
    {
        Connection conn = null; // DB연결된 상태(세션)을 담은 객체
        Statement stm = null;  // SQL 문을 나타내는 객체
        
        try {
            conn = DBConnection.getConnection();
            stm = conn.createStatement();
            
            String quary = "INSERT INTO TEST VALUES('id1', 'pw1', 'name1')";
            int success = stm.executeUpdate(quary);
            
            if(success > 0)
                System.out.println("데이터 입력 성공");
            else
                System.out.println("데이터 입력 실패");
 
        } catch (SQLException sqle) {
            sqle.printStackTrace();
        }
    }
}
```

---

### PreparedStatement

* PreparedStatement 객체는 Connection 객체의 preparedStatement( ) 메소드를 사용해서 생성한다. 이 메소드는 인수로 SQL문을 담은 String객체가 필요하다.

* SQL문장이 미리 컴파일되고, 실행 시간동안 인수값을 위한 공간을 확보할 수 있다는 점에서 Statement 객체와 다르다. 

* Statement 객체의 SQL은 실행될 때 매번 서버에서 분석해야 하는 반면, PreparedStatement 객체는 한 번 분석되면 재사용이 용이하다.

* 각각의 인수에 대해 위치홀더(placeholder)를 사용하여 SQL문장을 정의할 수 있게 해준다. 위치홀더는 ? 로 표현된다.

* 동일한 SQL문을 특정 값만 바꾸어서 여러 번 실행해야 할 때, 인수가 많아서 SQL문을 정리해야 될 필요가 있을 때 사용하면 유용하다.

```java
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.PreparedStatement;
 
public class PreparedStatementTest 
{
    public static void main(String args[])
    {
        Connection conn = null; // DB연결된 상태(세션)을 담은 객체
        PreparedStatement pstm = null;  // SQL 문을 나타내는 객체
        
        try {
            
            String quary = "INSERT INTO TEST VALUES(?, ?, ?)";
            
            conn = DBConnection.getConnection();
            pstm = conn.prepareStatement(quary);
            
            // 쿼리에 값을 세팅한다.
            // 여기서 1, 2, 3은 첫번째, 두번째, 세번째 위치홀더 라는 뜻
            pstm.setString(1, "id2");
            pstm.setString(2, "pw2");
            pstm.setString(3, "name2");
            
            int success = pstm.executeUpdate();
            
            if(success > 0)
                System.out.println("데이터 입력 성공");
            else
                System.out.println("데이터 입력 실패");
 
            
        } catch (SQLException sqle) {
            sqle.printStackTrace();
        }
    }
}
```

<br>

## PreparedStatement를 사용하는 것이 좋은 이유

* 동적인 쿼리문을 처리할 수 있으므로 같은 SQL문에서 값만 변경하여 사용한다던가 인수가 많은 경우에 사용하기 좋다. 

* 미리 컴파일되기 때문에 수행 속도가 Statement보다 빠른 장점이 있다.



<br>

<br>

<br>

출처: https://all-record.tistory.com/79 [세상의 모든 기록]