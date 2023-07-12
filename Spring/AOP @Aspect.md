# AOP @Aspect

스프링에서 AOP를 사용하기 위한 어노테이션.

### Advice 어노테이션

| 어노테이션      | 설명                                                         |
| --------------- | ------------------------------------------------------------ |
| @Before         | 메소드가 실행되기 이전에 실행.                               |
| @After          | 메소드가 종료된 후 무조건 실행. (try-catch에서 finally와 같은 동작) |
| @AfterReturning | 메소드가 성공적으로 완료되고 리턴한 다음에 실행.             |
| @AfterThrowing  | 메소드 실행 중 예외가 발생하면 실행. (try-catch에서 catch와 같은 동작) |
| @Around         | 메소드 호출 자체를 가로채서 메소드 실행 전후에 처리할 로직을 삽입. |

### Advice 어노테이션에 Pointcut 적용 샘플

```java
@Component
@Aspect
public class SpringAspect {
	// -1-
	@Before("execution(* aop *.*(..)")
	public void aop1() {}
	
    // -2-
	private final String pointcut = "execution(* aop *.*(..)";
	@Before("execution(* aop *.*(..)")
	public void aop2() {}
	
    // -3-
	@Pointcut("execution(* aop *.*(..)")
	private void method() {} 
	
	@Before("method()")
	public void aop3() {}
}
```

1. 어노테이션에 직접 문자열로 적용
2. String 변수를 활용하여 적용
3. Pointcut 메서드를 선언 후 해당 메서드로 적용

[AOP 포인트컷(Pointcut) execution() 표현식](./AOP 포인트컷(Pointcut) execution() 표현식.md)

### AOP 사용 예제

```java
@Aspect
@Component
@Slf4j
public class LogAopHelperCLS {
    
    @Pointcut("execution(* com.kbs..*.*(..))")
    public void AspectSample(){ }

    @Around("AspectSample()")
    public Object Around(ProceedingJoinPoint joinPoint) throws Throwable {
        log.info("AspectJ TEST  : Around Logging Start");
        try {
            Object result = joinPoint.proceed();
            log.info("AspectJ TEST  : Around Logging END");
            return result;
        }catch (Exception e) {
            log.error("AspectJ Around Exception");
            log.error(e.toString());
            return null;
        }
    }
    
}
```

```java
@RestController
@RequestMapping("/aspect")
@Slf4j
public class AspectController {

    @GetMapping("/test")
    public String getTest() {
        log.info("[[[[aspect getTest ");
        return "aspect getTest!!!";
    }
}
```

