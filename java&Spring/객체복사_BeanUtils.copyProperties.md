# 객체복사_BeanUtils.copyProperties

필드 개수가 많은 객체를 setter를 사용하는 대신에 사용.



### BeanUtils.copyProperties(source, target);

source : 원본 객체

target : 복사 대상 객체



```java
import org.springframework.beans.BeanUtils;

  public class Student {
    public static void main(String[] args) {
      StudentInfo st = new StudentInfo();
      st.setName("홍길동");
      st.setGrade(1);
      st.setNumber(2);
      StudentInfoTarget stTarget = new StudentInfoTarget();
      BeanUtils.copyProperties(st, stTarget); // StudentInfoTarget(name=홍길동, grade=2, number=15)
    }
  }

  class StudentInfo {
    private String name;
    private int grade;
    private int number;

    public String getName() {
      return name;
    }
    public void setName(String name) {
      this.name = name;
    }
    public int getGrade() {
      return grade;
    }
    public void setGrade(int grade) {
      this.grade = grade;
    }
    public int getNumber() {
      return number;
    }
    public void setNumber(int number) {
      this.number = number;
    }
  }

  class StudentInfoTarget {
    private String name;
    private int grade;
    private int number;

    public String getName() {
      return name;
    }
    public void setName(String name) {
      this.name = name;
    }
    public int getGrade() {
      return grade;
    }
    public void setGrade(int grade) {
      this.grade = grade;
    }
    public int getNumber() {
      return number;
    }
    public void setNumber(int number) {
      this.number = number;
    }
  }
```

 <br>

### BeanUtils.copyProperties(source, target, String ... ignoreProperites);

source : 원본 객체

target : 복사 대상 객체

ignoreProperities : 복사를 원하지 않는 프로퍼티명

```java
    StudentInfo st = new StudentInfo();
    st.setName("홍길동");
    st.setGrade(1);
    st.setNumber(2);
    StudentInfoTarget stTarget = new StudentInfoTarget();
    BeanUtils.copyProperties(st, stTarget, "grade","number"); //StudentInfoTarget(name=홍길동, grade=0, number=0)
```

<br><br><br><br>







출처: https://junghn.tistory.com/entry/SPRING-BeanUtilscopyProperties을-이용하여-Class간의-property-복사하기 [코딩 시그널]
