# MongoDB 쿼리

### SQL과 비교 정리

| MySQL 용어 | MongoDB 용어          |
| ---------- | --------------------- |
| database   | database              |
| table      | collection            |
| index      | index                 |
| row        | BSON document         |
| column     | BSON field            |
| join       | embedding and linking |

| SQL 문장                         | MongoDB 문장                  |
| -------------------------------- | ----------------------------- |
| CREATE TALE USERS (a int, b int) | db.createCollection("mycoll") |
| INSERT INTO USERS VALUES (3,5)                 | db.users.insert({a:3, b:5})                          |
| SELECT a, b FROM USERS                         | db.users.find({}, {a:1, b:1})                        |
| SELECT * FROM users                            | db.users.find()                                      |
| SELECT * FROM users WHERE age=33               | db.users.find({age:33})                              |
| SELECT a,b FROM users WHERE age=33             | db.users.find({age:33}, {a:1,b:1})                   |
| SELECT * FROM users WHERE age=33 ORDER BY name | db.users.find({age:33}).sort({name:1})               |
| SELECT * FROM users WHERE age>33               | db.users.find({'age':{$gt:33}})                      |
| SELECT * FROM users WHERE age<33               | db.users.find({'age':{$lt:33}})                      |
| SELECT * FROM users WHERE name LIKE"%Joe%"     | db.users.find({name:/Joe/})                          |
| SELECT * FROM users WHERE name LIKE "Joe%"     | db.users.find({name:/^Joe/})                         |
| SELECT * FROM users WHERE age>33 AND age<=40   | db.users.find({'age':{$gt:33,$lte:40}})              |
| SELECT * FROM users ORDER BY name DESC         | db.users.find().sort({name:-1})                      |
| SELECT * FROM users WHERE a=1 and b='q'        | db.users.find({a:1,b:'q'})                           |
| SELECT * FROM users LIMIT 10 SKIP 20           | SELECT * FROM users LIMIT 10 SKIP 20                 |
| SELECT * FROM users WHERE a=1 or b=2           | db.users.find( { $or : [ { a : 1 } , { b : 2 } ] } ) |
| SELECT * FROM users LIMIT 1                    | db.users.findOne()                                   |
| SELECT DISTINCT last_name FROM users           | db.users.distinct('last_name')                       |
| SELECT COUNT(*y) FROM users                    | db.users.count()                                     |
| SELECT COUNT(*y) FROM users where AGE > 30 | db.users.find({age: {'$gt': 30}}).count()       |
| SELECT COUNT(AGE) from users               | db.users.find({age: {'$exists': true}}).count() |
| CREATE INDEX myindexname ON users(name)         | db.users.ensureIndex({name:1})                      |
| CREATE INDEX myindexname ON users(name,ts DESC) | db.users.ensureIndex({name:1,ts:-1})                |
| EXPLAIN SELECT * FROM users WHERE z=3           | db.users.find({z:3}).explain()                      |
| UPDATE users SET a=1 WHERE b='q'                | db.users.update({b:'q'}, {$set:{a:1}}, false, true) |
| UPDATE users SET a=a+2 WHERE b='q'              | db.users.update({b:'q'}, {$inc:{a:2}}, false, true) |
| DELETE FROM users WHERE z="abc"                 | db.users.remove({z:'abc'});                         |

### 비교 연산자

| operator | 설명                                                   |
| -------- | ------------------------------------------------------ |
| $eq      | (equals) 주어진 값과 일치하는 값                       |
| $gt      | (greater than) 주어진 값보다 큰 값                     |
| $gte     | (greather than or equals) 주어진 값보다 크거나 같은 값 |
| $lt      | (less than) 주어진 값보다 작은 값                      |
| $lte     | (less than or equals) 주어진 값보다 작거나 같은 값     |
| $ne      | (not equal) 주어진 값과 일치하지 않는 값               |
| $in      | 주어진 배열 안에 속하는 값                             |
| $nin     | 주어빈 배열 안에 속하지 않는 값                        |

##### 샘플

```json
db.컬렉션이름.find({'details.manufacturer': 'Acme', tag: {$ne: "gardening"} })
db.컬렉션이름.find({'details.color': {$in: ['blue','Green']}})
```

### 논리 연산자

| operator | 설명                                   |
| -------- | -------------------------------------- |
| $or      | 주어진 조건중 하나라도 true 일 때 true |
| $and     | 주어진 모든 조건이 true 일 때 true     |
| $not     | 주어진 조건이 false 일 때 true         |
| $nor     | 주어진 모든 조건이 false 일때 true     |
| $exists  | 특정 키를 가지고 있는지 질의           |

##### 샘플

```json
db.컬렉션이름.find({
     '$or': [
           {'details.color': 'blue'},
           {'details.manufacturer': 'Acme'}
         ]
})
db.컬렉션이름.find({
      $and: [
           {tags: {$in: ['git','holiday']}},
           {tags: {$in: ['gardening','landscaping']}}
        ]
})
db.컬렉션이름.find({'details.color': {$exists: false}})
db.컬렉션이름.find({'details.color': {$exists: true}})
```

### 배열

| operator   | 설명                                                       |
| ---------- | ---------------------------------------------------------- |
| $elemMatch | 제공된 모든 조건이 동일한 하위 도큐먼트에 있는 경우 일치   |
| $size      | 배열 하위 도큐먼트의 크기가 제공된 리터럴 값과 같으면 일치 |

##### 샘플

```json
db.컬렉션이름.find({
   'addresses': {
       '$elemMatch': {
           'name': 'home',
           'state': 'NY'
          }
     }
})
db.컬렉션이름.find({'addresses': {$size: 3}})
```

### 정규식

| operator | 설명                                |
| -------- | ----------------------------------- |
| $regex   | 요소를 제공된 정규 표현식과 맞춰봄. |

*prefix 타입의 쿼리를 제외하고는 정규 표현식 쿼리는 인덱스를 사용할 수 없고, 대부분의 선택자보다 실행시간이 오래 걸린다.*

##### 샘플

```json
db.컬렉션이름.find({
    'text': {
              '$regex': "best|worst"
              '$options': "i"}
   })
```

> $options : 대소문자 구분 없이 검색

