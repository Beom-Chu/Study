# [Spring]Cache

### 캐시(Cache)란
캐시는 서버의 부담을 줄이고 성능을 높이기 위해 사용하는 기술입니다.
복잡한 로직이나 DB에서 데이터를 가져오는데 많은 시간이 소요되는 경우에 해당 데이터를 캐시에 등록 후 빠르게 사용할 수 있습니다.
캐시는 동일한 데이터를 반복적으로 처리하는 상황에서 유리합니다.
매번 다른 결과값을 가져와야하는 상황에서는 오히려 성능이 떨어지게 됩니다.

</br>
### Spring의 캐시 추상화
Spring에서는 AOP 방식으로 편리하게 메소드에 캐시 서비스를 적용 할 수 있습니다.
이를 통해 캐시 관련 로직을 비즈니스 로직에서 분리하고 쉽게 캐시 기능을 적용 할 수 있습니다.
그리고 Spring은 캐기 구현 기술에 종속되지 않도록 추상화된 서비스를 제공합니다.
그로 인해 환경이 바뀌거나 적용할 캐시 기술을 변경해도 코드에 영향을 주지 않습니다.

</br>
### Spring에서 캐시 사용

#### `@EnableCaching` 추가
Spring에서 어노테이션 기반의 캐시 기능을 사용하기 위해서는 먼저 별도의 선언이 필요합니다.
아래와 같이 설정 크래스에 `@EnableCaching` 어노테이션을 사용해 줘야 합니다.
```java
@EnableCaching
@Configuration 
public class CacheConfig {
    ... 
}
```

#### 캐시 매니저 빈 추가
어노테이션을 추가한 후에는 캐시를 관리해줄 CacheManager를 빈으로 등록해주어야 합니다.

* `ConcurrentMapCacheManager`: Java의ConcurrentHashMap을 사용해 구현한 캐시를 사용하는 캐시매니저
* `SimpleCacheManager`: 기본적으로 제공하는 캐시가 없어 사용할 캐시를 직접 등록하여 사용하기 위한 캐시매니저
* `EhCacheCacheManager`: 자바에서 유명한 캐시 프레임워크 중 하나인 EhCache를 지원하는 캐시 매니저
* `CompositeCacheManager`: 1개 이상의 캐시 매니저를 사용하도록 지원해주는 혼합 캐시 매니저
* `CaffeineCacheManager`: Java 8로 Guava 캐시를 재작성한 Caffeine 캐시를 사용하는 캐시 매니저
* `JCacheCacheManager`: JSR-107 기반의 캐시를 사용하는 캐시 매니저

많은 캐시매니저들 중에서 필요에 맞는 캐시 매니저를 찾아서 선택하면 됩니다.
위와 같이 캐시 매니저를 추가하면 해당 캐시 매니저가 우리의 캐시 관련 기능들을 담당하여 처리하게 됩니다.

#### 캐시 기능 어노테이션
##### `@Cacheable` 캐시 저장/조회
클래스나 인터페이스에도 캐시를 지정할 수는 있지만 보통 메소드 단위로 적용합니다. 
캐시를 적용할 메소드에 `@Cacheable` 어노테이션을 붙이면 아래와 같이 동작합니다.
1. 캐시에 데이터가 없으면 해당 로직을 수행하여 캐시에 데이터를 등록 및 반환
2. 캐시에 데이터가 있으면 로직 수행하지 않고 바로 데이터 반환

##### `@CachePut` 캐시 갱신
`@CachePut` 어노테이션을 사용하면 해당 캐시를 갱신합니다.
`@CachePut`은 `@Cacheable`과 유사하게 실행 결과를 캐시에 저장하지만, 조회 시에 저장된 캐시의 내용을 사용하지는 않고 항상 메소드의 로직을 실행한다는 점이 다릅니다.

##### `@CacheEvict` 캐시 삭제
캐시는 적절한 시점에 제거되어야 하는데, 만약 값이 달라진다면 캐시를 제거해야 할 것입니다. 
캐시를 제거하는 방법은 크게 다음의 2가지가 있습니다.
1. 일정한 주기로 캐시를 제거
2. 값이 변할 때 캐시를 제거

##### 샘플 코드
```java
@RestController
@AllArgsConstructor
public class CacheController {

	private final MenuService menuService;

    @GetMapping("/order/menu")
    @Cacheable(value = "menu") /* 캐시 저장 */
    public List<OrderEntity> getOrderMenu(){
	
        System.out.println("메뉴 조회 / 캐시 저장 ");
        return menuService.getMenu();
    }

    @GetMapping("/order/menu/put")
    @CachePut(value = "menu") /* 캐시 갱신 */
    public List<OrderEntity> updateMenu(){
	
        System.out.println("메뉴 캐시 업데이트");
        return menuService.getMenu();
    }

    @GetMapping("/order/menu/delete")
    @CacheEvict(value = "menu", allEntries = true) /* 캐시 삭제 */
    public String deleteMenu(){
	
        System.out.println("메뉴 캐시 삭제");
        return "삭제 완료";
    }
}
```
