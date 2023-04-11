# [JPA] @LastModifiedDate를 ZonedDateTime타입으로 사용하기

`@LastModifiedDate` 어노테이션은 기본적으로 `java.util.Date` 또는 `java.time.LocalDateTime` 타입의 필드에 적용되지만 `ZonedDateTime` 타입에는 적용 되지 않습니다.

그렇기 때문에 `ZonedDateTime` 타입에 적용하기 위해서는 추가적인 작업이 필요합니다.





###  hibernate.annotations 라이브러리 활용 (@UpdateTimestamp)

1. `ZonedDateTime` 타입 필드에 `@UpdateTimestamp` 어노테이션을 적용합니다.

```java
javaCopy codeimport org.hibernate.annotations.UpdateTimestamp;

import javax.persistence.*;
import java.time.ZonedDateTime;

@Entity
@Table(name = "my_table")
public class MyEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "created_dt")
    private ZonedDateTime createdDt;

    @Column(name = "updated_dt")
    @UpdateTimestamp
    private ZonedDateTime updatedDt;

    // getters/setters
}
```

위 코드에서 `@UpdateTimestamp` 어노테이션은 해당 필드가 자동으로 업데이트됨을 나타내며, `ZonedDateTime` 타입 필드를 사용하고 있습니다. 

`@UpdateTimestamp` 어노테이션은 `hibernate-java8` 라이브러리에서 제공되는 어노테이션으로, `@LastModifiedDate` 어노테이션과 유사한 기능을 수행합니다.

위와 같이 `@UpdateTimestamp` 어노테이션을 사용하면 `updated_dt` 필드가 수정될 때마다 자동으로 업데이트됩니다. 또한 `ZonedDateTime` 타입의 필드이므로, 애플리케이션의 타임존 설정에 따라 시간이 자동으로 변환됩니다.

<br>

### @PreUpdate 활용

```java
import java.time.ZoneId;
import java.time.ZonedDateTime;
import javax.persistence.*;

@Entity
public class MyEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Column(name = "last_modified_date", nullable = false)
    private ZonedDateTime lastModifiedDate;

    @PrePersist
    protected void onCreate() {
        lastModifiedDate = ZonedDateTime.now(ZoneId.of("UTC"));
    }

    @PreUpdate
    protected void onUpdate() {
        lastModifiedDate = ZonedDateTime.now(ZoneId.of("UTC"));
    }

    // constructors, getters, setters, etc.
}
```

`@PrePersist`는 엔티티가 데이터베이스에 저장되기 전에 실행되며, `@PreUpdate`는 엔티티가 업데이트되기 전에 실행됩니다. 이렇게 하면 엔티티가 저장되거나 업데이트될 때마다 `lastModifiedDate` 필드가 업데이트됩니다.

또한, 이 코드에서는 `ZonedDateTime.now(ZoneId.of("UTC"))`를 사용하여 현재 UTC 시간을 가져오도록 설정했습니다. 이 방법을 사용하면 데이터베이스의 모든 서버와 클라이언트가 동일한 시간대를 사용하도록 보장할 수 있습니다.