# JPA 성능 최적화 - N+1 문제

```java
@Entity
class Member{
    @Id @GeneratedValue
    private Long id;

    @OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
    private List<Order> orders = new ArrayList<>();
}

@Entity
class Order{
    @Id @GeneratedValue
    private Long id;

    @ManyToOne
    private Member member;
}
```

위와 같은 구조일 때 Member 전체 조회쿼리가 한번 실행되고,

Member의 수만큼 Order를 조회하는 쿼리가 실행되는 상황

```sql
SELECT * FROM Member; -- if result is 5
SELECT * FROM Order_ WHERE MEMBER_ID = 1;
SELECT * FROM Order_ WHERE MEMBER_ID = 2;
SELECT * FROM Order_ WHERE MEMBER_ID = 3;
SELECT * FROM Order_ WHERE MEMBER_ID = 4;
SELECT * FROM Order_ WHERE MEMBER_ID = 5;
```

<br>

## 해결법

#### 패치조인

SQL 조인을 이용해서 엔티티가 함께 조회하므로 N+1 문제가 발생하지 않음

```sql
SELECT m FROM Member m JOIN FETCH m.orders
```

<br>

#### 하이버네이트 @BatchSize

하이버네이트가 제공하는 `org.hibernate.annotations.BatchSize` 어노테이션을 이용하면 연관된 엔티티를 조회할 때 지정된 size 만큼 SQL의 IN절을 사용해서 조회.

```java
@Entity
class Member{
    @Id @GeneratedValue
    private Long id;

    @org.hibernate.annotations.BatchSize(size = 5)
    @OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
    private List<Order> orders = new ArrayList<>();
}
```

```sql
SELECT * FROM
ORDER_ WHERE MEMBER_ID IN(
    ?, ?, ?, ?, ?
)
```

<br>

#### 하이버네이트 @Fetch(FetchMode.SUBSELECT)

연관된 데이터를 조회할 때 서브 쿼리를 사용.

```java
@Entity
class Member{
    @Id @GeneratedValue
    private Long id;

    @org.hibernate.annotations.Fetch(FetchMode.SUBSELECT)
    @OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
    private List<Order> orders = new ArrayList<>();
}
```

```sql
SELECT * FROM Member;
SELECT * FROM Order_
    WHERE MEMBER_ID IN(
        SELECT ID
        FROM Member
    )
```

<br>

#### @EntityGraph 사용

Repository 인터페이스에서 @EntityGraph를 사용해서 Fetch 조인을 사용하도록 함

```java
public interface MemberRepository extends JpaRepository<Member, Long>, MemberRepositoryCustom {
    @Override
    @EntityGraph(attributePaths = {"order"})
    List<Member> findAll();
}
```

```sql
SELECT * FROM Member m 
	LEFT OUTER JOIN Order o 
	ON m.team_id = o.team_id
```

<br><br><br><br><br>

참조 : https://joont92.github.io/jpa/JPA-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/

