# StringJoiner

문자열의 중간에 특정 문자를 넣으면서 합쳐야하는 경우 사용.



#### StringBuilder vs StringJoiner 사용 비교

```java
int[] number = {1,2,3,4};
StringBuilder sb = new StringBuilder();

for(int n : number) {
    sb.append(n).append(",");
}
sb.deleteCharAt(sb.length()-1);

System.out.println(sb.toString()); //1,2,3,4
```

```java
int[] number = {1,2,3,4};
StringJoiner sj = new StringJoiner(",");

for(int n : number) {
    sj.add(String.valueOf(n));
}

System.out.println(sj.toString()); //1,2,3,4
```

**StringJoiner 사용시 더욱 간결한 코드 작성이 가능**



#### 접두사(Prefix)와 접미사(Suffix)도 사용 가능

```java
int[] number = {1,2,3,4};
StringJoiner sj = new StringJoiner(",","[","]");

for(int n : number) {
    sj.add(String.valueOf(n));
}

System.out.println(sj.toString()); //[1,2,3,4]
```



#### Stream 사용 예

```java
int[] number = {1,2,3,4};

String str = Arrays.stream(number).mapToObj(i->String.valueOf(i)).collect(Collectors.joining(","));
System.out.println(str); //1,2,3,4

String str2 = Arrays.stream(number).mapToObj(i->String.valueOf(i)).collect(Collectors.joining(",","[","]"));
System.out.println(str2); //[1,2,3,4]
```



