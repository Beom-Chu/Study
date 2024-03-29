# @Bean과 @Component 차이

스프링 MVC에서는 `@Controller`, `@Service`, `@Repository` 등으로 빈으로 등록할 수 있으며, 

configuration 관련 객체들은 `@Bean`과 `@Component` 으로 스프링 컨테이너에 객체를 빈으로 등록할 수 있다.

<br>

그럼 `@Bean` 과 `@Component`의 차이는 무엇일까?

* `@Bean` : 메소드 레벨에서 선언하며, 반환되는 객체(인스턴스)를 개발자가 수동으로 빈으로 등록.

* `@Component` : 클래스 레벨에서 선언함으로써 스프링이 런타임시에 컴포넌트스캔을 하여 자동으로 빈을 찾고(detect) 등록.

<br>

**@Bean 사용 예제**

```
@Configuration
public class AppConfig {
   @Bean
   public MemberService memberService() {
      return new MemberServiceImpl();
   }
}
```

**@Component 사용 예제**

```
@Component
public class Utility {
   // ...
}
```

블로그 <기억보단 기록을>의 저자인 동욱님께서는 개발자가 컨트롤이 불가능한 외부 라이브러리를 빈으로 등록하고 싶을때 `@Bean`을 사용하며, 

개발자가 직접 컨트롤이 가능한 클래스의 경우 `@Component`를 사용한다고 한다.

<br>

| Bean                                                   | Component                                        |
| :----------------------------------------------------- | :----------------------------------------------- |
| 메소드에 사용                                          | 클래스에 사용                                    |
| 개발자가 컨트롤이 불가능한 외부 라이브러리 사용시 사용 | 개발자가 직접 컨트롤이 가능한 내부 클래스에 사용 |

<br>

## @Bean 사용시 주의 사항

* 메서드 이름과 반환 타입
  * `@Bean` 어노테이션을 사용하여 빈을 생성할 때, 메서드 이름은 빈의 이름, 메서드 반환 타입은 빈의 타입으로 사용 됩니다. 따라서 메서드 이름과 반환 타입이 일치하도록 신경써 줘야 합니다.
* 스코프
  * `@Bean` 어노테이션에는 기본적으로 싱글톤 스코프로 생성 됩니다. 
  * 만약 다른 스코프로 생성하고 싶다면, `@Scope` 어노테이션을 사용하여 명시적으로 지정해야 합니다.
* 의존성 관리
  * `@Bean` 어노테이션을 사용하여 빈을 생성할 때, 해당 빈이 의존하는 다른 빈들을 명시적으로 주입해주어야 합니다. 그렇지 않으면 런타임 오류가 발생할 수 있습니다.
* 빈 생성 순서
  * `@Bean` 어노테이션으로 빈을 생성할 때, 생성 순서가 중요한 경우가 있습니다. 이때 `@DependsOn` 어노테이션을 사용하여 빈 생성 순서를 명시적으로 지정해주어야 합니다.

## @Component 사용시 주의 사항

* 이름 지정
  * `@Component` 어노테이션을 사용하여 클래스를 스프링의 컴포넌트로 등록할 때, 클래스 이름이 빈의 이름으로 사용 됩니다. 
  * 만약 빈의 이름을 명시적으로 지정하고 싶다면, `value` 옵션을 줘서 지정 합니다. ex)`@Component(value="mystudent")`
* 스코프
  * 기본적으로 싱글톤 스코프로 생성 됩니다.
  * 다른 스코프로 생성하고 싶다면, `@Scope` 어노테이션을 사용 합니다.
* 의존성 주입
  * `@Component` 어노테이션을 사용하여 클래스를 스프링의 컴포넌트로 등록하면, 해당 클래스가 다른 빈들과 의존성을 가질 수 있습니다. 이때 의존하는 빈들은 명시적으로 주입되어야 합니다.
  * `@Autowired` 어노테이션을 사용하여 필요한 빈을 자동으로 주입할 수 있습니다.
* 프로파일링
  * `@Component` 어노테이션은 스프링의 프로파일링 기능을 지원하지 않습니다.
  * 만약 특정 프로파일에서만 빈을 등록하고 싶다면, `@Profile` 어노테이션을 사용 합니다.

<br><br><br>

참조 : https://youngjinmo.github.io/2021/06/bean-component/
