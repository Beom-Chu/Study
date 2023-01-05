# @SpringBootTest, 단위테스트

| 구분       | 통합테스트                                                   | 단위테스트                                                   |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 어노테이션 | @SpringBootTest                                              | @DataCassandraTest<br/>@DataJdbcTest<br/>@DataJpaTest<br/>@DataLdapTest<br/>@DataMongoTest<br/>@DataNeo4jTest<br/>@DataR2dbcTest<br/>@DataRedisTest<br/>@JdbcTest<br/>@JooqTest<br/>@JsonTest<br/>@RestClientTest<br/>@WebFluxTest<br/>@WebMvcTest<br/>@WebServiceClientTest |
| 설명       | 실제 운영 환경에 가까운 모든 의존성을 제공                   | 특정 범위의 테스트가 필요할 경우 활용되며 해당 단위테스트에 필요한 범위의 의존성을 제공 |
| 장점       | 실제 운영환경에 가까운 테스트가 가능하기 때문에 이슈가 적다. | 원하는 범위의 테스트에 대해 빠른 결과를 얻을 수 있다.        |
| 단점       | 간단한 테스트를 하려고 해도 모든 환경을 로드하기 때문에 시간이 오래 걸린다. | 모든 환경을 테스트하는 것이 아니기 때문에 단위테스트 외에서 문제가 발생 할 수 있다. |

<br>

## @SpringBootTest 옵션

#### 1. properties (value 또는 생략으로 입력해도 동일하게 동작)

* 속성 값을 정의

```java
@SpringBootTest( properties = { "testId=born", "testName=탄생" } )
//@SpringBootTest( value = { "testId=born", "testName=탄생" } )
//@SpringBootTest( { "testId=born", "testName=탄생" } )
class TestApplication {
    @Value("${testId}")
    private String testId; 
    
    @Value("${testName}")
    private String testName;
}
```

### 2. args

* application의 arguments를 삽입

```java
@SpringBootTest( args = {"--app.test=one", "--app.test2=two"} )
class TestApplication {
    @Value("${app.test}")
    private String appTest;

    @Autowired
    private ApplicationArguments args;
    
    @BeforeEach
    public void setUp() {
        assertThat(args.getOptionNames()).containsOnly("app.test");
        assertThat(args.getOptionValues("app.test")).containsOnly("one");
    }
}
```

### 3. classes

* 기본적으로 @SpringBootTest는 모든 빈을 등록한다. 하지만 classes 속성을 정의하면 해당 클래스의 빈만 정의.

```java
@SpringBootTest( classes = {ArticleServiceImpl.class, CommonConfig.class})
class TestApplication {
    @Autowired
    private ArticleServiceImpl articleServiceImpl;

    @Autowired
    private RestTemplate restTemplate;
}
```

### 4. WebEnvironment

* WebEnvironment.MOCK : 아무런 설정이 없을 시 적용되는 디폴트 설정이다.
  * mock 서블릿 환경으로 내장톰캣이 구동되지 않는다.(브라우저에서 접속되지 않는다.)

* WebEnvironment.RANDOM_PORT : 스프링부트를 직접 구동시킨 것처럼 **내장톰캣이 구동되나 랜덤포트로 구동된다.**

* WebEnvironment.DEFINED_PORT : **정의된 포트로 내장톰캣이 구동된다**
* WebEnvironment.NONE : WebApplicationType.NONE으로 구동된다.