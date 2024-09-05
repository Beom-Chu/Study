# [Java] 함수형 인터페이스(Functional Interfaces)

## Java 기본 제공 함수형 인터페이스

| **함수형 인터페이스**  | Descripter      | Method                    |
| -------------- | --------------- | ------------------------- |
| Predicate      | `T -> boolean`  | `boolean test(T t)`       |
| Consumer       | `T -> void`     | `void accept(T t)`        |
| Supplier       | `() -> T`       | `T get()`                 |
| Function<T, R> | `T -> R`        | `R apply(T t)`            |
| Comparator     | `(T, T) -> int` | `int compare(T o1, T o2)` |
| Runnable       | `() -> void`    | `void run()`              |
| Callable       | `() -> T`       | `V call()`                |

## Predicate

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

`Predicate`는 인자 하나를 받아서 `boolean` 타입을 리턴한다.

람다식으로는 `T -> boolean` 으로 표현한다.

#### 사용 예

```java
List<Integer> list = List.of(1, 2, 3, 4, 5);
long count = list.stream()
            .filter(o -> o > 3)
            .count();
```

1부터 5까지의 숫자가 든 리스트에서 `stream`으로 3보다 큰 숫자들을 필터링 후 결과의 숫자를 구하는 코드 이다. 이 때 `filter` 메서드가 위와 같이 `Predicate` 를 사용한다.

## Consumer

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

`Consumer`는 인자 하나를 받고 아무것도 리턴하지 않는다.

람다식으로 `T -> void` 로 표현한다.

소비자라는 이름에 걸맞게 인자를 받아서 소비만 하고 끝낸다고 생각하면 된다.

#### 사용 예

```java
    List<Integer> list = List.of(1, 2, 3, 4, 5);
    list.forEach(o -> System.out.println(o));
```

1부터 5까지의 숫자가 든 리스트를 순서대로 출력하는 코드이다. 이때 `forEach` 메서드가 위와 같이 `Consumer`를 사용한다.

이를 메소드 참조 형식으로 변환하면 아래와 같이 더 보기 좋은 코드로 만들 수 있다.

```java
list.forEach(System.out::println);
```




