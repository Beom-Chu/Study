# [Spring]WebMvcConfigurer

Spring MVC 구성을 확장하고 맞춤화할 수 있는 인터페이스.

이 인터페이스를 구현하고 `@Configuration` 어노테이션을 사용하여 구성 클래스를 만들면 Spring에서 제공하는 기본 MVC구성을 수정하거나 확장 할 수 있습니다.

### 주요 메소드

* `addViewControllers`: 뷰 컨트롤러를 등록할 수 있습니다. 이를 통해 간단한 URL 매핑을 구현할 수 있습니다.
* `addResourceHandlers`: 정적 파일의 위치를 등록할 수 있습니다.
* `configureDefaultServletHandling`: 기본 서블릿 핸들러를 설정할 수 있습니다. 이를 통해 정적 리소스를 서비스할 수 있습니다.
* `addFormatters`: 컨트롤러에서 사용할 수 있는 포맷터를 등록할 수 있습니다.
* `addInterceptors`: 인터셉터를 등록할 수 있습니다. 이를 통해 컨트롤러 메서드가 실행되기 전과 후에 실행되는 작업을 구현할 수 있습니다.
* `configureMessageConverters`: 메시지 컨버터를 설정할 수 있습니다. 이를 통해 HTTP 요청 및 응답에서 사용되는 데이터 형식을 변환할 수 있습니다.
* `extendMessageConverters`: 메시지 컨버터를 확장할 수 있습니다. 이를 통해 기본적으로 제공되지 않는 데이터 형식을 처리할 수 있습니다.
* `addArgumentResolvers`: 컨트롤러에서 사용할 수 있는 아규먼트 리졸버를 등록할 수 있습니다. 이를 통해 컨트롤러 메서드에 전달되는 파라미터를 변환할 수 있습니다.
* `addReturnValueHandlers`: 컨트롤러의 반환값을 처리할 수 있는 핸들러를 등록할 수 있습니다. 이를 통해 컨트롤러 메서드의 반환값을 변환하거나, 추가적인 작업을 수행할 수 있습니다.
* `configureViewResolvers`: 뷰 리졸버를 설정할 수 있습니다. 이를 통해 컨트롤러에서 반환되는 뷰 이름을 해석하고, 뷰 객체를 생성할 수 있습니다.

### 샘플 코드 

```java
@Configuration
@EnableWebMvc
public class MvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/login").setViewName("login");
    }
    
    
    
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/resources/**").addResourceLocations("/static/");
    }
    
    
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoggingInterceptor());
    }
    
    private static class LoggingInterceptor implements HandlerInterceptor {
    
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            System.out.println("Request URL: " + request.getRequestURL().toString());
            return true;
        }
    }
    
    
    
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
        converters.add(converter);
    }
    
    
    
    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(new CurrentUserHandlerMethodArgumentResolver());
    }

    private static class CurrentUserHandlerMethodArgumentResolver implements HandlerMethodArgumentResolver {
    
        @Override
        public boolean supportsParameter(MethodParameter parameter) {
            return parameter.getParameterAnnotation(CurrentUser.class) != null;
        }
    
        @Override
        public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
            // 현재 사용자 정보를 가져와서 반환
            return SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        }
    }
}
```

