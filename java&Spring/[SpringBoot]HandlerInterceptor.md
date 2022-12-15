# [SpringBoot] HandlerInterceptor

## HandlerInterceptor

Spring Framework에서 지원하는 기능이며, URI 요청, 응답 시점을 가로채서 전/후 처리를 하는 역할을 합니다. Interceptor 시점에 **Spring Context**와 **Bean**에 접근할 수 있습니다. 

이와 비슷한 역할로 Filter와 AOP가 있습니다.

**Filter**는 Spring Framework와 무관하게 동작하며, Spring 자원을 이용할 수 없습니다. Filter는 보통 인코딩, XSS방어 등...의 용도로 이용됩니다. 

**AOP**는 주로 비즈니스 로직에서 실행됩니다. Logging, transaction 처리 등 중복 코드가 발생할 경우 중복을 줄이기 위해 사용되며, 메소드 처리 전후 지점에 자유롭게 설정이 가능합니다. 

## Filter, Interceptor, AOP의 흐름

![](./images/Filter,Interceptor,AOP흐름.png)

## Interceptor 생성

```java
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor.preHandle");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("MyInterceptor.postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("MyInterceptor.afterCompletion");
    }
}
```

* **PreHandle**(HttpServletRequest request, HttpServletResponse response, Object handler)
  * 컨트롤러에 진입하기 전에 실행됩니다. 반환 값이 true일 경우 컨트롤러로 진입하고 false일 경우 진입하지 않습니다.
  * Object handler는 진입하려는 컨트롤러의 클래스 객체가 담겨있습니다.

* **PostHandle**(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)
  * 컨트롤러 진입 후 View가 랜더링 되기 전에 수행됩니다.

* **afterComplete**(HttpServletRequest request, HttpServletResponse response, Object object, Exception ex)
  * 컨트롤러 진입 후 view가 랜더링 된 후에 실행되는 메소드입니다.

* **afterConcurrentHandlingStarted**(HttpServletRequest request, HttpServletResponse response, Object h)
  * 비동기 요청 시 PostHandle과 afterCompletion이 수행되지 않고 afterConcurrentHandlingStarted가 수행됩니다. 

## Interceptor 등록하기

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor())
                .addPathPatterns("/interceptor/*")  // 인터셉터 대상 패턴
                .excludePathPatterns("/interceptor/excludePath");    // 인터셉터 제외 패턴
    }
}
```

WebMvcConfigurer를 구현해 Spring Boot에서 기본적으로 제공해주는 설정 중 interceptor부분을 커스텀합니다.

## InterCeptor Controller

```java
@RestController
@RequestMapping("/interceptor")
public class InterceptorController {

    @GetMapping("/test")
    public String test() {
        System.out.println("인터셉트 테스트 : /test");
        return "인터셉트 포함 path 패턴";
    }

    @GetMapping("/excludePath")
    public String excludePath() {
        System.out.println("인터셉트 테스트 : /excludePath");
        return "인터셉트 제외 path 패턴";
    }
}

```

<br><br>

<br>

ref : https://bamdule.tistory.com/149