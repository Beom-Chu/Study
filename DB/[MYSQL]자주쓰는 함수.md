# [MYSQL]자주쓰는 함수

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

<br>

## 문자 관련 함수

#### ASCII('문자') : 문자의 아스키 코드값 반환

```sql
select ascii('a'); -- 97
select ascii('1'); -- 49
```

#### CONCAT('문자열1','문자열2','문자열3'...) : 문자열을 연결

```sql
select concat('ab','c','def'); -- abcdef
```

#### INSERT('문자열1',시작위치,길이,문자열2) : 문자열1의 시작위치부터 길이 만큼의 문자열 대신에 문자열2로 변경

```sql
select insert('MySql web study','7','3','offline'); -- MySql offline study
```

#### REPLACE('문자열1','문자열2','문자열3') : 문자열1에서 문자열2를 문자열3으로 변경

```sql
select replace('MySql web study','web','offline'); -- MySql offline study
```

#### INSTR('문자열','찾는문자열') : 문자열 중 찾는 문자열의 위치값을 반환

```sql
select instr('MySql web study','web'); -- 7
```

#### LEFT('문자열',개수) : 문자열 중 왼쪽부터 개수만큼 반환

```sql
select left('MySql web study',5); -- MySql
```

#### RIGHT('문자열',개수) : 문자열 중 오른쪽부터 개수만큼 반환

```sql
select right('MySql web study',5); -- study
```

#### MID('문자열', 시작위치, 개수) : 문자열 중 시작위치부터 개수만큼 반환

```sql
select mid('MySql web study',7,3); -- web
```

#### SUBSTRING : MID()와 동일

#### LTRIM('문자열') : 왼쪽 공백을 제거

#### RTRIM('문자열') : 오른쪽 공백을 제거

#### TRIM('문자열') : 양쪽 공백을 제거

#### LCASE('문자열') or LOWER('문자열') : 소문자로 변경

#### UCASE('문자열') or UPPER('문자열') : 대문자로 변경

#### REVERSE('문자열') : 문자열을 반대로 나열

```sql
select reverse('MySql web study'); -- yduts bew lqSyM
```

<br>

## 논리 관련 함수

#### IF(논리식,참일때 값, 거짓일때 값) : 논리식이 참일때와 거짓일때 지정한 값을 반환

```sql
select 
name, price,
if(price > 2000 ,'비싸다', '싸다') as '가격'
from (
    select '사과' as name, 3000 as price union all
    select '배' as name, 1000 as price union all
    select '귤' as name, 2000 as price 
) fruit
```

| name | price | 가격   |
| ---- | ----- | ------ |
| 사과 | 3000  | 비싸다 |
| 배   | 1000  | 싸다   |
| 귤   | 2000  | 싸다   |

#### IFNULL(값1, 값2) : 값1이 NULL이면 값2를 반환, 아니면 값1 반환

```sql
select 
name, price,
ifnull(price ,'가격 모름') as '가격'
from (
	select '사과' as name, 3000 as price union all
	select '배' as name, null as price union all
	select '귤' as name, 2000 as price 
) fruit
```

| name | price | 가격      |
| ---- | ----- | --------- |
| 사과 | 3000  | 3000      |
| 배   | null  | 가격 모름 |
| 귤   | 2000  | 2000      |

<br>

## 날짜 관련 함수

#### NOW() or SYSDATE() or CURRENT_TIMESTAMP() : 현재 날짜 시간 출력

```sql
select now(), now() + 0;
```

| now()               | now() + 0      |
| ------------------- | -------------- |
| 2022-10-20 10:17:49 | 20221020101749 |

