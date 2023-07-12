# 전역 예외처리(@RestControllerAdvice,@ExceptionHandler)



`@RestControllerAdvice`와 `@ExceptionHandler`는 예외 처리와 관련된 기능을 제공하는 어노테이션입니다.

이 두 어노테이션은 컨트롤러 예외처리를 단순화하고, 응답을 일관성 있게 처리 할 수 있도록 도와 줍니다.

* `@RestControllerAdvice`
  * 전역 예외 처리를 정의하는데 사용
  * 하나 이상의 클래스에 예외 처리 기능을 제공
* `@ExceptionHandler` 
  * 메서드를 포함
  * 해당 클래스에서 발생하는 예외를 처리

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        // 예외 처리 로직
        return new ResponseEntity<>("Something went wrong", HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

위 샘플 코드를 보면 `@RestControllerAdvice` 는 전역 예외 처리 클래스를 정의하고 있습니다.

`@ExceptionHandler(Exception.class)`는 `Exception` 타입의 예외를 처리하는 메서드를 정의 하고 있습니다.

이 메서드는 `Exception` 이 발생했을 때 실행되고 `ResponseEntity` 객체를 반환 합니다.

위 경우에는 `"Something went wrong"` 라는 응답 본문과 `HttpStatus.INTERNAL_SERVER_ERROR` 상태 코드를 반환 합니다.

커스텀 Exception을 만들어서도 사용 가능 합니다.

`@RestController` 를 사용한 컨트롤러에서는 `@RestControllerAdvice` 를 `@Controller` 를 사용한 컨트롤러에서는 `@ControllerAdvice` 를 사용 합니다.

**특정 컨트롤러에서만 예외 처리**를 하고 싶은 경우에는 아래와 같이 사용 가능합니다.

```java
@RestControllerAdvice(basePackages = "com.example.controllers")
@RestControllerAdvice(assignableTypes = MyController.class)
```

`basePackages` : 예외 처리 패키지

`assignableTypes` : 예외 처리 클래스

