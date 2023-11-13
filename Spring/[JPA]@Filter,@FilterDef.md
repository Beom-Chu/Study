# [JPA] @Filter, @FilterDef

Spring Data JPA에서 `@Filter` 및 `@FilterDef` 애노테이션은 Hibernate의 기능을 이용하여 엔터티에 대한 동적 필터링을 적용하는 데 사용됩니다. 
동적 필터링은 특정 조건에 따라 엔터티의 데이터를 조회하거나 변경할 수 있게 해줍니다.

### @FilterDef
`@FilterDef` 애노테이션은 필터를 정의하는 데 사용됩니다. 
이 애노테이션은 JPA 엔터티 클래스에 적용됩니다.

```java
@Entity
@FilterDef(name = "activeUserFilter", parameters = @ParamDef(name = "isActive", type = "boolean"))
public class User {
    // 엔터티 필드 및 메서드 정의
}
```
* name: 필터의 이름을 정의
* parameters: 필터에 전달되는 파라미터를 정의

### @Filter
`@Filter` 애노테이션은 `@FilterDef`에서 정의한 필터를 적용하는 데 사용됩니다. 
이 애노테이션은 엔터티 클래스의 필드 레벨이나 클래스 레벨에 적용될 수 있습니다.

```java
@Entity
@Filter(name = "activeUserFilter", condition = ":isActive = true")
public class User {
    // 엔터티 필드 및 메서드 정의
}
```
* name: @FilterDef에서 정의한 필터의 이름을 지정
* condition: 필터의 조건을 지정합니다. 여기서 :isActive는 @FilterDef에서 정의한 파라미터 중 하나


### 사용 예시
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    @Query("SELECT u FROM User u")
    @Filter(name = "activeUserFilter", condition = ":isActive = true")
    List<User> findActiveUsers(@Param("isActive") boolean isActive);

}
```
이렇게 설정된 경우, findActiveUsers 메서드를 호출할 때 activeUserFilter 필터가 자동으로 적용되어 isActive 값이 true인 사용자만 검색됩니다. 
필터를 사용하면 특정 조건에 따라 엔터티를 조회하거나 변경할 수 있어서 유연하고 동적인 쿼리를 작성할 수 있게 됩니다.