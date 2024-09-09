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

## Supplier

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

`Supplier`는 아무런 인자를 받지 않고 T 타입의 객체를 리턴한다.

람다식으로는 `() -> T` 로 표현한다.

공급자라는 이름처럼 아무것도 받지 않고 특정 객체를 리턴한다.

#### 사용 예

```java
    Optional<String> optional = Optional.ofNullable(null);
    String result = optional.orElseGet(() -> "No Data");
    System.out.println(result);
```

`Optional` 타입의 인스턴스를 만들어서 가지고 있는 값이 null인 경우 `No Data` 라는 `String`으로, `Null`이 아닌 경우 해당 값을 출력 하는 코드이다. 이때 `orElseGet()` 메소드에서 `Supplier` 인터페이스를 사용했다.

## Function

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

`Function`은 T 타입 인자를 받아서 R 타입을 리턴한다.

람다식으로는 `T -> R` 로 표현한다.

수학식에서의 함수처럼 특정 값을 받아서 다른 값으로 반환해준다.

T와 R은 같은 타입을 사용할 수도 있다.

#### 사용 예

```java
    List<Integer> list = List.of(1, 2, 3, 4, 5);
    list.stream()
            .map(o -> o * 2)
            .forEach(System.out::println);
```

1부터 5까지의 숫자 리스트를 각각 2배로 만들어서 순차적으로 출력하는 코드이다. 이때 `map` 메소드에서 `Function` 인터페이스를 사용한다.

## Comparator

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

`Comparator` 타입은 T 타입 인자 두개를 받아서 `int` 타입을 리턴한다.

람다식으로는 `(T, T) -> int` 로 표현한다. 

#### 사용 예

```java
    List<Integer> list = List.of(1, 2, 3, 4, 5);
    list.stream()
            .sorted((i1, i2) -> i2.compareTo(i1))
            .forEach(System.out::println);
```

1부터 5까지의 숫자 리스트를 크기가 큰 순서대로 정렬 후 출력하는 코드이다. 이 때 `sorted` 메소드에서 `Comparator` 인터페이스를 사용 한다.

## Runnable

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

`Runnable`은 아무런 객체를 받지 않고 리턴도 하지 않는다.

람다식으로는 `() -> void`로 표현한다.

`Runnable` 이라는 이름에 맞게 실행 가능한 이라는 뜻을 나타내며 이름 그대로 실행만 할 수 있다고 생각하면 된다.

#### 사용 예

```java
    Runnable runnable = () -> System.out.println("run runnable!");
    runnable.run();
```

위 코드처럼 람다식으로 텍스트를 출력하는 코드를 작성할 수 있다.

## Callable

```java
@FunctionalInterface
public interface Callable<T> {
    T call() throws Exception;
}
```

`Callable`은 인자를 받지 않으며, 특정 타입의 객체를 반환한다. 또한 `call()` 메소드 수행 중 `Exception`을 발생시킬 수 있다.

람다식은 `() -> T` 로 표현한다.

#### 사용 예

```java
    Callable<DateTime> callable = () -> DateTime.now();
    try {
        DateTime call = callable.call();
        System.out.println(call);
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
```

위 코드처럼 람다식으로 현재 날짜를 가져오는 코드를 작성해서 `call()` 메소드로 반환 받아보았다. `call()` 메소드는 `Exception`을 발생시킬 수 있으므로 `try-catch` 구문을 활용해주었다.
