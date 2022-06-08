# Base64 인코딩,디코딩



## Linux & MacOS

#### ~~base64 인코딩~~

```sh
$ echo '김범수' | base64
6rmA67KU7IiYCg==
```

#### base64 디코딩

```sh
$ echo 6rmA67KU7IiYCg== | base64 -d
김범수

```

### 위 방식은 인코딩시 개행문자를 포함하게 되므로 `-n` 옵션을 사용하자.

#### base64 인코딩

```sh
$ echo -n '김범수' | base64
6rmA67KU7IiY
```

#### base64 디코딩은 동일



## Windows

#### base64 인코딩

1. 인코딩할 텍스트를 입력한 txt파일 생성

2. CLI 입력

   ```sh
   PS D:\> certutil -encode test.txt out.b64
   입력 길이 = 9
   출력 길이 = 70
   CertUtil: -encode 명령이 성공적으로 완료되었습니다.
   ```

3. 결과 확인 (out.b64 파일 open)

   ```
   -----BEGIN CERTIFICATE-----
   6rmA67KU7IiY
   -----END CERTIFICATE-----
   ```

#### base64 디코딩

1. 디코딩할 텍스트를 입력한 txt파일 생성

2. CLI 입력

   ```sh
   PS D:\> certutil -decode test2.txt out2.b64
   입력 길이 = 12
   출력 길이 = 9
   CertUtil: -decode 명령이 성공적으로 완료되었습니다.
   ```

3. 결과 확인 (out2.b64 파일 open)

   ```
   김범수
   ```

   

