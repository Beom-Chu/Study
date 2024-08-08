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

```java
List<String> list = List.of("abc", "def");
```

위와 같이 문자열 List를 출력 해보면 아래와 같다.

```shell
[abc, def]
```

만약 문자열의 한 문자씩 List로 만들려면 어떻게 해야할까?

```java
List<List<String>> list2 = list.stream()
    map(o -> Arrays.stream(o.split("")).collect(Collectors.toList()))
    collect(Collectors.toList()););
```

위와 같이 map을 사용해서 문자열을 split 후 List로 만들면 아래와 같은 결과가 나온다.

```shell
[[a, b, c], [d, e, f]]
```

2차원 배열 형태로 만들어진 것을 알 수 있다.

flatMap을 사용해보자.

```java
List<String> list3 = list.stream()
    map(o -> Arrays.stream(o.split("")).collect(Collectors.toList()))
    flatMap(List::stream)
    collect(Collectors.toList());
```

```shell
[a, b, c, d, e, f]
```

flatMap으로 stream 평면화를 통해 1차원 형태로 List가 만들어졌다.
