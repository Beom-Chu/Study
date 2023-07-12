# CORS(Cross Origin Resource Sharing)

한 사이트의 스크립트에서 다른 사이트에 있는 콘텐츠에 접근 할 수 없는 제약 **SOP(Same Origin Policy)** 가 있습니다.

도메인이나 서브도메인, 프로토콜, 포트가 다른 곳으로 요청을 보내는 것을 **CORS**라고 합니다.

<br>

### Origin의 의미

Origin은 URL에서 **프로토콜, 도메인 포트 번호**를 합친 부분을 의미합니다.

> URL : https://it-eldorado.com:80/posts/123456?data=789

여기서 프로토콜에 해당하는 부분은 `https://` 이고, 도메인에 해당하는 부분은 `it-eldorado.com` 이며, 포트 번호에 해당하는 부분은 `:80` 입니다.

따라서 Origin은 `https://it-eldorado.com:80` 입니다.

<br>

### SOP와 CORS의 관계

**SOP(Same Origin Policy)는 동일 출처 정책**이라고 부릅니다.

이것은 다른 Origin으로 요청을 보낼 수 없도록 금지하는 브라우저의 기본적인 보안 정책입니다.

즉, 동일한 Origin으로만 요청을 보낼 수 있게 하는 것입니다.

아주 옛날부터 절대적인 규칙이었으며, 다른 Origin으로 요청을 보내는 것은 불가능했습니다.

그러나 기술이 발달하면서 서로 다른 Origin끼리 데이터를 주고받아야 하는 일이 많아졌고,

이로 인해 SOP는 별도의 예외 사항을 두게 되었습니다.

그것이 **CORS(Cross Origin Resource Sharing) : 교차 출처 자원 공유**로 다른 Origin으로 요청을 보낼 수 있게 하는 것입니다.

즉, **CORS는 다른 Origin으로 요청을 보내기 위해 지켜야 하는 정책으로, 원래대로라면 SOP에 의해 막히게 될 요청을 풀어주는 정책**이라고 볼 수 있습니다.

<br>

## SpringBoot CORS 적용

### @CrossOrigin 사용

```java
@CrossOrigin(originPatterns = "http://localhost:8080")
@RestController
public class LoginController {

    @GetMapping("/login")
    public String login() {
        return "로그인 성공!";
    }
}
```

컨트롤러 위 쪽에 해당 어노테이션을 붙이고 originPatterns 속성을 통해 요청을 허용할 출처를 입력해 줍니다.

### WebMvcConfigurer 사용

```java
@Configuration 
public class WebConfig implements WebMvcConfigurer { 
  @Override 
  public void addCorsMappings(CorsRegistry registry) { 
    registry
      .addMapping("/**") // 프로그램에서 제공하는 URL 
      .allowedOrigins("*") // 요청을 허용할 출처를 명시, *는 전체 허용 (가능하다면 목록을 작성한다. 
      .allowedHeaders("*") // 어떤 헤더들을 허용할 것인지 
      .allowedMethods("*") // 어떤 메서드를 허용할 것인지 (GET, POST...) 
      .allowCredentials(false) // 쿠키 요청을 허용한다(다른 도메인 서버에 인증하는 경우에만 사용해야하며, true 설정시 보안상 이슈가 발생할 수 있다) 
      .maxAge(1500); // preflight 요청에 대한 응답을 브라우저에서 캐싱하는 시간
  } 
}
```

#### 작성 예시

##### allowedOrigins

`.allowedOrigins("http://localhost:8080", "http://localhost:8081")`

##### allowedMethods

`.allowedMethods("GET", "POST")`

