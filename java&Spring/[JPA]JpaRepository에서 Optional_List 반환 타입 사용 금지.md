# [JPA] JpaRepository에서 Optional<List<T>> 반환 타입 사용 금지

Spring Data JPA에서 `JpaRepository`를 상속받아 Interface로 CRUD를 간단하게 구현 할 수 있습니다.

그런데 여기서 기본으로 제공되는 `findById()`는 넘겨준 파라미터를 PK로 DB를 조회해서 객체를 가져오는데 이때 반환 타입은 `Optional` 입니다.

> ##### Optional이란?
>
> Java8에서부터 제공되는 Wrapper 클래스로 NPE(NullPointerException) 방지를 목적으로 합니다.
>
> Null이 들어 올 수도 있는 객체를 감싸주는 역할을 합니다.

이러한 패턴에 맞추려다 `JpaRepository` 상속으로 조회 메서드를 만들때 여러 건을 조회할 때 `Optional<List<T>>` 와 같은 반환 타입으로 만들게 되는 경우가 있습니다.

이 때 단건 객체를 반환할 때와 동일한 결과를 기대하고 `isPresent()` 와 같은 메서드를 사용하게 된다면 생각과는 다른 결과값을 얻을 수 있습니다.

```java
@Repository
public interface TestRepository extends JpaRepository<TestEntity, Long> {
    Optional<TestEntity> findByOne(Long id);
    Optional<List<TestEntity>> findByAll();
}
```

```java
@SpringBootTest
@Transactional
public class JpaOptionalTest {

    @Autowired
    private TestRepository testRepository;

    @Test
    public void test() {

        Optional<TestEntity> byId = testRepository.findById(1L);
        Optional<List<TestEntity>> allByName = testRepository.findAllByName("");

        System.out.println("[[[byId.isPresent() = " + byId.isPresent());
        System.out.println("[[[allByName.isPresent() = " + allByName.isPresent());
        
        // 출력 log
        // [[[byId.isPresent() = false
		// [[[allByName.isPresent() = true
    }
}
```

Entity에 해당하는 Table에 data가 하나도 없는 상태에서 위와 같이 조회해보면 다른 결과값이 반환되는 걸 확인 할 수 있습니다.

이유는 조회 결과가 없더라도 repository에서 반환되는 결과값의 List 객체는 null이 아니기 때문입니다.

결론은 **여러 건 조회시 반환 타입을 `Optional<List<T>>` 가 아닌 `List<T>`로만 사용하는게 좋겠습니다**.

<br>

### 관련 이슈 내용

관련 내용으로 이슈가 되었던건 spring boot 버전에 따라서 다른 결과가 나왔기 때문입니다.

기존 `2.3.7`  버전에서  `2.7.4`  버전으로 변경하는 작업을 했었습니다.

기존 버전에서는 위와 같이 `Optional<List<T>>` 타입으로 반환된 객체가 아무런 값이 없을 때 `isPresent()` 메서드의 응답값은 `true` 를 반환 했습니다.

그러나 버전 변경 후 응답값이 `false` 로 반환되었습니다.

<br>

[Test Code](https://github.com/Beom-Chu/Template-Or-Test/blob/main/src/test/java/com/kbs/templateortest/jpa/JpaOptionalTest.java)

