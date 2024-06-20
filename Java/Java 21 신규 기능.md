# Java 21 신규 기능

## JEP 431: [Sequenced Collections](https://openjdk.org/jeps/431)

기존의 자바 컬렉션은 정해진 순서의 원소에 접근하는 것이 어려웠다. 예를 들어 List와 Deque는 모두 처음과 마지막 원소에 접근할 수 있지만, 이들의 공통 상위 인터페이스는 Collection이라 접근 방법이 다르다. SortedSet과 LinkedHashSet 역시 비슷한데, 이로 인해 일관되지 못한 방법으로 원소에 접근하게 되었었다.

| Type          | First element         | Last element            |
| ------------- | --------------------- | ----------------------- |
| List          | list.get(0)           | list.get(list.size()-1) |
| Deque         | deque.getFirst()      | deque.getLast()         |
| SortedSet     | set.first()           | set.last()              |
| LinkedHashSet | set.iterator().next() | //missing               |

자바 21부터는 정해진 순서의 원소를 조회할 수 있는 컬렉션을 표현하는 새로운 인터페이스를 도입한다. 이를 통해 정해진 순서의 원소(첫 원소, 마지막 원소 등)에 접근하고, 이를 역순으로 처리하기 위한 일관된 API를 제공한다.

#### SequencedCollection

처음과 마지막 요소에 대한 공통된 기능을 제공한다. reversed()는 새롭게 추가된 메소드이지만, 나머지는 Deque로부터 승격된 것이다.

```java
interface SequencedCollection<E> extends Collection<E> {
    // new mthod
    SequencedCollection<E> reversed();
    // methods promoted from Deque
    void addFirst(E);
    void addLast(E);
    E getFirst();
    E getLast();
    E removeFirst();
    E removeLast();
}
```

추가로 제공되는 reversed()는 기존 컬렉션에서 역순으로 원소들을 처리할 수 있게 된다. 예를 들어 iterator(), forEach(), stream(), parallelStream(), toArray() 등과 같은 기능을 역순 상태의 원소로 호출 가능해지는 것이다.

#### SequencedSet

중복된 원소를 갖지 않는 SequencedCollection에 해당하는 Set이다.

SortedSet과 같은 컬렉션은 SequencedCollection 부모 인터페이스에 선언된 addFirst, addLast 메소드 등을 지원할 수 없기 때문에 Exception이 발생 할 수 있다.

```java
interface SequencedSet<E> extends Set<E>, SequencedCollection<E> {
    SequencedSet<E> reversed();    // covariant override
}
```

addFirst와 addLast는 LinkedHashSet과 같은 구현체에 특별한 의미를 갖는다. 원소가 이미 존재하는 경우 적절한 위치로 재배치가 되기 때문이다. 이는 요소를 재배치할 수 없는 LinkedHasSet의 오랜 결함을 해결해준다.

#### SequencedMap

SequencedMap 역시 정해진 순서의 원소에 대한 공통 기능을 제공한다.

SequencedSet과 마찬가지로 putFirst, putLast 메소드는 원소가 이미 존재하는 경우 적절한 위치로 재배치되는 특별한 의미를 갖는다.

```java
interface SequencedMap<K,V> extends Map<K,V> {
    // new methods
    SequencedMap<K,V> reversed();
    SequencedSet<K> sequencedKeySet();
    SequencedCollection<V> sequencedValues();
    SequencedSet<Entry<K,V>> sequencedEntrySet();
    V putFirst(K, V);
    V putLast(K, V);
    // methods promoted from NavigableMap
    Entry<K, V> firstEntry();
    Entry<K, V> lastEntry();
    Entry<K, V> pollFirstEntry();
    Entry<K, V> pollLastEntry();
}
```

## JEP 440:  **[Record Patterns](https://openjdk.org/jeps/440)**

레코드에 타입 패턴을 함께 적용하여 레코드의 값을 손쉽게 처리할 수 있게 해준다.

자바 16에서 instance of 연산자에 타입 패턴을 적용하여 패턴 매칭을 개선시켰다.

타입 패턴 덕분에 instance of 하위 블록에서는 직접적으로 타입 캐스팅할 필요가 없어졌다.

```java
// prior to java 16
if (obj instanceof String) {
    String s = (String)obj;
    ... use s ...
}


// as of java 16
if (obj instanceof String s) {
    ... use s ...
}
```

이러한 바탕은 자바 언어가 보다 데이터 중심적 프로그래밍 스타일로 나아가기 위함인데, 자바 21부터는 레코드 타입에 대해 보다 개선된 방법으로 사용 가능해졌다.

이전에는 레코드 클래스에 instance of 연산자를 적용하면 다음과 같이 사용했었다.

```java
record Point(int x, int y) {}

static void printSum(Object obj) {
    if (obj instanceof Point p) {
        int x = p.x();
        int y = p.y();
        System.out.println(x + y);
    }
}
```

하지만 자바 21부터는 다음과 같은 방식을 사용 가능해졌다. 이를 통해 obj가 Point의 인스턴스인지 여부를 테스트할 뿐만 아니라 개발자를 대신해 접근자 메소드를 호출하여 직접 변수들에 접근할 수 있게 된다.

```java
static void printSum(Object obj) {
    if (obj instanceof Point(int x, int y)) {
        System.out.println(x + y);
    }
}
```


