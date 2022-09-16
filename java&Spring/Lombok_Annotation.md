# Lombok Annotation

| Annotation               | 설명                                                         |
| ------------------------ | ------------------------------------------------------------ |
| @Getter                  | Class에 선언된 변수들의 Get 메소드 생성.                     |
| @Setter                  | Class에 선언된 변수들의 Set 메소드 생성.                     |
| @ToString                | toString() 메소드를 자동으로 생성.<br>Exclude로 제외 필드 지정 가능. |
| @NonNull                 | 변수에 선언하며 해당 변수는 반드시 값이 있어야 함.<br>Setter에 Null값 입력시 NullPointException 발생. |
| @NoArgsConstructor       | 파라미터가 없는 생성자를 생성.                               |
| @RequiredArgsConstructor | final, @NonNull인 변수만 파라미터로 받는 생성자를 생성.      |
| @AllArgsConstructor      | 모든 변수를 파라미터로 받는 생성자를 생성.                   |
| @Data | @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 모두 포함한 Annotation. |
| @Value | 불변의 객체를 선언.<br>해당 Annotation 사용시 Setter 메소드 사용 불가. |
| @Cleanup | close() 메소드를 자동으로 호출. |
| @EqualsAndHashCode | equals, hashCode 메소드 생성.<br>exclude로 제외 처리 가능. |
| @Builder | 빌더 패턴을 사용할 수 있도록 해줌. |
| @Builder.Default | 빌더 패턴 사용시 특정 필드의 기본값을 설정해줄 경우 사용. |
| @Accessors(chain=true) | 객체 생성 후 체인 형태로 set 메소드 사용 가능. |
| @Slf4j | 자동으로 log 필드를 만들고, 해당 클래스의 이름으로 로거 객체를 생성하여 할당. |
| @SneakyThrows | 명시적인 예외 처리를 생략 가능. |

<br>

## 사용시 주의 사항

#### @AllArgsConstructor

* 발생할 수 있는 문제 상황

  예로 들어, 두 개의 같은 타입 인스턴스 멤버를 선언한 상황에서 개발자가 선언된 인스턴스 멤버의 순서를 바꾸면, 개발자도 인식하지 못하는 사이에 lombok이 생성자의 파라미터 순서를 필드 선언 순서에 따라 변경하게 된다. 이때, IDE가 제공해주는 리팩토링은 전혀 동작하지 않고, 두 필드가 동일 타입이기 때문에 기존 소스에서도 오류가 발생하지 않아 아무런 문제없이 동작하는 것으로 보이지만, 실제로 입력된 값이 바뀌어 들어가는 상황이 발생한다.

#### @Data

- @Data = @RequiredArgsConstructor + @Getter + @Setter + @ToString + @EqualsAndHashCode
  - 각 annotation의 주의사항을 모두 포함한다.
  - @EqualsAndHashCode : equals 메소드와 hashcode 메소드를 생성한다
    - 예를 들어, 객체를 Set에 저장한 뒤 필드 값을 변경하면 hashCode가 변경되면서 이전에 저장한 객체를 찾을 수 없는 문제가 발생한다.
  - @Data 대신, @Getter, @Setter, @ToString으로 명시하는 것을 권장한다.

#### @NoArgsConstructor

- 사용 시 주의 사항
  - 필드들이 final로 생성되어 있는 경우에는 필드를 초기화할 수 없기 때문에 생성자를 만들 수 없고 에러가 발생한다. → @NoArgsConstructor(force=true) 옵션을 이용해 final 필드를 강제 초기화 시켜 생성자를 만들 수 있다.
  - @Nonnull 같이 필드에 제약조건이 설정되어 있는 경우, 추후 초기화를 진행하기 전까지 생성자 내 null-check 로직이 생성되지 않는다.

#### @Builder

- Builder를 사용하면 객체 생성이 명확해진다. 가급적 클래스 보다는 직접 만든 생성자 혹은 static 객체 생성 메소드에 붙이는 것을 권장한다.
- @Builder를 붙이면 파라미터 순서가 아닌 이름으로 값을 설정하기 때문에 리팩토링에 유연하게 대응할 수 있고, 필드 순서를 변경해도 문제가 없다.

