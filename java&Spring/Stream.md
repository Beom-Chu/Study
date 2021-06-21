# Stream   API

Stream이란 다양한 데이터를 표준화된 방법으로 다루기 위한 라이브러리



```
example.stream().filter(x -> x < 2).count();
```

stream() <- 스트림생성

filter < - 중간 연산 (스트림 변환) - 연속에서 수행 가능합니다.

count <- 최종 연산 (스트림 사용) - 마지막에 단 한 번만 사용 가능합니다.



**Stream의 특징**

* Stream은 데이터를 변경하지 않습니다.

* Stream은 1회용 입니다.

* Stream은 지연 연산을 수행합니다

* Stream은 병렬 실행이 가능합니다

 

**Stream의 종류**

| Stream <T>   | 범용 Stream               |
| ------------ | ------------------------- |
| IntStream    | 값 타입이 Int인 Stream    |
| LongStream   | 값 타입이 Long인 Stream   |
| DoubleStream | 값 타입이 Double인 Stream |

 

**Stream의 중간 연산 명령어**

| Stream < T > distinct()                                      | Stream의 요소 중복 제거                              |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| Stream < T > sorted()                                        | Stream 요소 정렬                                     |
| Stream < T > filter(Predicate < T > predicate)               | 조건에 충족하는 요소를 Stream으로 생성               |
| Stream < T > limit(long maxSize)                             | maxSize 까지의 요소를 Stream으로 생성                |
| Stream < T > skip(ling n)                                    | 처음 n개의 요소를 제외하는 stream 생성               |
| Stream < T > peek(Consumer< T > action)                      | T타입 요소에 맞는 작업 수행                          |
| Stream < R > flatMap(Function< T, stream<? extends R>> Tmapper) | T타입 요소를 1:N의 R타입 요소로 변환하여 스트림 생성 |
| Stream < R > map(Function<? super T, ? extends R> mapper)    | 입력 T타입을 R타입 요소로 변환한 스트림 생성         |
| Stream mapToInt(),mapToLong(),mapToDobule()                  | 만약 map Type이 숫자가 아닌 경우 변환하여 사용       |



**Stream의 최종 연산 명령어**

| <R, A> R collect(Collector<? super T, A, R> collector) |                                                     |
| ------------------------------------------------------ | --------------------------------------------------- |
| void forEach(Consumer <? super T> action)              | Stream 의 각 요소에 지정된 작업 수행                |
| long count()                                           | Stream 의 요소 개수                                 |
| Optional < T > sum (Comparator <? super T> comparator) | Stream 의 요소 합                                   |
| Optional < T > max (Comparator <? super T> comparator) | Stream 요소의 최대 값                               |
| Optional < T > min (Comparator <? super T> comparator) | Stream 요소의 최소 값                               |
| Optional < T > findAny()                               | Stream 요소의 랜덤 요소                             |
| Optional < T > findFirst()                             | Stream 의 첫 번째 요소                              |
| boolean allMatch(Pradicate < T > p)                    | Stream 의 값이 모두 만족하는지 boolean 반환         |
| boolean anyMatch(Pradicate < T > p)                    | Stream 의 값이 하나라도 만족하는지 boolean 반환     |
| boolean noneMatch(Pradicate < T > p)                   | Stream 의 값이 하나라도 만족하지않는지 boolean 반환 |
| Object[] toArray()                                     | Stream 의 모든 요소를 배열로 반환                   |

 

| reduce 연산                                                  | Stream 의 요소를 하나씩 줄여가며 계산한다.                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| - Optional < T > reduce(Binary Operator< T > accumulator)<br>- T reduce ( T identity, BinaryOperator< T > accumulator)<br/>- < U > U reduce (U indentity, BiFunction<U, ? super T, U> accumulator, BinaryOperator< U > combiner) | - .reduce((x,y) -> x > y ? x : y );<br/>- .reduce(1, (x,y) -> x * y);<br/>- .reduce(0.0,  (val1, val2) -> Double.valueOf(val1 + val2 / 10),  (val1, val2) -> val1 + val2); |

 

| collector 연산                                               |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| - Collectors.toList()<br>- Collectors.toSet() <br>- Collectors.toMap() <br>- Collectors.groupingBy()  <br/>- Collectors.partioningBy  <br/>- Collectors.summarizingInt() | Stream의 요소를 수집하여 요소를 그룹화 하거나 결과를 담아 반환하는데 사용한다. |

 
