# [java] Comparable vs Comparator

Java에서 `Comparable` 인터페이스와 `Comparator` 인터페이스는 둘 다 객체들 간의 순서 비교를 가능하게 하는 인터페이스입니다.

## 차이점

1. `Comparable`은 객체에 대한 기본 정렬 기준을 제공하는 인터페이스이고, `Comparator`는 객체 간의 비교를 위해 별도의 클래스를 구현하는 인터페이스입니다.
2. `Comparable`은 `java.lang` 패키지에 정의된 인터페이스이며, 객체를 구현한 클래스에서 `Comparable` 인터페이스를 구현합니다. `Comparator`는 `java.util` 패키지에 정의되어 있으며, 객체 비교를 위한 별도의 클래스를 작성하고 `Comparator` 인터페이스를 구현합니다.
3. `Comparable`은 `compareTo()` 메서드를 정의하고, 이를 사용하여 객체의 순서를 결정합니다. `Comparator`는 `compare()` 메서드를 정의하고, 이를 사용하여 두 객체 간의 순서를 결정합니다.
4. `Comparable`은 객체 간의 기본 순서를 정의하므로, 객체가 이미 정렬 가능한 경우 유용합니다. `Comparator`는 객체의 여러 가지 순서 기준을 지정할 수 있으므로, 정렬 순서를 변경하거나 여러 가지 방식으로 정렬할 수 있는 유연성을 제공합니다.

따라서, `Comparable`은 객체의 기본 순서를 지정하고, `Comparator`는 객체 간의 비교를 위한 다양한 정렬 기준을 제공합니다.

## 샘플

### Comparable

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Student implements Comparable<Student> {
    private int id;
    private String name;
    private int age;

    public Student(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    // Comparable을 구현하여 기본 정렬 기준을 지정합니다.
    @Override
    public int compareTo(Student student) {
        return this.id - student.id;
    }

    // Getter와 Setter 생략

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class Main {
    public static void main(String[] args) {
        List<Student> studentList = new ArrayList<>();

        studentList.add(new Student(4, "John", 21));
        studentList.add(new Student(2, "Sarah", 19));
        studentList.add(new Student(1, "Mike", 20));
        studentList.add(new Student(3, "Emily", 18));

        // Comparable을 이용하여 기본 정렬 기준으로 정렬합니다.
        Collections.sort(studentList);

        System.out.println("기본 정렬 기준으로 정렬된 학생들: " + studentList);
    }
}
```

`Comparable` 인터페이스를 구현한 후 `Collections.sort()` 메서드를 호출하면 기본 정렬 기준으로 객체를 정렬할 수 있습니다.

### Comparator 

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class Student {
    private int id;
    private String name;
    private int age;

    public Student(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    // Getter와 Setter 생략

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class Main {
    public static void main(String[] args) {
        List<Student> studentList = new ArrayList<>();

        studentList.add(new Student(4, "John", 21));
        studentList.add(new Student(2, "Sarah", 19));
        studentList.add(new Student(1, "Mike", 20));
        studentList.add(new Student(3, "Emily", 18));

        // Comparator를 이용하여 나이순으로 정렬합니다.
        Comparator<Student> sortByAge = Comparator.comparingInt(Student::getAge);
        Collections.sort(studentList, sortByAge);

        System.out.println("나이순으로 정렬된 학생들: " + studentList);
    }
}
```

