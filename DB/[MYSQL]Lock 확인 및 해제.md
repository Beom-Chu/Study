# [MYSQL] Lock 확인 및 해제

### Lock Table 확인

```sql
SQL> SHOW FULL PROCESSLIST;

Id     User  Host             db   Command   Time   State                        
507390 dev   127.0.0.1:60635 dev   Query     1000   Waiting for table metadata lock
```

State 컬럼으로 Lock 상태 확인 가능합니다.



### Lock 해제

```sql
SQL> KILL 507390;
```

Lock이 발생한 ID값을 Kill로 종료함으로써 Lock 해제가 가능합니다.