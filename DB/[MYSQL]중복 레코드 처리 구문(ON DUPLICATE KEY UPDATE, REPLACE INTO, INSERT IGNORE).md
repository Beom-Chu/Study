# [MYSQL]중복 레코드 처리 구문(ON DUPLICATE KEY UPDATE, REPLACE INTO, INSERT IGNORE)

MySql에서 Insert DML을 수행할때 중복된 Key를 가진 레코드를 등록하게 되는 경우 아래와 같은 오류를 만나게 됩니다.

```sql
-- Duplicate entry 'key_value' for key 'key_name'
Duplicate entry '312101208' for key 'PRIMARY'
```

중복된 key로 등록을 할 때 추가적인 동작을 위해서 사용 가능한 구문들이 있습니다.



## ON DUPLICATE KEY UPDATE

* `ON DUPLICATE KEY UPDATE` 구문은 삽입할 레코드의 **키 값이 이미 테이블에 존재하면 해당 레코드를 업데이트** 하고, 그렇지 않은 경우 삽입합니다.

##### 샘플 코드

```sql
INSERT INTO example_table 
(id, name, age)
VALUES 
(1, 'John', 25)
ON DUPLICATE KEY UPDATE name='John', age=25;
```

위 샘플에서는 id가 같은 레코드가 존재하면 name, age를 업데이트 하게 됩니다.

```sql
INSERT INTO example_table (id, name, age)
VALUES (1, 'John', 25)
ON DUPLICATE KEY UPDATE 
    name=VALUES(name),
    age=VALUES(age);
```

위 샘플처럼 values 함수를 사용할 수 도 있습니다.



## REPLACE INTO

* `REPLACE INTO` 구문은 기존의 레코드가 존재할 경우 **기존 레코드를 먼저 삭제**하고, 새로운 레코드를 삽입 합니다.

##### 샘플 코드

```sql
REPLACE INTO example_table (id, name, age)
VALUES (1, 'John', 25);
```

위 샘플에서는 id가 같은 레코드가 존재하면 삭제 후 새로운 레코드를 삽입합니다.



## INSERT IGNORE

* `INSERT IGNORE` 구문은 중복된 키값의 레코드가 존재할 경우 에러를 **무시하고 계속해서 다음 레코드를 추가**합니다.

##### 샘플 코드

```sql
INSERT IGNORE INTO example_table (id, name, age)
VALUES (1, 'John', 25);
```

위 샘플에서는 id가 같은 레코드가 존재하면 레코드가 삽입되지 않고 에러도 발생하지 않습니다.



## 정리

| 구문                    | ON DUPLICATE KEY UPDATE | REPLACE INTO | INSERT IGNORE |
| ----------------------- | ----------------------- | ------------ | ------------- |
| 중복 레코드 존재시 동작 | 업데이트                | 삭제 후 삽입 | 무시(삽입X)   |

