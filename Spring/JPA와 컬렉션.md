# JPA와 컬렉션

## 컬렉션

* JPA에서 컬렉션 사용 예시
  * @OneToMany, @ManyToMany를 사용해서 관계를 매핑할 때
  * @ElementCollection을 사용해서 값 타입을 하나 이상 보관할 때
* 자바 컬렉션 인터페이스 특징
  * **Collection** : 자바가 제공하는 최상위 컬렉션. 하이버네이트는 중복을 허용하고 순서를 보장하지 않는다고 가정.
  * **Set** : 중복을 허용하지 않고 순서를 보장하는 컬렉션
  * **List** : 순서를 보장하고 중복을 허용하는 컬렉션
  * **Map** : key, value 구조로 되어 있는 컬렉션

### JPA와 컬렉션

하이버네이트는 엔티티를 영속 상태로 만들 때 컬렉션 필드를 하이버네이트에서 준비한 컬렉션으로 감싸쇼 사용

```java
@Entity
public class Team {
		@Id
		private String id;
		
		@OneToMany
		@JoinColumn
		private Collection<Member> members = new ArrayList<Member>();
		...
}
```

```java
//실행 코드
Team team = new Team();

System.out.println("before persist = "+ team.getMembers().getClass());
em.persist(team);
System.out.println("after persist = " + team.getMebers().getClass());

//출력 결과
before persist = class java.util.ArrayList 
after persist = class org.hibernate.collection.internal.PersistentBag
```

* 하이버네이트는 컬렉션을 효율적으로 관리하기 위해 엔티티를 영속 상태로 만들 때 원본 컬렉션을 감싸고 있는 내장 컬렉션을 생성해서 사용하도록 참조를 변경함
* 하이버네이트가 제공하는 내장 컬렉션은 원본 컬렉션을 감싸고 있어서 래퍼 컬렉션 이라고도 부름
* 이런 특징 때문에 하이버네이트에서는 컬렉션 사용시 즉시 초기화를 권장

```java
Collection<Member> members = new ArrayList<Member>();
```

##### 하이버네이트 내장 컬렉션과 특징

| 컬렉션 인터페이스   | 내장 컬렉션    | 중복 허용 | 순서 보관 |
| ------------------- | -------------- | --------- | --------- |
| Collection, List    | PersistentBag  | O         | X         |
| Set                 | PersistentSet  | X         | X         |
| List + @OrderColumn | PersistentList | O         | O         |

### Collection, List

* 중복을 허용하고 PersistentBag 래퍼 컬렉션 사용
* ArrayList로 초기화
* 객체를 추가하는 add() 메소드는 항상 true 반환, 찾거나 삭제시 equals() 메소드 사용

```java
List<Comment> comments = new ArrayList<Comment>();
...

boolean result = comments.add(data); //단순 추가, 항상 true

comments.contains(comment); //equals 비교
comments.remove(comment); //equals 비교
```

* **엔티티를 추가할 때 중복된 엔티티가 있는지 비교하지 않고 단순 저장한다. 따라서 엔티티를 추가해도 지연 로딩된 컬렉션을 초기화 하지 않는다.**

### Set

* 중복을 허용하지 않고 PersistentSet 래퍼 컬렉션 사용
* HashSet으로 초기화
* add() 메소드를 사용할 때 마다 equals() 메소드로 같은 객체가 있는지 비교
  * 같은 객체가 없으면 추가 후  true, 있으면 실패하며 false 를 반환
  * 해시 알고리즘을 사용하므로 hashcode()도 함께 사용해서 비교

```java
Set<Comment> comments = new HashSet<Comment>();
...

boolean result = comments.add(data); //hashcode + equals 비교
comments.contains(comment); //hashcode + equals 비교
comments.remove(comment); //hashcode + equals 비교
```

* **Set은 추가할 때 중복 엔티티를 비교한다. 따라서 엔티티를 추가할 때 지연 로딩된 컬렉션을 초기화 한다.**

### List + @OrderColumn

* List 인터페이스에 @OrderColumn을 추가하면 순서가 있는 특수한 컬렉션으로 인식
* 데이터베이스에 순서값을 저장해서 조회할 때 사용한다는 의미
* 하이버네이트 내부 컬렉션 PersistentList 를 사용

* 데이터베이스에 순서값도 함께 관리

##### @OrderColumn 단점

- @OrderColumn을 Board 엔티티에 매핑하므로 Comment는 POSITION의 값을 알 수 없음. 그래서 Board.comment의 위치 값을 사용해서 POSITION의 값을 UPDATE하는 SQL이 추가 발생

- List를 변경하면 연관된 많은 위치 값을 변경, 가운데 값을 줄이면 POSITION의 값을 각각 하나씩 줄이게 되므로 많은 UPDATE문이 발생

- POSITION 값이 없으면 조회한 List에는 null이 보관. 강제로 삭제 후 POSITION의 값을 수정하지 않으면 [0, 2, 3]이 되어 1값이 없는 현상 발생. 그로 인해 NullPointException이 발생

**이런 단점들 때문에 실무에서 잘 사용 하지 않음**

### @Orderby

* 데이터베이스의 ORDER BY 절을 사용해서 컬렉션을 정렬
* 순서용 컬럼을 매핑하지 않아도 가능
* 모든 컬렉션에서 사용 가능

```java
@Entity
public class Team {
		@Id @GeneratedValue
		private Long id;
		private String name;

		@OneToMany(mappedBy = "team")
		@OrderBy("username desc, id asc")
		private Set<Member> members = new HashSet<Member>();
		...
}

@Entity
public class Member {
		@Id @GeneratedValue
		private Long id;

		@Column(name = "MEMBER_NAME")
		private String username;

		@ManyToOne
		private Team team;
		...
}
```

* @OrderBy의 값은 JPQL의 order by 처럼 **엔티티 필드를 대상**으로 적용

```java
//초기화
Team findTeam = em.find(Team.class, team.getId());
findTeam.getMembers().size(); //초기화

//Team.members를 초기화 할 때 SQL 실행결과
SELECT M.*
FROM 
		MEMBER M
WHERE
		M.TEAM_ID=?
ORDER BY
		M.MEMBER_NAME DESC,
		M.ID ASC
```

*SQL에 ORDER BY 가 사용된 걸 확인 가능.*