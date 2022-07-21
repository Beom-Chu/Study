# Lombok @RequiredArgsConstructor로 DI시 @Qualifier 사용 이슈

## 문제

`Lombok` `@RequiredArgsConstructor` 을 사용해서 `final` 필수 멤버변수를 자동 생성자를 만들어주면서 빈 주입이 되도록 한다.

```java
@RequiredArgsConstructor
public class WantQuailfierAutowired{

    @Qualifier("myTarget")
    private final Target taget;
}
```

그런데 이때 `Spring` 의 `@Qualifier` 를 사용해서 빈 지정을 해줘도 에러가 발생한다.

```sh
Parameter 0 of constructor in podo.WantQuailfierAutowired required a single bean, but 2 were found:
```



## 원인

`Lombok`은 `@RequiredArgsConstructor` 어노테이션을 사용하면 자동으로 생성자를 만드는데, 인자에 `@Qualifier`를 붙여주지 않는다.

* **Lombok이 만든 class **

```java
@RequiredArgsConstructor
public class WantQuailfierAutowired{

    @Qualifier("myTarget")
    private final Target taget;
    
    public WantQuailfierAutowired(Target target){ // @Qualifier가 없음
        this.target = target;
    }
}
```

스프링은 어떤 빈을 주입해야 하는지 판단하지 못한다.



## 해결법

 ### 1. 인스턴스 이름 변경

```java
@RequiredArgsConstructor
public class WantQuailfierAutowired{

    //@Qualifier("myTarget")
    private final Target myTarget; // 인스턴스 이름 변경!
}
```

만약 `@Primary` 를 사용했다면 이 방법으로 해결이 불가능하다.

이름을 변경해도 `@Primary`가 붙은 빈으로 계속 주입이 된다. 

이런 경우 Lombok 설정 추가 방법으로 해보자.



### 2. Lombok 설정 추가

프로젝트 최 상단에 lombok.config 파일 생성 후 아래 코드 입력

```
lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Qualifier
```

그리고 컴파일을 해보면 생성된 class 파일에 `@Qualifier`가 인자 부분에 포함된 것을 확인 할 수 있다.

```java
@RequiredArgsConstructor
public class WantQuailfierAutowired{

    @Qualifier("myTarget")
    private final Target taget;
    
    public WantQuailfierAutowired(@Qualifier("myTarget") Target target){ 
        this.target = target;
    }
}
```

