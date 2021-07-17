# Java_CallByValue_CallByReference

### 함수의 호출 방식

* **Call by value(값에 의한 호출)** : 인자로 받은 **값을 복사**하여 처리
  * 장점 : 복사하여 처리하기 때문에 원래 값이 보존
  * 단점 : 복사를 하기 때문에 메모리 사용량이 증가
* **Call by reference(참조에 의한 호출)** : 인자로 받은 **주소를 참조**하여 직접 값에 영향을 줌
  * 장점 : 복사하지 않고 직접 참조하므로 빠름
  * 단점 : 직접 참조를 하기 때문에 원래의 값이 변경될 수 있음



### 자바는 Call by Value

객체를 메서드로 넘길 때 객체를 참조하는 지역변수의 **실제 주소를 넘기는것이 아니고**, 그 지역변수가 가리키고 있는 **힙 영역의 객체를 가리키는 새로운 지역변수를 생성**하고 전달.

그러므로 메서드에서 객체의 데이터를 수정하면 메서드가 종료된 후에도 수정된 데이터가 적용 되지만 객체에 new 연산자로 다시 초기화 하는 경우에는 메서드 호출시 사용된 객체에는 변화가 없음.

```java
public static void main(String[] args) {
    Dog aDog = new Dog("Max");
    Dog oldDog = aDog;

    // we pass the object to foo
    foo(aDog);
    // aDog variable is still pointing to the "Max" dog when foo(...) returns
    aDog.getName().equals("Max"); // true
    aDog.getName().equals("Fifi"); // false
    aDog == oldDog; // true
}

public static void foo(Dog d) {
    d.getName().equals("Max"); // true
    // change d inside of foo() to point to a new Dog instance "Fifi"
    d = new Dog("Fifi");
    d.getName().equals("Fifi"); // true
}
```

![img](https://cjlee38.github.io/assets/images/2020-10-27-21-06-02_2020-10-21-java_wrapper_class.md.png)

![img](https://cjlee38.github.io/assets/images/2020-10-27-21-08-22_2020-10-21-java_wrapper_class.md.png)

만약에 메서드 안에서  `d.setName("Fifi")` 를 사용해서 객체의 데이터를 직접 변경 했다면 아래와 같았음.

![img](https://cjlee38.github.io/assets/images/2020-10-27-21-15-17_2020-10-21-java_wrapper_class.md.png)





### Wrapper Class가 인자일때는?

보통 객체를 인자로 넘겨서 메서드에서 사용할 경우에는 객체를 초기화 하지 않는 이상 기존 객체의 실제 데이터에 접근이 가능한데 Wrapper Class의 경우는 **불변객체이므로 객체를 변경하거나 연산하면 초기화**가 되기 때문에 기존 객체의 데이터에 영향을 주지 않음.

String도 불변객체이기 때문에 동일.

