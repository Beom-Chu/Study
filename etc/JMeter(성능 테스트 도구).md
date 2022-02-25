# JMeter(성능 테스트 도구)

기능의 부하테스트 및 측정을 지원하는 Java Application 도구

<br>

### 메뉴 한글 설정 가능

`Options` -> `Choose Language` -> `Korean`  

<br>

**툴을 다시 실행시키면 다시 영문 메뉴로 변경 되므로 설정파일 수정**!

`jmeter폴더/bin/jmeter.properties`  파일 텍스트 편집

```properties
#Preferred GUI language. Comment out to use the JVM default locale's language.
#language=en  <- 이부분 아래 처럼 변경
language=ko
```

<br>

### View Results Tree 의 응답값 한글 깨짐 현상 해결

`jmeter폴더/bin/jmeter.properties`  파일 텍스트 편집

```properties
# The encoding to be used if none is provided (default ISO-8859-1)
# sampleresult.default.encoding=ISO-8859-1 <- 이부분 아래처럼 변경
sampleresult.default.encoding=UTF-8
```

<br><br>

### 사용법 reference

https://medium.com/@bluewings/%EB%B6%80%ED%95%98%ED%85%8C%EC%8A%A4%ED%8A%B8-jmeter-%EB%9E%80-6299ecc2f7b1

https://zz1-hyunn.tistory.com/48

https://velog.io/@ehdrms2034/%EC%84%B1%EB%8A%A5-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%8F%84%EA%B5%AC-Apache-Jmeter-%EC%84%A4%EC%B9%98%EB%B6%80%ED%84%B0-%EA%B0%84%EB%8B%A8%ED%95%9C-%EC%82%AC%EC%9A%A9%EA%B9%8C%EC%A7%80

https://huistorage.tistory.com/84

https://ddanx2.tistory.com/117?category=807160