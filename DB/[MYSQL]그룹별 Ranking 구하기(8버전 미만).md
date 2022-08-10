# [MYSQL]그룹별 Ranking 구하기(8버전 미만)

#### 우선 8 버전의 경우 `rank()` 함수를 사용하면 편하다.

```sql
SELECT ID, AMOUNT
	,RANK() OVER (ORDER BY AMOUNT DESC) AS RANKING
FROM EX_CARD
```

* 그룹별 랭킹을 구하기

```sql
SELECT ID, TYPE, AMOUNT
	,RANK() OVER (PARITION BY TYPE ORDER BY AMOUNT DESC) AS RANKING
FROM EX_CARD
```

</br>

#### 여기서 부터는 8 미만 버전

#### 사용자 정의 변수 활용

```sql
SELECT ID, AMOUNT
	,(@RANK := @RANK + 1) AS RANKING
FROM DX_CARD A
	,(SELECT @RANK := 0) AS B
ORDER BY A.AMOUNT DESC;
```

* 그룹별로 랭킹 구하기

```sql
SELECT ID, TYPE, AMOUNT
	,CASE @GROUPING WHEN TYPE THEN @RANK := @RANK + 1 ELSE @RANK := 1 AS RANKING
	,@GROUPING := TYPE
FROM DX_CARD A
	,(SELECT @GROUPING := '', @RANK := 0) B
ORDER BY A.TYPE, A.AMOUNT DESC
```

> @GROUPING을 대입 해주는 부분이 RANKING 부분 보다 뒤에 있어야 함