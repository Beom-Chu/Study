# [Java] Long 타입과 비교 연산

Long 타입의 경우 원시값이 아닌데 `==` 로 비교시 참으로 반환되는 경우가 있다.

```java
Long l1 = 1L;
Long l2 = 1L;
System.out.println(l1 == l2); // true
```

Java에서 Long 사용 시 -128 부터 127까지의 숫자는 상수 풀을 유지한다. 

그러므로 `==`  연산자로 비교시 정상적으로 동작 할 수 있다. 

하지만 위 범위보다 더 큰 값을 사용시 정상적으로 동작하지 않기 때문에 `equals` 를 사용해야 한다.

```java
Long l1 = 1L;
Long l2 = 1L;
System.out.println(l1.equals(l2)); // true

Long l3 = 128L;
Long l4 = 128L;
System.out.println(l3 == l4); // false
System.out.println(l3.equals(l4)); // true
```

<br>

그렇다면 원시 타입 long과 참조 타입 Long 을 비교하는 경우는 어떻게 될까?

```java
Long l5 = 128L;
long l6 = 128L;
System.out.println(l5 == l6); // true
System.out.println(l5.equals(l6)); // true
```

이 경우 묵시적 형변환 후 비교를 하게 되어 정상적인 결과를 얻을 수 있다.
