# [PostgreSQL]Lock 확인 및 해제

### 현재 수행중인 SQL 전체 조회
```sql
select  datname, 
        pid, 
        usename, 
        application_name, 
        client_addr, 
        client_port, 
        backend_start, 
        query_start, 
        wait_event_type, 
        state, 
        backend_xmin
        query 
from pg_stat_activity;
```

|Column|Type|Comment|
|----|----|----|
|datid |oid |데이터베이스oid|
|datname |name |데이터베이스 이름|
|pid |integer |프로세스id|
|usesysid |oid |사용자고유번호|
|usename |name |사용자이름|
|application_name |text |응용프로그램이름|
|client_addr |inet |접속ip|
|client_hostname |text |접속 호스트 이름|
|client_port |integer |접속한 TCP 포트|
|backend_start |timestamp |서버 접속시간|
|xact_start |timestamp |트랜잭션 시작 시간|
|query_satrt |timestamp |쿼리 시작 시간|
|state_change |timestamp |state 마지막 변경 시간|
|waiting |boolean |대기중 = true|
|state |text |상태 정보 (하단 참조)|
|query |text |state=active 인 row에 대해 실행중인쿼리|

</br>

### LOCK 테이블 확인
```sql
SELECT  t.relname,
        l.locktype,
        page,
        virtualtransaction,
        pid,
        mode,
        granted
FROM pg_locks l,
	 pg_stat_all_tables t
WHERE l.relation = t.relid
```

</br>

### Process Kill
```sql
SELECT pg_cancel_backend([pid]);

SELECT pg_terminate_backend([pid]) FROM pg_stat_activity;
```
`pg_cancel_backend`는 해당 PID만 중지시키고, `pg_terminate_backend`는 해당 PID와 연계된 모든 상위 쿼리 프로세스를 종료시킨다. 
따라서 `pg_cancel_backend`로 해당 작업이 종료 되는지 먼저 체크한 뒤, 중지 되지 않는다면 `pg_terminate_backend`를 수행하여 해당 프로세스를 종료시키도록 한다.
