# [MYSQL] EXISTS vs IN

## EXISTS 연산자

서브쿼리가 반환하는 결과값이 있는지 조사.

단지 반환된 행이 있는지 없는지만 보고 값이 있으면 참, 없으면 거짓을 반환.

* 한 테이블이 다른 테이블과 외래키(FK)와 같은 관계가 있을 때 유용
* 조건에 해당하는 Row의 존재 유무와 이후 더 수행하지 않음(지연 평가 원리라 성능이 좋음)
* 일반적으로 SELECT절까지 가지 않기 때문에 **IN 에 비해 속도나 성능면에서 유리**
* 쿼리 순서 : 메인 쿼리 -> EXISTS 쿼리

<br>

**주문이 존재하는 사용자 조회**

```sql
SELECT * FROM customers 
WHERE EXISTS (
  SELECT * 
  FROM orders 
  WHERE orders.c_id = customers.c_id
);
```

> 서브쿼리 내에서 부모쿼리의 필드를 사용할 수 있다.
>
> 이처럼 서브쿼리에서 메인쿼리의 테이블을 활용하는 형태를 **연관 서브쿼리** 라고 한다.
>
> EXISTS 다음으로 오는 SELECT 절에는 *, 1, 'abc' 등 아무거나 입력해도 상관없다.
>
> EXISTS는 조건이 맞는지에 대한 True / False 만 확인하기 때문.

## EXISTS vs IN 연산자 차이

EXISTS, IN 둘다 WHERE절에서 사용되며, 조건에 따라 데이터를 걸러내는 결과를 조회할때 사용.

#### 서브쿼리 IN 연산자

* 집합 내부에 값이 존재하는지 여부 확인
* 조건에 해당하는 ROW의 컬럼 비교하여 체크
* SELECT절에서 조회한 컬럼 값으로 비교하여 **EXISTS에 비해 성능 떨어짐**

* 쿼리 순서 : IN 쿼리 -> 메인 쿼리

```sql
SELECT * 
FROM customers 
WHERE c_id IN ( SELECT DISTINCT c_id FROM orders);
```