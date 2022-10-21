# Comparator 구현시 주의사항(IllegalArgumentException: Comparison method violates its general contract!)

```java
if (a.value() < b.value() ) {
  return -1;
} else {
  return 1;
}
```

Comparator 구현 시 양수와 음수만 반환 되도록 구현하면 `IllegalArgumentException: Comparison method violates its general contract!` 에러를 만나게 됩니다.

**항상 발생하는 건 아니고 a, b에 특정 값이 들어온 경우에 발생**합니다.

에러 발생하는 건 a와 b의 순서가 바뀌어서 비교되는 경우입니다.

예를 들어 compare(a,b)가 양수이면 compare(b,a)는 양수가 되면 안 됩니다.

그런데 위 코드에서는 같은 값이면 둘 다 양수가 되도록 됩니다.

이런 경우 정렬 알고리즘이 판단하기에 기준이 모호하기 때문에 에러가 발생합니다.



*비교 대상의 Row들 중에 같은 값이 있는데 정렬 알고리즘에 의해 처리되다가 한번 비교된 두 Row들의 순서가 바뀌어서 다시 비교가 되는 경우가 발생하면 에러가 발생하는 것으로 보입니다.*

<br>

## 결론

### 동일한 값일때 0을 반환해주는 부분 필수

```java
if(a.value() == b.value() ) {
  return 0;
}else if (a.value() < b.value() ) {
  return -1;
} else {
  return 1;
}
```

위 로직에서 같은 값일때 0을 반환 해주는 부분을 넣어주거나, compare 메서드를 활용하자.

```java
// 숫자 타입인 경우
return Integer.compare(a.value(), b.value());
```

```java
// 문자열 타입인 경우
return a.value().compareTo(b.value());
```

