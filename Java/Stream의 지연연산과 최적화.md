# Stream의 지연연산과 최적화

Stream의 대표적인 특징 중 하나는 바로 지연 연산(Lazy Evaluation)을 수행한다는 것이다.

지연 연산이란 결과값이 필요할때까지 계산을 늦추는 기법이다.

즉, 눈앞에 코드가 주어졌을때 바로 해당 코드를 실행하지 않고 실행 결과가 필요한 시점에 실행하도록 하는 것이다. 다만, 이러한 방식으로 코드가 동작하기 위해 내부적으로 준비 작업이 필요하므로 무조건 효율적이라고 보기는 어렵다.

### Stream이 지연연산이 아니었다면

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

### 지연 연산과 최적화

스트림 파이프라인을 실행하게 되면 JVM은 곧바로 스트림 연산을 실행시키지 않는다. 대신 최소한의 필수적인 작업만을 수행하기 위해 지연 연산의 준비 작업을 수행한다. 

이 준비 작업이란 스트림 파이프라인이 어떤 중간, 최종 연산으로 구성되어 있는지에 대한 검사로 시작된다. 이러한 검사 결과를 바탕으로 JVM은 사전에 어떤 방식으로 최적화를 진행할지 미리 계획하고, 그 계획에 따라 스트림의 개별 요소에 대한 스트림 연산을 수행하게 된다.

스트림에서 제공하는 대표적인 최적화 전략으로는 루프퓨전과 쇼트서킷이 있다.

### 루프퓨전

루프퓨전(loop fusion)이란 파이프라인에서 **연속적으로 체이닝된 복수의 스트림 연산을 하나의 연산 과정으로 병합**시키는 것이다.

위에서 실행된 결과로 루프퓨전을 확인 할 수 있다.

콘솔에 출력된 결과를 보면 `데이터 1`에 대해 작업A, B, C가 순차적으로 실행 되었고, `데이터 2`와 `데이터 3`도 마찬가지로 순차적으로 수행된 것을 알 수 있다. 이는 두개의 중간연산 `peek`과 최종연산 `forEach`가 **하나의 과정으로 병합**되었음을 의미한다.

즉, 아래와 같이 동작했다고 보면 된다.

```java
    List<Data> dataList = List.of(new Data(1), new Data(2), new Data(3));
    for (Data data : dataList) {
        data.runOperationA();
        data.runOperationB();
        data.runOperationC();
    }
```

이처럼 복수의 스트림 연산이 하나로 병합되는 것을 최적화라고 볼 수 있는 이유는 기본적으로 **개별 스트림 요소에 접근하는 횟수가 최소화**되기 때문이다. 만일 루프병합이 일어나지 않는다면 개별 스트림 연산에서 매번 스트림의 요소를 처음부터 전부 다 순회해야 했을 것이다.

즉, 위의 예시를 생각해보면 3개의 스트림 요소는 원래 각각 3번씩, 총 9번 접근되어야 했다. 그러나 루프병합 과정 덕분에 각각 1번씩, 총 3번만 접근되었음을 알 수 있다.

다만 주의할 점은 **모든 스트림 연산에 대해 루프병합이 적용되는 것은 아니라는 점**이다.


