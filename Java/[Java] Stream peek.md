# [Java] Stream peek

peek은 Stream의 중간 연산자이다.

관련 정보를 찾아보면 디버깅 용도로 사용되는 내용만을 많이 다뤄서 다른 용도의 사용법을 기록하고자 한다.

우선 **디버깅** 용도로 사용되는 샘플 코드를 확인해 보자.

```java
class User {
  private String Id;
  private String name;
  private int age;

  public User(String id, String name, int age) {
    Id = id;
    this.name = name;
    this.age = age;
  }
  ...getter..
  ...setter..
  ...toString..
}
```

위와 같은 POJO 클래스가 있다고 가정해보자.

```java
        List<User> users = List.of(new User("001", "김땡땡", 15),
                new User("002", "이땡땡", 20),
                new User("003", "박땡땡", 10));

        List<User> list = users.stream()
                .peek(o -> System.out.println("초기값 : " + o))
                .filter(o -> o.getAge() > 15)
                .peek(o -> System.out.println("15살 초과 : " + o))
                .toList();
```

위와 같이 `println` 메소드를 사용해서 디버깅을 하게 되면 아래와 같은 결과를 얻을 수 있다.

```
초기값 : User{Id='001', name='김땡땡', age=15}
초기값 : User{Id='002', name='이땡땡', age=20}
15살 초과 : User{Id='002', name='이땡땡', age=20}
초기값 : User{Id='003', name='박땡땡', age=10}20}
```

디버깅 외에도 **POJO의 특정 필드값만 변경 후에 데이터 수집**을 하고 싶다면 `peek`에서 `setter`를 사용해주면 된다.

```java
        List<User> list2 = users.stream()
                .peek(o -> o.setAge(o.getAge() + 5))
                .filter(o -> o.getAge() > 15)
                .toList();
        System.out.println(list2);
```

```
[User{Id='001', name='김땡땡', age=20}, User{Id='002', name='이땡땡', age=25}]
```

결과에서 볼 수 있듯이 나이를 각각 5씩 더해주고 필터링이 된 것을 확인 할 수 있다.

이렇듯 디버깅 용도만이 아닌 **POJO를 새로 인스턴스화 하지 않고** peek을 사용해서 특정 필드만 업데이트 할 수 있다.

### peek  사용시 주의 사항

peek은 중간 연산자 이므로 최종 연산자에 따라서 수행이 생략 될 수도 있다.

```java
    long count = IntStream.range(1, 10)
        .peek(System.out::println)
        .count();
```

위 코드를 실행해보면 출력문이 없는것을 확인 할 수 있다.

peek은 count의 갯수에 영향을 주지 않으므로 출력 함수가 실행되지 않은 것이다.

즉, 중간연산자는 최적화를 위해 실행되지 않을 수 있다.


