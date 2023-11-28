# SpEL : Spring Expression Language

SpEL (Spring Expression Language) 은 런타임에서 객체에 대한 쿼리와 조작(querying and manipulating)을 지원하는 강력한 표현 언어입니다.

|타입|연산자|
|----|----|
|산술|`+`, `-`, `*`, `/`, `%`, `^`, `div`, `mod`|
|관계|`<`, `>`, `==`, `!=`, `<=`, `>=`, `lt`, `gt`, `eq`, `ne`, `le`, `ge`|
|논리|`and`, `or`, `not`, `&&`, `||`, `!`|
|조건|`?:`|
|정규식|`matches`|

</br>
### 연산자
SpEL 표현식은 # 기호로 시작하며 중괄호로 묶어서 표현한다. `#{표현식}`
속성 값을 참조할 때는 $ 기호와 중괄호로 묶어서 표현한다. `${property.name}`
아래와 같이 속성 값 참조는 표현식 안에서 사용이 가능하다.
```
#{${someProperty} + 2}
```
</br>
### 산술연산자
```java
@Value("#{19 + 1}") // 20
private double add;

@Value("#{2 ^ 10}") // 1024
private double powerOf;

@Value("#{(2 + 2) * 2 + 9}") // 17
private double brackets;
```

</br>
### 관계 및 논리 연산자
```java
@Value("#{1 == 1}") // true
private boolean equal;

@Value("#{!true}") // false
private boolean notTrue;
```

</br>
### 조건부(삼항) 연산자
```java
@Value("#{some.property != null ? some.perperty : 'default'}")
private String ternary;

@Value("#{some.property != null ?: 'default'}") // 위와 동일하게 null인 경우 default 주입
private String elvis;
```

</br>
### 정규식(Regex) 표현법
```java
@Value("#{'100' matches '\\d+'}") // true
private boolean validNumericStringResult;

@Value("#{'100asdf' matches '\\d+'}") // false
private boolean invalidNumericStringResult;
```

</br>
### List, Map 객체 참조
스프링 빈으로 생성된 객체의 List, Map 프로퍼티에 대해서도 표현식을 통해 참조가 가능하다.

```java
@Component("membersHolder")
public class MembersHolder {
    private List<String> members = new ArrayList<>();
    private Map<String, Integer> membersAge = new HashMap<>();

    public MembersHolder() {
        members.add("devwithpug");
        members.add("John");

        membersAge.put("devwithpug", 24);
        membersAge.put("John", 30);
    }

    // Getter & Setter 생략
}
```
위와 같이 임의의 스프링 빈 객체를 생성하면 간단하게 참조할 수 있다.

```java
@Value("#{membersHolder.members.size()}") // 2
private Integer numberOfMembers;

@Value("#{membersHolder.membersAge['devwithpug']}") // 24
private Integer age;
```