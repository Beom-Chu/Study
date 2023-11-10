# [PostgreSQL] 암복호화

###  확장 모듈 설치

```
create extension pgcrypto;
```

확장 모듈을 설치하면 암호화 관련 함수 사용 가능

* convert_to/convert_from : 문자열 변환/복원
* encode/decode : 16진수 인코딩/디코딩
* encrypt/decrypt : 암호화/복호화



### 암호화

```sql
select encode(encrypt(convert_to('암호화할문자열','utf8'),'암호화키','aes'),'hex');
```

> utf8로 변환한 후, 암호화키로 'aes'알고리즘을 사용하여 암호화한 후, 그 값을 16진수(hex)로 encoding



<BR>

### 복호화

```sql
select convert_from(decrypt(decode('위에서 암호화된문자열','hex'),'암호화키','aes'),'utf8');
```

> 암호화의 역순. 16진수값을 decoding한 후 암호화를 해제하고, 그 값을 다시 utf8에서 변환

