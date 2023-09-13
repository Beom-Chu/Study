# @SqlResultSetMapping

Jpa에서 제공하는 기능으로, 엔터티 클래스와 관련 없는 SQL 쿼리의 결과를 DTO나 사용자 지정 클래스에 매핑하는데 사용 됩니다.

주로 NativeQuery와 함께 사용 됩니다.

### 역할

* SQL 쿼리 결과 매핑
  * `@SqlResultSetMapping`을 사용하면 SQL 쿼리의 결과 집합을 엔터티 클래스가 아닌 자바 객체에 매핑할 수 있습니다. 이것은 주로 복잡한 쿼리 결과를 DTO나 사용자 지정 클래스에 매핑하는 데 사용됩니다.
* 엔터티와 관련 없는 결과 매핑
  * JPA에서는 주로 `@Entity` 어노테이션이 붙은 엔터티 클래스와 관련된 결과 매핑을 처리합니다. 그러나 `@@SqlResultSetMapping`을 사용하면 엔터티와 관련 없는 결과 집합을 매핑할 수 있습니다.
* 복수의 엔터티 또는 스칼라 값 매핑
  * 하나의 SQL쿼리에서 여러 엔터티나 스칼라 값(*문자열, 숫자 등*)을 매핑할 수 있습니다. 이를 통해 하나의 쿼리 결과 집합에 다양한 종류의 데이터를 추출하고 매핑할 수 있습니다.

<br>

### 샘플 코드

```java
@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String author;
    private double price;

    // Getter와 Setter 메서드
}


@SqlResultSetMapping(
    name = "BookDTOMapping",
    classes = {
        @ConstructorResult(
            targetClass = BookDTO.class,
            columns = {
                @ColumnResult(name = "title", type = String.class),
                @ColumnResult(name = "author", type = String.class),
                @ColumnResult(name = "price", type = Double.class)
            }
        )
    }
)
@NamedNativeQuery(
    name = "getBooksByAuthor",
    query = "SELECT title, author, price FROM Book WHERE author = :author",
    resultSetMapping = "BookDTOMapping"
)
@Entity
public class BookDTO {
    private String title;
    private String author;
    private double price;

    public BookDTO(String title, String author, double price) {
        this.title = title;
        this.author = author;
        this.price = price;
    }

    // Getter와 Setter 메서드
}
```

의 코드에서 `@SqlResultSetMapping` 어노테이션은 `BookDTO` 클래스에 대한 매핑을 정의하고, `@NamedNativeQuery` 어노테이션을 사용하여 `BookDTO`에 대한 네이티브 SQL 쿼리를 정의하고 매핑 정보를 제공합니다. 그런 다음 `BookDTO` 객체를 사용하여 특정 작가의 책 목록을 조회할 수 있습니다.