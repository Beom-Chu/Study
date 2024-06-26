# [Java] Record

Record는 boilerplate code를 줄이기 위해 사용하는 클래스 선언 방법이다.

Java 16부터 정식 스펙으로 도입되었다.

### 구조

```java
public record TestRecord(String name, int age) {}
```

Record는 일반적인 클래스와 다르게 선언과 함께 필드를 나열하여 선언한다.

### 선언만으로 제공되는 기능

Inttellij에서 Record를 일반 클래스로 변환하는 기능을 제공 하는데, 이를 사용해 변경해보면 아래와 같다.

```java
public final class TestRecord {
    private final String name;
    private final int age;

    public TestRecord(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String name() {
        return name;
    }

    public int age() {
        return age;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj == this) return true;
        if (obj == null || obj.getClass() != this.getClass()) return false;
        var that = (TestRecord) obj;
        return Objects.equals(this.name, that.name) &&
                this.age == that.age;
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "TestRecord[" +
                "name=" + name + ", " +
                "age=" + age + ']';
    }
}
```

변화된 클래스 코드를 통해 Record가 제공해주는 기능을 보면 다음과 같다.

* 상속 불가능 처리(final class)

* 필드 private final 처리 및 Setter 미구현으로 불변성 제공

* Getter 구현

* equals 구현

* hasCode 구현

* toString 구현




