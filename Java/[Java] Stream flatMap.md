# [Java] Stream Map vs flatMap

## Map

스트림 내부의 요소 하나하나에 접근해서 넣어준 함수를 수행해주는 메서드.

```java
   List<Person> people = Arrays.asList(
                new Person("personA", 24),
                new Person("personB", 26),
                new Person("personC", 28),
                new Person("personD", 30)
        );
```

위와 같은 리스트를 만들었다고 가정해보자.

```java
List<String> nameList = people.stream()
                .map(person -> person.getName())
                .collect(Collectors.toList());
```

이런 식으로 person 객체의 name으로 String 리스트를 만들 수 있다.

## flatMap

스트림 평면화를 위한 메서드.

배열안의 배열, 즉 2차원 배열을 1차원 배열로 변경을 원할 때 사용한다.


