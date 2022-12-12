# @Modifying 옵션

## @Modifying

**@Query** Annotation으로 작성 된 **변경, 삭제** 쿼리 메서드를 사용할 때 필요합니다. 주로 **벌크 연산**시에 사용 됩니다.

**JPA Entity LifeCycle**을 무시하고 쿼리가 실행되기 때문에 해당 어노테이션을 사용할 때는 영속성 콘텍스트 관리를 주의 해야합니다.

이 부분은 **@Modifying**과 함께 사용 할 수 있는 **clearAutomatically, flushAutomatically** 옵션으로 간단히 해결 가능합니다.

## clearAutomatically

* 해당 쿼리 실행 직 후, 영속성 컨텍스트를 clear 할 것인지를 지정하는 옵션입니다.

* default값 : false

### 문제 case

JPA에서는 영속성 컨텍스트에 있는 1차 캐시를 통해 엔티티를 캐싱하고, DB의 접근 횟수를 줄임으로써 성능 개선을 합니다. 1차 캐시는 @Id 값을 Key 값으로 엔티티 관리를 합니다. 그래서 findById 등을 통해 엔티티를 조회 했을 시 1차 캐시에 존재한다면 DB에 접근하지 않고 캐싱된 엔티티를 반환합니다.

그렇다면, 벌크 연산을 통해 데이터 변경 쿼리를 실행하고, 해당 엔티티를 조회를 하면 어떻게 될까요?

#### 테스트 시나리오

1. 엔티티 객체를 하나 생성합니다. (해당 엔티티는 **transient** 상태)

2. 객체의 **초기값**을 설정합니다.

3. 해당 엔티티의 Repository를 통해 해당 엔티티 객체를 save() 합니다. (해당 엔티티는 **persistent** 상태)

4. **벌크 연산**을 통해 기존에 설정한 초기값을 변경합니다. (**SQL 실행**)

5. **findById()**를 통해 해당 엔티티를 조회합니다. 

```java
@Entity
@Getter
@Setter
public class Article {
    @Id @GeneratedValue
    private Long id;
    private String titile;
    private String content;
}
```

```java
public interface ArticleRepository extends JpaRepository<Article, Long> {
    @Modifying
    @Query("UPDATE Article a SET a.title = ?2 WHERE a.id = ?1")
    int updateTitle(Long id, String title);
}
```

```java
@Autowired
private ArticleRepository articleRepository;

@Test
public void test() {
    Article article = new Article();
    article.setTitle("before");
    articleRepository.save(article);
    
    int result = articleRepository.updateTitle(1l, "after");
    assertThat(result).isEqualTo(1);
    
    System.out.println(articleRepository.findById(1L).get().getTitle());
}
```

의도했던 출력값은 "after"지만 테스트에서는 "before"가 출력 됩니다.

DB를 확인해보면 "after"로 정상 변경 되었습니다.

이는 **벌크 연산으로 업데이트된 엔티티가 영속성 컨텍스트에 캐싱되지 않았기 때문**입니다.

이를 해결하기 위해 `clearAutomatically` 옵션을 사용합니다.

```java
public interface ArticleRepository extends JpaRepository<Article, Long> {
    @Modifying(clearAutomatically = true)
    @Query("UPDATE Article a SET a.title = ?2 WHERE a.id = ?1")
    int updateTitle(Long id, String title);
}
```

`clearAutomatically` 옵션을 `true` 로 사용하면 벌크 연산을 수행시 영속성 컨텍스트를 clear하기 때문에, 이후 find 메소드를 사용 시 DB에서 다시 조회해 와서 영속성 컨텍스트에 캐싱 됩니다.

그로 인해 의도 했던대로 "after"를 출력하게 됩니다.