기준 차이는 존재 => [차이점 보기]((./[MYSQL]NOW(),SYSDATE(),CURRENT_TIMESTAMP()차이)

#### CURDATE() or CURRENT_DATE() : 현재 날짜 출력

#### CURTIME() or CURRENT_TIME() : 현재 시간 출력

#### DATE_ADD(날짜,INTERVAL 기준값) : 날짜에 기준값만큼 더하기

> 기준값 : YEAR, MONTH, DAY, HOUR, MINUTE, SECOND

```sql
select now(), date_add(now(), interval 2 day) as date_add;
```

#### DATE_SUB(날짜,INTERVAL 기준값) : 날짜에 기준값만큼 빼기 

DATE_ADD()와 같은 기준값 사용

#### YEAR(날짜) : 날짜의 연도 반환

#### MONTH(날짜) : 날짜의 월 반환

#### MONTHNAME(날짜) : 날짜의 월을 영어로 반환

#### DAYNAME(날짜) : 날짜의 요일을 영어로 반환

#### DAYOFMONTH(날짜) : 날짜의 월별 일자 출력

#### DAYOFWEEK(날짜) : 날짜의 요일을 숫자로 출력 (일(1), 월(2), 화(3) ... 토(7))

#### WEEKDAY(날짜) : 날짜의 요일을 숫자로 출력 (일(6), 월(0), 화(1) ... 토(5))

#### DAYOFYEAR(날짜) : 일년 기준으로 현재의 일자 수 반환

#### WEEK(날짜) : 일년 중 몇번째 주인지 반환

#### DATE_FORMAT(날짜,형식) : 형식에 맞게 날짜 출력

```sql
SELECT DATE_FORMAT('2020-04-05', '%W %M %Y'); -- Sunday April 2020
```

| **형식** | **구분**         | **표시형식**                                                 |
| -------- | ---------------- | ------------------------------------------------------------ |
| %Y       | 연               | 4자리 연도                                                   |
| %y       | 연               | 2자리 연도                                                   |
| %m       | 월               | 2자리 (00-12)                                                |
| %c       | 월               | 1자리, 10보다 작을경우 (1-12)                                |
| %M       | 월               | 이름(January, February…)                                     |
| %b       | 월               | 줄인 이름(Jan, Feb…)                                         |
| %d       | 일               | 2자리 (00-31)                                                |
| %e       | 일               | 1자리, 10보다 작을경우 (0-31)                                |
| %D       | 일               | 1st, 2nd…                                                    |
| %H       | 시               | 24시간 형식 (00-23)                                          |
| %h       | 시               | 12시간 형식 (01-12)                                          |
| %I       | 시               | 12시간 형식 (01-13)                                          |
| %k       | 시               | 24시간 형식, 10보다 작을경우 한자리 (0-23)                   |
| %l       | 시               | 12시간 형식, 10보다 작을경우 한자리 (1-12)                   |
| %i       | 분               | 2자리 (00-59)                                                |
| %S       | 초               | 2자리 (00-59)                                                |
| %s       | 초               | 2자리 (00-59)                                                |
| %f       | 마이크로초       | 100만분의 1초                                                |
| %p       | 오전/오후        | AM/PM                                                        |
| %T       | 시분초           | 24시간 형식 (hh:mm:ss)                                       |
| %r       | 시분초 오전/오후 | 12시간 형식 (hh:mm:ss AM/PM)                                 |
| %j       | 일               | 그해의 몇번째 일인지 표시 (001-366)                          |
| %w       | 일               | 그주의 몇번째 일인지 표시 (0=일요일, 6=토요일)               |
| %W       | 주               | 이름(Monday,Tuesday…)                                        |
| %a       | 주               | 줄인 이름(Mon,Tue…)                                          |
| %U       | 주               | 그해의 몇번째 주인지 표시 (00-53) 일요일이 주의 첫번째일     |
| %u       | 주               | 그해의 몇번째 주인지 표시 (00-54) 월요일이 주의 첫번째일     |
| %X       | 연               | 그주가 시작된 해을 표시, %V와 같이 사용                      |
| %x       | 연               | 그주가 시작된 해을 표시, %v와 같이 사용                      |
| %V       | 주               | 그주가 시작된 해의 몇번째 주인지 표시 (01-53) 일요일이 주의 첫번째일 %X 와 함께사용 |
| %v       | 주               | 그주가 시작된 해의 몇번째 주인지 표시 (01-53) 월요일이 주의 첫번째일 %x 와 함께사용 |

<br>

<br>

http://egloos.zum.com/piccom/v/2866254