#### @Accessors(chain=true)

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Accessors(chain = true)
public class AccessorsChain {
    private int id;
    private String name;
}
```

```java
@Test
public void test (){
    AccessorsChain accessorsChain = new AccessorsChain();
    accessorsChain.setId(10).setName("testName");
}
```

#### @Cleanup

```java
   public void copyFile(String in, String out) throws IOException {
       @Cleanup FileInputStream inStream = new FileInputStream(in);
       @Cleanup FileOutputStream outStream = new FileOutputStream(out);
       byte[] b = new byte[65536];
       while (true) {
           int r = inStream.read(b);
           if (r == -1) break;
           outStream.write(b, 0, r);
       }
   }
```
위와 같이 사용시 아래처럼 컴파일.
```java
	public void copyFile(String in, String out) throws IOException {
       @Cleanup FileInputStream inStream = new FileInputStream(in);
       try {
           @Cleanup FileOutputStream outStream = new FileOutputStream(out);
           try {
               byte[] b = new byte[65536];
               while (true) {
                   int r = inStream.read(b);
                   if (r == -1) break;
                   outStream.write(b, 0, r);
               }
           } finally {
               if (outStream != null) outStream.close();
           }
       } finally {
           if (inStream != null) inStream.close();
       }
   }
```

#### @Slf4j

```java
   @Slf4j
   public class LogExample {
   }
```
```java
   public class LogExample {
       private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);
   }
```

#### @SneakyThrows

```java
   @SneakyThrows(UnsupportedEncodingException.class)
   public void utf8ToString(byte[] bytes) {
       return new String(bytes, "UTF-8");
   }
```

```java
   public void utf8ToString(byte[] bytes) {
       try {
           return new String(bytes, "UTF-8");
       } catch (UnsupportedEncodingException $uniqueName) {
           throw useMagicTrickeryToHideThisFromTheCompiler($uniqueName);
           // This trickery involves a bytecode transformer run automatically during the final stages of compilation;
           // there is no runtime dependency on lombok.
       }
   }
```

</br>

### @Getter, @Setter, @Data 사용시 boolean 타입에 필드명이 is 시작하는 경우

일반적으로 생성 되듯이 `get[필드명]`, `set[필드명]` 처럼 생성 되지 않음.

`User.java`

```java
@Data
public class User {
    private int id;
    private boolean isAdult;
}
```

`User.class`

```java
// . . . 생략 . . .
	public int getId() {
        return this.id;
    }
    public boolean isAdult() {
        return this.isAdult;
    }
    public void setId(final int id) {
        this.id = id;
    }
    public void setAdult(final boolean isAdult) {
        this.isAdult = isAdult;
    }
// . . . 생략 . . .
```

위처럼 `getter` 는 `isAdult` 로, `setter` 는 `setAdult` 로 만들어진 걸 확인 가능. 

**이 때문에 Spring Controller에서 Java 객체 형식으로 받아서 처리할 때 해당 값이 항상 `false` 가 되는 현상이 발생**.

#### 해결 방법

1.  기본 타입인 `boolean` 대신에 참조 타입인 `Boolean` 을 사용
2.  기본 타입 `boolean` 을 사용하고 싶으면  직접 getter, setter 메서드를 만들어 사용. (ex : getIsAdult, setIsAdult)

<br>

#### 생성자 생성 어노테이션의 access 옵션(@NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor)

```java
@Data
@AllArgsConstructor(access = AccessLevel.PROTECTED)
@NoArgsConstructor(access = AccessLevel.PRIVATE)
public class AccessOption {
    private int id;
    private String name;
}
```

롬복으로 생성되는 생성자의 접근제어자를 설정 가능하다.

```java
// 빌드된 AccessOption.class
//...생략...//
	protected AccessOption(final int id, final String name) {
        this.id = id;
        this.name = name;
    }

    private AccessOption() {
    }
//...생략...//
```

생성자의 접근제어자가 각각 protected, private로 만들어진 걸 확인 할 수 있다.
