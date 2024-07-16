# [Java] Comparator 구현

자바의 Collection이나 자료구조는 정렬할 수 있는 Sort() 같은 메서드들을 가지고 있다.

이런 메서드는 자료를 비교하기 위해 함수형 인터페이스인 Comparator를 요구하곤  한다.

```java
// Arrays의 sort()
static <T> void srot(T[] a, Comparator<? super T> c)
// List 의 sort()
default void sort(Comparator<? super E> c)
// Stream의 sorted
Stream<T> sorted(Comparator<? super T> comparator)
```

특히 객체간 비교를 하는 경우, 객체간 비교 방법을 Comparator로 명시해주어야 한다.

## 기본 구현

```java
public static class User {
  public String name;
  public int age;

  public String getName() {
    return name;
  }
  public int getAge() {
    return age;  
  }
}
```

위와 같은 `User` 클래스가 있다고 가정했을 때, 해당 객체를 인스턴스화 해서 `List`에 담은 상태에서 나이순으로 정렬을 하기 위해선 아래와 같이 할 수 있다.

```java
users.sort((user1, user2) -> user1.age > user2.age ? -1 : 1);
```

Comparator를 사용하는 정렬 메서드는 **Comparator가 음수를 반환하면 비교된 두 수를 바꾸지 않는다.**

위 코드에서 삼항연산자를 사용하지 않는 방법으로 코딩을 하면 아래와 같이 할수 있다.

```java
users.sort((u1, u2) -> u1.age - u2.age);
```

## Comparator 메서드

Comparator를 사용하면 좀 더 깔끔한 코딩이 가능하다.

```java
users.sort(Comparator.comparing(User::getAge));
```

역순으로 정렬하려면 아래와 같이 할 수 있다.

```java
users.sort(Comparator.comparing(User::getAge).reversed());
```

비교 대상이 기본형이라면 Comparator가 제공하는 static  메서드를 활용 가능하다.

```java
collect.sort(Comparator.naturalOrder()); // 오름차순
collect.sort(Comparator.reverseOrder()); // 내림차순
```

비교하려는 기준이 여러개인 경우 `thenComparing` 을 사용하면 된다.

```java
users.sort(Comparator.comparing(User::getName).thenComparing(User::getAge));
```
