# [MariaDB] 암복호화(AES_ENCRYPT,AES_DECRYPT)

### 암호화(AES_ENCRYPT)

> AES_ENCRYPT(암호화 대상 문자열, 키 문자열)
>
> HEX 함수를 같이 사용해 줘야 문자가 안깨짐

```sql
SELECT HEX(AES_ENCRYPT('암호화테스트','passwordkey_abc')) FROM DUAL;
```

**result** : `53762EB95B0A994541FBBA85892535ECA5424A21A109975462BD874C395B4D80`

<BR>

### 복호화(AES_DECRYPT)

> AES_DECRYPT(복호화 대상 문자열, 키 문자열)
>
> UNHEX, CAST 함수 같이 사용

```sql
SELECT CAST(AES_DECRYPT(UNHEX('53762EB95B0A994541FBBA85892535ECA5424A21A109975462BD874C395B4D80'),'passwordkey_abc') AS CHAR) FROM DUAL; 
```

**result** : `암호화테스트`
