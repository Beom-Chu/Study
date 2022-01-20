# Property(yaml) 값 주입

### @Value("") 사용

변수에 문자열을 주입해서 사용.

```java
@RestController
public class ValueController {

    @Value("Hello World")
    private String greetingMessage;

    @GetMapping("")
    public String sendGreeting(){
        return greetingMessage;
    }
}
```

`String` 뿐만 아니라 다른 타입에도 사용 가능.

```java
@Value("1.234")
private double doubleValue; //could be Double

@Value("1234")
private Integer intValue; //could be int

@Value("true")
private boolean boolValue; //could be Boolean

@Value("2000")
private long longValue;
```



### @Value("${...}") 사용

`application.properties` , `application.yml`에 정의한 내용을 가져와서 사용

```properties
# application.properties
greeting.message=Hello World!
```
```yaml
# application.yml
greting:
  message: Hello World!
```

```java
@RestController
public class ValueController {

    @Value("${greeting.message}") 
    private String greetingMessage;

    @GetMapping("")
    public String sendGreeting(){
        return greetingMessage;
    }
}
```

속성 값은 런타임에 변수로 주입되며 만약 속성값이 properties 파일에 없으면 아래와 같은 오류가 발생.

```java
Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2020-02-29 21:54:43.953 ERROR 2996 --- [main] o.s.boot.SpringApplication: Application run failed
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'valueController': Injection of autowired dependencies failed; nested exception is java.lang.IllegalArgumentException: Could not resolve placeholder 'greeting.message' in value "${greeting.message}"
```

### @Value("${...:...}") 기본값 설정

오류를 방지하기 위해 `@Value` 어노테이션에는 기본값을 설정 가능.

`@Value("${greeting.message:Greeting not found!}")` 처럼 콜론을 붙이고 뒤에 기본값을 붙임.

만약 콜론 앞에 공백이 있으면 에러가 발생.

```properties
# application.properties
my.int.value=20
my.double.value=3.142
my.boolean.value=true
```
```java
@Value("${my.int.value:0}")
private int intValue; // 런타임에 20 주입

@Value("${my.double.value: 0.0}")
private double doubleValue; // 런타임에 3.142 주입

// property 에 값이 있어도 공백 때문에 기본값이 들어감
@Value("${my.boolean.value :false}")
private boolean boolValue; // 공백 때문에 false 주입

// proprety 에 값이 없어서 기본값 사용
@Value("${my.long.value:300}")
private long longValue; // 런타임에 300 주입
```



### @Value("${...}") 로 List 주입

```properties
# application.properties
my.weekdays=Mon,Tue,Wed,Thu,Fri
```
```java
@Value("${my.weekdays}")
private List<String> strList; // injects [Mon, Tue, Wed, Thu, Fri]

//Providing default value
@Value("${my.weekends:Sat,Sun,Fri}")
private List<String> strList2; // injects [Sat, Sun, Fri]
```



### @Value("#{${...}}") 로 Map 주입

```properties
# application.properties
database.values={url:'http://127.0.0.1:3306/', db:'mySql', username:'root', password:'root'}
```
```java
@RestController
public class ValueController {

    @Value("#{${database.values: {url: 'http://127.0.0.1:3308/', db: 'mySql', username: 'root', password: ''}}}")
    private Map<String, String> dbValues;

    @GetMapping("")
    public Map getDBProps(){
        return dbValues;
    }
}
```



### @Value("${...}") 생성자 파라미터에 주입

생성자에 파라미터로 넘기면서 값을 주입할 수도 있다.

```properties
# application.properties
company.name= Scopesuite Pty ltd
# company.location= Sydney
```
```java
@Service
public class CompanyService {
   private String compName;
   private String location;

   public CompanyService(
    @Value("${company.name}") String compName,
    @Value("${company.location:Washington}") String location
   ) {
       this.compName = compName;
       this.location = location;
   }
}
```