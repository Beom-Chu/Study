# [DATA JPA] findFirst vs findTop

## 차이

| method        | 결과수 제한          | 리턴타입     |
| ------------- | --------------- | -------- |
| `findFirst`   | 단일 결과만을 반환      | Optional |
| `findTop[숫자]` | 숫자 수 만큼의 결과를 반환 | List     |

## 언제 사용?

* `findFirst` : 단일 결과만 필요한 경우에 사용

* `findTop` : 결과의 상위 n개만 필요로 할때 사용

## 예시

* findFirst

```java
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findFirstByUsername(String username);
}
```

=> 사용자명으로 첫번째 사용자를 조회한다.

* findTop

```java
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findTop5ByOrderByCreatedAtDesc();
}
```

=> 생성일 기준으로 최근 5명의 사용자를 조회한다.
