# String

* 문자열을 다루는 Class

* 인스턴스를 한 번 생성하면 읽기만 가능하고 변경할 수 없는 불변객체
* String은 실제 데이터를 비교하는 equals 메서드가 재정의 되어있음

* 선언시 new 연산자 사용방식과 리터럴 방식에 따라 메모리에 할당되는 방식이 다름
  * new 연산자 : Heap 영역
  * 리터럴 : Heap 영역의 String Constant Pool

```java
String str1 = "madplay";
String str2 = "madplay";
String str3 = new String("madplay");
String str4 = new String("madplay");
```



![java heap space](https://madplay.github.io/img/post/2018-05-12-java-string-literal-vs-string-object-1.png)

그러므로 리터럴로 선언한 경우 이중등호(==)로 true 값이 반환됨. 