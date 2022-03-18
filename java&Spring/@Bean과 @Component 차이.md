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



<br><br><br>

참조 : https://youngjinmo.github.io/2021/06/bean-component/
