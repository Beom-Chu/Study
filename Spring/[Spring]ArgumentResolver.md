# [Spring] Argument Resolver

서비스는 다양한 종류의 데이터를 받게 됩니다. Argument Resolver는 어떠한 요청이 컨트롤러에 들어왔을 때, 요청에 들어온 데이터를 원하는 객체로 만들어내는 일을 간접적으로 해줍니다.

### 용도

예를 들어 JWT 토큰과 함께 요청이 들어왔다고 가정합니다. 우리는 이 토큰이 유효한지 검증 후 토큰에 저장된 ID를 꺼내 로그인 유저 객체로 만드는 과정이 필요합니다.

이러한 경우, Argument Resolver를 사용하지 않는다면 모든 컨트롤러에 토큰 검증과 로그인 유저 객체 변환하는 로직이 구현되어야 합니다. 그러면 중복 코드가 발생하고 컨트롤러의 책임이 증가하게 됩니다.

이러한 문제를 해결하기 위해 Argument Resolver를 사용합니다.

### 동작 순서

1. Client의 요청

2. Dispatcher Servlet에서 해당 요청을 처리

3. Client 요청에 대한 Handler Mapping

   1. Request Mapping에 대한 매칭

   2. **Interceptor 처리**

   3. **Argument Resolver 처리**

   4. Message Converter 처리

4. Controller Method Invoke

### 구현

Argument Resolver는 HandlerMethodArgumentResolver를 구현함으로써 사용 할 수 있습니다.

해당 인터페이스는 아래 두 메소드를 구현하도록 명시하고 있습니다.

* supportsParameter
  * 주어진 메소드의 파라미터가 이 Argument Resolver에서 지원하는 타입인지 검사.
  * 지원한다면 true, 아니면 false 반환.
* resolveArgument
  * supportsParameter에서 true를 받는 경우 parameter가 원하는 형태로 정보를 바인딩하여 반환.

```java
public class UserArgumentResolver implements HandlerMethodArgumentResolver {
    
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.getParameterType().equals(UserDto.class);
    }

    @Override
    public Object resolveArgument(@NotNull MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) {

        HttpServletRequest request = (HttpServletRequest) webRequest.getNativeRequest();
        String remoteAddr = request.getRemoteAddr();
        String uri = request.getRequestURI();

        String id = webRequest.getParameter("id");

        return new UserDto(id, remoteAddr, uri);
    }
}
```

**[구현 샘플](https://github.com/Beom-Chu/Template-Or-Test/tree/main/src/main/java/com/kbs/templateortest/argument/resolver)**

### 인터셉터와 차이점

* Argument Resolver는 인터셉터 이후에 동작을 하며, 어떠한 요청이 컨트롤러에 들어왔을 때, **요청에 들어온 값으로부터 원하는 객체를 반환**합니다.
* 인터셉터는 실제 컨트롤러가 실행되기 전에 요청을 가로채며, **특정 객체를 반환할 수 없습니다**. 오직 boolean 혹은 void 반환 타입만 존재합니다.