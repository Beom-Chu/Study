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

### 일반 Class와 다른 equals() 동작 방식

일반 Class는 기본적으로 객체의 주소값을 비교하는 방식으로 구현되어 있지만 Record는 이를 Override하여 필드의 값을 모두 비교하도록 구현되어 있다.

```java
TestClass class1 = new TestClass("name", 1);
TestClass class2 = new TestClass("name", 1);
System.out.println(class1.equals(class2));        // false

TestRecord record1 = new TestRecord("name", 1);
TestRecord record2 = new TestRecord("name", 1);
System.out.println(record1.equals(record2));    // true
```

### Lombok과 다른 Getter 메소드명

Lombok은 JavaBeans API specification에서 제공하는 bean-standard 표준을 따르는 `get` 접두사를 사용하는 네이밍을 기본으로 사용하고 있다.

그러나 Record는 Getter가 필드명과 동일하게 생성된다.

```java
TestRecord record = new TestRecord("name", 1);
System.out.println(record.age());
System.out.println(record.name());
```

### 컴팩트 생성자(Compact Constructor)

Record의 표준 생성자를 호출하면 자동으로 함께 실행되는 코드를 작성할 수 있는 기능이다.

표준 생성자와 달리 컴팩트 생성자 내부에서는 인스턴스 필드에 접근할 수 없다는 특징이 있다.

아래와 같이 validation 용도로 사용 할 수 있다.

```java
public record TestRecord(String name, int age) {
    public TestRecord {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
}
```

### 리플렉션으로 조작 불가능

Java 17부터는 언어 차원에서 Reflection을 사용한 필드 조작을 방지하고 있다. 이는 Record의 불변성을 더욱 강화시켜주는 특징이다.

```java
TestClass clss = new TestClass("name", 1);
Class<TestClass> cls = (Class<TestClass>) Class.forName("org.example.TestClass");
Field clsName = cls.getDeclaredField("name");
clsName.setAccessible(true);
clsName.set(clss, "new name");

System.out.println(clss.name());	// new name
TestRecord record = new TestRecord("name", 1);
Class<TestRecord> recordClass = (Class<TestRecord>) Class.forName("org.example.TestRecord");
Field recordName = recordClass.getDeclaredField("name");
recordName.setAccessible(true);
recordName.set(record, "new name");	// IllegalAccessException 예외 발생

System.out.println(record.name());
```

##### 완전한 불변성을 보장하기 위한 팁

이러한 특징에도 불구하고 불변성이 깨질 수 있는 케이스가 있다.

객체를 필드로 주입 받는 경우인데 대표적으로 Collection 타입의 객체를 주입 받는 경우가 있다.

```java
public record Parent(String name, int age, List<String> children) {}
```

```java
List<String> children = new ArrayList<>();
children.add("john");
Parent parent = new Parent("name", 1, children);

parent.children().add("jane");
System.out.println(parent.children());	// [john, jane]
```

이렇듯 객체의 참조를 받아와 조작하면 불변성이 깨질 수 있다.

이를 방지하기 위해 표준 생성자를 Override하고, 직접 Collection을 불변으로 처리하면 된다.

```java
public record Parent(String name, int age, List<String> children) {
    public Parent(String name, int age, List<String> children) {
        this.name = name;
        this.age = age;
        this.children = Collections.unmodifiableList(children);
    }
}
```


