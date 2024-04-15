# [MariaDB]명령어

## TimeZone 설정 및 조회

```sql
-- TimeZone 설정
SET time_zone = '+09:00';
SET time_zone = 'Asia/Seoul'

-- TimeZone 조
SELECT @@session.time_zone;
```

## 프로세스 정보 조회 및 종료

```sql
show processlist;

kill 40; -- processlist에서 조회되는 Id
```

## DB 시스템 변수 조회 및 변경

```sql
show global variables like '%';
show variables like '%';

set global wait_timeout = 28800;
set wait_timeout = 28800;
```

## DB 상태값 조회

```sql
show global status like '%';
show status;
```
