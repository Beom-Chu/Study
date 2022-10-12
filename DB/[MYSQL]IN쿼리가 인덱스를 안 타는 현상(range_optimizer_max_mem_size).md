# [MYSQL] IN쿼리가 인덱스를 안 타는 현상(range_optimizer_max_mem_size)

* 해당 버전 : Mysql5.7 

* 쿼리 IN절에 항목 수가 많은 경우 인덱스가 제대로 타지 않는 경우 발생.

* 쿼리 자체를 메모리에 올리는 것 또한 메모리 제한이 존재 하는데 그 제한 옵션이 `range_optimizer_max_mem_size`

참고링크 : https://dev.mysql.com/doc/refman/5.7/en/range-optimization.html#range-optimization-memory-use

**해당 옵션을 0으로 설정해주면 제한이 없어진다 **

만약 0이 아닌 값이 셋팅되어 있다면, **그 값을 초과했을 경우 옵티마이져는 풀스캔 혹은 이외의 인덱스**를 태우기도 한다.



IN쿼리는 우리 눈에는 함축적으로 보이지만, 사실 상은 OR쿼리가 조합된 것이므로 메모리에 특이나 주의한다.

```sql
SELECT COUNT(*) FROM t WHERE a=1 OR a=2 OR a=3 ... OR a=N;
```

하나의 OR당 대략 230byte를 차지한다.

 

그러므로 230byte * N이 이 쿼리의 용량이 되는데, 이 용량이 `range_optimizer_max_mem_size`보다 크다면 인덱스를 타지 못한다.

(참고로 AND조건은 하나당 대략 125byte를 차지한다.)

 

그렇다면 다음과 같은 쿼리의 용량은 어떨까?

```sql
SELECT COUNT(*) FROM t WHERE a IN (1, 2, ..., N) AND b IN (1, 2, ..., M)
```

위에서 설명한 것을 가지고 계산하면 230byte * N * M이 될것이다.

 

여기서 주의할점은 Mysql5.7.11 이전 버전은 OR쿼리가 230byte가 아닌 대략 700byte라고 한다.

 