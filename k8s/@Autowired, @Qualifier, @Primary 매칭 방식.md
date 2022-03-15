# @Autowired, @Qualifier, @Primary 매칭 방식



### @Autowired

```java
@Autowired
private DiscountPolicy rateDiscountPolicy
```

1. 타입 매칭
2. 타입 매칭 결과가 여러개 인 경우 필드명, 파라미터 명으로 빈 이름 매칭

<br>

### @Qualifier

추가 구분자를 붙여주는 방법

##### 빈 등록

```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {}
```
```java
@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {}
```

##### 생성자 자동 주입

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository,  @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```

##### 수정자 자동 주입

```java
@Autowired
public DiscountPolicy setDiscountPolicy(@Qualifier("mainDiscountPolicy")
DiscountPolicy discountPolicy) {
    return discountPolicy;
}

```

1. @Qualifier끼리 매칭
2. 빈 이름 매칭
3. `NoSuchBeanDefinitionException` 예외 발생

<br>

### @Primary

우선순위를 정하는 방법

`@Autowired` 사용시 여러 빈이 매칭되면 `@Primary`가 우선

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {}

@Component
public class FixDiscountPolicy implements DiscountPolicy {}
```

##### 사용 코드

```java
//생성자
@Autowired
public OrderServiceImpl(MemberRepository memberRepository,
 DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
//수정자
@Autowired
public DiscountPolicy setDiscountPolicy(DiscountPolicy discountPolicy) {
    return discountPolicy;
}
```

<br>

#### @Primary VS @Qualifier

둘 중 우선순위는 `@Qualifier` 가 높음
