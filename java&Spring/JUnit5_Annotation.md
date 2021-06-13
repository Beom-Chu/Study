# JUnit5 Annotation

#### @Test

* 해당 메서드는 테스트 대상

```java
	@Test
	void test() {
		System.out.println("@Test");
	}
```



#### @ParameterizedTest

`@ValueSource`와 같이 사용되며 테스트와 동일.

#### @ValueSource

해당 annotation에 지정한 배열을 파라미터 값으로 순서대로 전달.

리터럴 값의 배열을 테스트 메서드에 전달.

##### literal values 종류

short, byte, int, long, float, double, char, java.lang.String, java.lang.Class

```java
	@ParameterizedTest
	@ValueSource(ints = {1, 2, 3, 4, 5}) // 5개 인수
	void parameterizedTest(int number) {
		System.out.println(number);
	}
    //1
    //2
    //3
    //4
    //5
```



#### @RepeatedTest

동일 Test를 반복할 때 사용.

```java
	@TestFactory
	@RepeatedTest(3)
	void repeatedTest() {
		System.out.println("@RepeatedTest");
	}
	//@RepeatedTest
	//@RepeatedTest
	//@RepeatedTest
```



#### @BeforeAll

* 해당 메서드가 현재 클래스의 모든 테스트 메서드보다 먼저 실행.
* 해당 메서드는 static 필수.

```java
	@BeforeAll
	static void beforeAll() {
		System.out.println("@BeforeAll");
	}
```



#### @BeforeEach

* 해당 메서드는 각 테스트 메서드 전에 실행.

```java
	@BeforeEach
	void beforeEach() {
	    System.out.println("@BeforeEach");
	}
```



#### @DisplayName

* 테스트 클래스 또는 메서드의 이름을 정의

```java
	@Test
	@DisplayName("첫번째 테스트")
	void test() {
		System.out.println("@Test");
	}
```



#### @Disabled

* 테스트 클래스 또는 메서드를 비활성화.

```java
	@Test
	@DisplayName("두번째 테스트")
	@Disabled
	void test2() {
		System.out.println("@Test2");
	}
```



#### @AfterAll

* 해당 메서드가 현재 클래스의 모든 테스트 메소드보다 이후에 실행.
* 해당 메서드는 static 필수.

```java
	@AfterAll
	static void afterAll() {
		System.out.println("@AfterAll");
	}
```



#### @AfterEach

* 해당 메서드가 각 테스트 메서드 이후에 실행

```java
	@AfterEach
	void afterEach() {
		System.out.println("@AfterEach");
	}
```





## JUnit assert Method

| Method                                                       | 내용                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| assertArrayEquals(a,b)                                       | 배열 a와b가 일치하는지 확인한다.                        |
| assertEquals(a,b)                                            | 객체 a와b의 값이 같은지 확인한다.                       |
| assertSame(a,b)                                              | 객체 a와b가 같은 객체인지 확인한다.                     |
| assertTrue(a)                                                | a가 참인지 확인한다.                                    |
| assertFalse(a)                                               | a가 거짓인지 확인한다.                                  |
| assertNotNull(a)                                             | a객체가 Null이 아님을 확인한다.                         |
| assertThrows(CustomException.class, () -> new CustomClass(-1)) | 첫번째 인자로 발생할 예외 클래스의 Class 타입을 받는다. |

