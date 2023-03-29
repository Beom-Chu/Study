# DateTimeFormatter

날짜, 시간 객체를 처리하도록 도와주는 포맷터.

자주 사용하는 상수도 제공.

| 상수 | Example |
| ---- | ---- |
|  ISO_LOCAL_DATE|2023-03-29|
|ISO_OFFSET_DATE|2023-03-29+09:00|
|ISO_DATE|2023-03-29+09:00|
|ISO_LOCAL_TIME|11:04:58.4891677|
|ISO_OFFSET_TIME|11:04:58.4891677+09:00|
|ISO_TIME|11:04:58.4891677+09:00|
|ISO_LOCAL_DATE_TIME|2023-03-29T11:04:58.4891677|
|ISO_OFFSET_DATE_TIME|2023-03-29T11:04:58.4891677+09:00|
|ISO_ZONED_DATE_TIME|2023-03-29T11:04:58.4891677+09:00[Asia/Seoul]|
|ISO_DATE_TIME|2023-03-29T11:04:58.4891677+09:00[Asia/Seoul]|
|ISO_ORDINAL_DATE|2023-088+09:00|
|ISO_WEEK_DATE|2023-W13-3+09:00|
|ISO_INSTANT|2023-03-29T02:04:58.489167700Z|
|BASIC_ISO_DATE|20230329+0900|
|RFC_1123_DATE_TIME  |Wed, 29 Mar 2023 11:04:58 +0900|

####  사용 예

``` java
ZonedDateTime now = ZonedDateTime.now();
String dateStr = now.format(DateTimeFormatter.ISO_LOCAL_DATE);
System.out.println(dateStr); //2023-03-29
```

##### 상수로 제공 되지 않는 포맷을 원하면 직접 패턴을 설정 한다.

```java
ZonedDateTime now = ZonedDateTime.now();
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm:ss.SSSZ");
String formattedString = now.format(formatter);
System.out.println(formattedString);
```

