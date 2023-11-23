# MYSQL vs PostgreSQL 함수 비교

|기능|MYSQL|PostgreSQL|
|----|----|----|
|문자 위치 찾기|instr|position|
|문자 합치기|concat| `||` |
|날짜 형식 지정|DATE_FORMAT|TO_CHAR|
|조건에 따른 값 반환|IF(), CASE WHEN|CASE WHEN|
|현재일자|curdate()|current_date|

</br>
## 사용 예
### 문자 위치 찾기
##### instr
```sql
-- 문자열 'Hello, World!'에서 'World'의 시작 위치 찾기
SELECT INSTR('Hello, World!', 'World');
-- 결과: 7
```
##### position
```sql
-- 문자열 'Hello, World!'에서 'World'의 시작 위치 찾기
SELECT POSITION('World' IN 'Hello, World!');
-- 결과: 7
```

</br>
### 문자 합치기
##### concat
```sql
-- 문자열 'Hello'와 'World'를 결합
SELECT CONCAT('Hello', ' ', 'World');
-- 결과: 'Hello World'
```
##### `||`
```sql
-- 문자열 'Hello'와 'World'를 결합
SELECT 'Hello' || ' ' || 'World';
-- 결과: 'Hello World'
```

</br>
### 날짜 형식 지정
##### DATE_FORMAT
```sql
-- 현재 날짜 및 시간을 'YYYY-MM-DD HH:MI:SS' 형식으로 포맷팅
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s') AS formatted_date;
-- 결과: '2023-01-01 12:34:56'
```
##### TO_CHAR
```sql
-- 현재 날짜 및 시간을 'YYYY-MM-DD HH:MI:SS' 형식으로 포맷팅
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD HH24:MI:SS') AS formatted_date;
-- 결과: '2023-01-01 12:34:56'
```

</br>
### 조건에 따른 값 반환
##### if
```sql
-- 값이 10보다 크면 'Big', 그렇지 않으면 'Small' 반환
SELECT IF(column_name > 10, 'Big', 'Small') AS result FROM table_name;
-- 결과: 'Big' 또는 'Small'
```
##### case when
```sql
-- 값이 10보다 크면 'Big', 그렇지 않으면 'Small' 반환
SELECT
  CASE
    WHEN column_name > 10 THEN 'Big'
    ELSE 'Small'
  END AS result
FROM table_name;
-- 결과: 'Big' 또는 'Small'
```

