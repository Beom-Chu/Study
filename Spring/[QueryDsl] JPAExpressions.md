# [QueryDsl] JPAExpressions

QueryDsl에서 서브쿼리가 필요할 때 사용한다.

### where 절에서 사용

```java
    @Test
    public void subQuery() throws Exception {
        QMember memberSub = new QMember("memberSub");

        List<Member> result = queryFactory
                .selectFrom(member)
                .where(member.age.eq( // 메인 쿼리: 멤버의 나이가 최댓값인 회원 조회
                        // 서브쿼리: member의 나이의 최댓값 조회
                        JPAExpressions
                                .select(memberSub.age.max())
                                .from(memberSub)
                ))
                .fetch();

        assertThat(result).extracting("age")
                .containsExactly(40);

    }
```

#### where 절에서 사용 -> SQL

```sql
select m1.*
  from member m1
 where m1.age = (select max(m2.age)
                   from member m2)
```

### select 절에서 사용

```java
import com.querydsl.jpa.JPAExpressions; // JPAExpression static import
 
    @Test
    public void selectSubQuery() throws Exception {
        QMember memberSub = new QMember("memberSub");

        List<Tuple> result = queryFactory
                .select(member.username,
                        // 서브 쿼리: 유저 평균 나이 조회, JPAExpressions static import
                        select(memberSub.age.avg())
                        .from(memberSub))
                .from(member)
                .fetch();

        for (Tuple tuple : result) {
            System.out.println("tuple = " + tuple);
        }
    }
```

#### select 절에서 사용 - SQL

```sql
select m1.username
      ,(select avg(m2.age) from member m2)
  from member m1
```


