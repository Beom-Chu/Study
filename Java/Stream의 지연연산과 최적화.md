# Stream의 지연연산과 최적화

Stream의 대표적인 특징 중 하나는 바로 지연 연산(Lazy Evaluation)을 수행한다는 것이다.

지연 연산이란 결과값이 필요할때까지 계산을 늦추는 기법이다.

즉, 눈앞에 코드가 주어졌을때 바로 해당 코드를 실행하지 않고 실행 결과가 필요한 시점에 실행하도록 하는 것이다. 다만, 이러한 방식으로 코드가 동작하기 위해 내부적으로 준비 작업이 필요하므로 무조건 효율적이라고 보기는 어렵다.



아래와 같은 class가 있다고 가정해보자.

```java
class Data {
    private final int value;

    public Data(int value) {
        this.value = value;
    }

    public void runOperationA() {
        System.out.printf("데이터 %d의 작업A%n", value);
    }

    public void runOperationB() {
        System.out.printf("데이터 %d의 작업B%n", value);
    }

    public void runOperationC() {
        System.out.printf("데이터 %d의 작업C%n", value);
    }

    public int getValue() {
        return value;
    }

    @Override
    public String toString() {
        return "Data{" + value + '}';
    }
}
```

이런 `Data` 인스턴스로 구성된 스트림을 형성하고 아래와 같이 순차적으로 메소드를 호출한다고 생각해보자.

```java
    Stream.of(new Data(1), new Data(2), new Data(3))
            .peek(Data::runOperationA)
            .peek(Data::runOperationB)
            .forEach(Data::runOperationC);
```

개별 스트림 연산이 **즉각적으로(eager) 실행**된다고 가정하자. 그렇다면 3개 Data 스트림이 각각 `runOperationA`를 호출하고, 기존의 `Stream<Data>`를 반환할 것이다. 그 후 두 번째 연산에서 `runOperationB` 를 호출한 후 동일한 타입을 반환하고 마지막으로 `runOperationC`를 수행하게 될 것이다. 이런 방식으로 수행된다면 콘솔에는 아래와 같이 출력될 것이다.

```shell
데이터 1의 작업A
데이터 2의 작업A
데이터 3의 작업A
데이터 1의 작업B
데이터 2의 작업B
데이터 3의 작업B
데이터 1의 작업C
데이터 2의 작업C
데이터 3의 작업C
```

그러나 위 코드를 실제로 실행해보면 아래와 같이 콘솔에 출력된다.

```shell
데이터 1의 작업A
데이터 1의 작업B
데이터 1의 작업C
데이터 2의 작업A
데이터 2의 작업B
데이터 2의 작업C
데이터 3의 작업A
데이터 3의 작업B
데이터 3의 작업C
```








