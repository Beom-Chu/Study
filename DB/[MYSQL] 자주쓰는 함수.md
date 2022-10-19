# [MYSQL] 자주쓰는 함수

## 숫자 관련 함수

#### ABS(숫자) : 절대값 출력

```sql
select abs(-10); -- 10
```

#### CEILING(숫자) : 올림

```sql
select ceiling(10.1); -- 11
select ceiling(10.5); -- 11
select ceiling(10.8); -- 11
select ceiling(-10.1); -- -10
select ceiling(-10.5); -- -10
select ceiling(-10.8); -- -10
```

#### FLOOR(숫자) : 내림

```sql
select floor(10.1); -- 10
select floor(10.5); -- 10
select floor(10.8); -- 10
select floor(-10.1); -- -11
select floor(-10.5); -- -11
select floor(-10.8); -- -11
```

#### ROUND(숫자) : 반올림

```sql
select round(10.1); -- 10
select round(10.5); -- 11
select round(10.8); -- 11
select round(-10.1); -- -10
select round(-10.5); -- -11
select round(-10.8); -- -11
```

#### TRUNCATE(숫자, 자릿수) : 소수점 이하 자릿수에서 버림

```sql
select truncate(1234.1,3); -- 1234.1
select truncate(1234.5123,2); -- 1234.51
select truncate(1234.5123,-2); -- 1200
```

#### POW(X, Y) : X의 Y승

```sql
select pow(2,3); -- 8
```

#### MOD(분자, 분모) : 분자를 분모로 나눈 나머지 구하기(연산자%동일)

```sql
select mod(7,3); -- 1
select mod(7,4); -- 3
```

#### GREATEST(숫자1,숫자2,숫자3...) : 가장 큰 수 반환.

```sql
select greatest(7,4,9,10); -- 10
```

#### LEAST(숫자1,숫자2,숫자3...) : 가장 작은 수 반환.

```sql
select least(7,4,9,10); -- 4
```

#### INTERVAL(대상숫자,숫자1,숫자2,숫자3...) : 대상 숫자의 위치 반환.

숫자들의 크기를 비교해서 대상숫자가 정렬될 때 들어가야할 위치를 반환 한다. (0부터 시작)

숫자1,2,3..은 오름차순으로 정렬 되어야 한다.

```sql
select interval(3,1,2,4); -- 2
select interval(1,2,3,4); -- 0
```







http://egloos.zum.com/piccom/v/2866254