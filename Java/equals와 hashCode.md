

# equals와 hashCode

equals와 hashCode는 모든 java객체의 부모 객체인 Object 클래스에 정의 되어 있습니다.

그러므로 모든 java 객체는 equals와 hashCode 함수를 상속 받고 있습니다.

### equals란

기본적으로 2개의 객체가 동일한지 검사하기 위해 사용됩니다.

equals가 구현된 방법은 **2개의 객체가 참조하는 것이 동일한지 확인**하는 것이며, 이는 동일성을 비교하는 것입니다.

즉, 2개의 객체가 가리키는 곳이 동일한 메모리 주소일 경우에만 동일한 객체가 됩니다.

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

하지만 프로그래밍을 하다보면 동일한 객체가 메모리 상에 여러 개 띄워져 있는 경우가 있습니다. 해당 객체는 서로 다른 메모리에 띄워져 있으므로 동일한 객체가 아닙니다.

하지만 프로그래밍 상으로는 같은 값을 지니므로 같은 객체로 인식되어야 하는데, 이를 위해 우리는 값으로 객체를 비교하도록 equals 메소드를 오버라이딩 해주는 것 입니다.

```java
String s1 = new String("Test");
String s2 = new String("Test");

System.out.println(s1 == s2);			// false
System.out.println(s1.equals(s2));		// true;
```

위와 같이 동일한 값을 갖는 문자열 2개를 생성하면 각각 다른 메모리에 할당되므로 동일하지 않습니다. 대신 같은 값을 지니고 있습니다. 

equals 메소드를 호출해보면 true가 나오는데, 그 이유는 String 클래스에서 equals 메소드를 오버라이드하여 객체가 같은 값을 갖는지 비교하도록 처리했기 때문입니다.

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    
    return false;
}
```

<br>

### hashCode란

**실행 중에(Runtime) 객체의 유일한 integer값을 반환**합니다. Object 클래스에서는 **heap에 저장된 객체의 메모리 주소를 반환**하도록 되어 있습니다.

<br>

### equals와 hashCode의 관계

동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 합니다.

그렇기 때문에 equals 메소드를 오버라이드 한다면, hashCode 메소드도 함께 오버라이드 되어야 합니다.

* Java 프로그램을 실행하는 동안 equals에 사용된 정보가 수정되지 않았다면, hashCode는 항상 동일한 정수값을 반환해야 합니다.
* 두 객체가 equals에 의해 동일하다면, 두 객체의 hashCode값도 일치해야 합니다.
* 두 객체가 equals에 의해 동일하지 않다면, 두 객체의 hashCode 값을 일치하지 않아도 됩니다.

즉, obj1.equals(obj2) == True 이면 hashCode(obj1) == hashCode(obj2) 이여야하지만,

hashCode(obj1) == hashCode(obj2) 라고 obj1.equals(obj2) == True일 필요는 없습니다.

하지만 우리는 다른 객체에 대해 동일한 hashCode를 생성한다면 hashTable을 생성하는데 불이익을 받을 수 있음을 인지하고 있어야 합니다.

<br>

## equals와 hashCode의 override

### equals override의 필요성

만약 아래와 같은 클래스가 있다고 했을 때, id는 고유값을 가집니다.

```java
public class Employee{
    private Integer id;
    private String firstname;
    private String lastName;
    private String department;
}
```

만약 아래와 같이 같은 id 값을 갖는 2개의 Employee를 서로 다른 처리 과정에 의해 얻었다고 합시다. 2개의 Employee는 같은 id를 갖기 때문에 equals 연산을 하면 true를 반환해야 합니다. 하지만 아래 예제에서는 false를 반환 합니다.

```java
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
 
        e1.setId(100);
        e2.setId(100);
 
        System.out.println(e1.equals(e2));  //false
    }
}
```

이러한 문제를 해결하기 위해 아래와 같이 equals 메소드를 오버라이드 해야합니다.

```java
public boolean equals(Object o) {
    if (o == null) {
        return false;
    }
    if (o == this) {
        return true;
    }
    if (getClass() != o.getClass()) {
        return false;
    }
     
    Employee e = (Employee) o;
    return (this.getId() == e.getId());
}
```

이렇게 equals에 의한 문제는 해결된 것 처럼 보이지만 Employee를 **HashSet 같은 자료구조에 저장하려고 하면 또 다른 문제가 발생**합니다.

### hashCode override의 필요성

```java
import java.util.HashSet;
import java.util.Set;
 
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
 
        e1.setId(100);
        e2.setId(100);
 
        //Prints 'true' now!
        System.out.println(e1.equals(e2));
 
        Set<Employee> employees = new HashSet<Employee>();
        employees.add(e1);
        employees.add(e2);
         
        System.out.println(employees);  //Prints two objects
    }
}
```

equals 메소드를 오버라이드 후 두 객체의 equals 결과는 true로 반환 됩니다.

 그러나 HashSet에 저장된 값을 보면 id가 같은 객체임에도 불구하고 2개의 객체로 저장된 것을 확인 할 수 있습니다.

Object 클래스의 hashCode는 해당 메모리 주소값을 반환 합니다. 그렇기 때문에 e1, e2는 다른 해시값을 반환할 것이고, HashSet에는 2개의 객체가 서로 다른 위치에 저장된 것입니다.

이런 문제를 해결하기 위해 hashCode 메소드도 Employee 클래스에 오버라이하여 수정해주어야 합니다.

```java
@Override
public int hashCode() {
    return Objects.hash(id);
}
```

> Objects.hash(Object... values) : 매개 값으로 주어진 값들을 이용해서 해시 코드를 생성하는 함수