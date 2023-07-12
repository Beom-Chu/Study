# 자원해제 방법(close(), try-with-resources활용)

JDBC API를 예로 들어보자.

 Connection, Statement, ResultSet 를 닫아줘야 한다.

## 1. close()를 사용하는 가장 이상적인 방법

```java
public class Class1 {
    public method1() throws Exception {
            
      Connection conn = null;
      PreparedStatement ps = null;
      ResultSet rs = null;
      
      try {
          // ...
          conn = ...;
          ps = ...;
          rs = ...;
          
          // ...
      } catch (Exception e) {
          // ...
      } finally {
          if (rs != null) try { rs.close(); } catch(Exception e) {}
          if (ps != null) try { ps.close(); } catch(Exception e) {}
          if (conn != null) try { conn.close(); } catch(Exception e) {}
      }
    }
}
```

<br>

## 2. Java7 버전의 try-with-resources 사용

try 블록의 소괄호 안에서 `close()` 메서드 호출이 필요한 객체를 할당해 준다.

```java
public class Class1 {
    public method1() throws Exception {
      try (Connection conn = DriverManager.getConnection("...");
            Statement stat = conn.createStatement();
            ResultSet rs = stat.executeQuery("SELECT 1 from dual")) {
          
          // ...
      } catch (Exception e) {
          // ...
      } 
    }
}
```

<br>

## 3. Java9 이상에서 향상된 try-with-resources 사용

Java9 부터는 `try-with-resources` 를 좀 더 유연하게 try 블록의 밖에서 선언된 객체를 잠조 할 수 있다.

```java
public class Class1 {
    public method1() throws Exception {
      Connection conn = DriverManager.getConnection("...");
      Statement stat = conn.createStatement();
      ResultSet rs = stat.executeQuery("SELECT 1 from dual")

      try (conn; stat; rs) {
          // ...
      } catch (Exception e) {
          // ...
      } 
    }
}
```

