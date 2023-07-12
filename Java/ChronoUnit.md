# ChronoUnit

`ChronoUnit` 클래스는 `java.time.temporal` 패키지에 속하는 열거형 클래스입니다. 

이 클래스는 날짜와 시간의 간격을 측정하는 데 사용되는 다양한 시간 단위를 제공합니다.

 `ChronoUnit`은 `java.time.temporal.TemporalUnit` 인터페이스를 구현하므로 시간 단위의 속성과 메서드를 제공합니다.

### 사용법

* `ChronoUnit` 객체
  *  `ChronoUnit` 클래스는 다양한 시간 단위를 나타내는 상수를 정의하고 있습니다. 이러한 상수들을 사용하여 `ChronoUnit` 객체를 생성할 수 있습니다.

```java
ChronoUnit days = ChronoUnit.DAYS;  // 일(day) 단위를 나타내는 ChronoUnit 객체 생성
ChronoUnit hours = ChronoUnit.HOURS;  // 시간(hour) 단위를 나타내는 ChronoUnit 객체 생성
```

* `between()` 메서드
  *  `ChronoUnit` 클래스는 `between()` 메서드를 제공합니다. 이 메서드는 두 시간(Temporal) 사이의 간격을 특정 시간 단위로 계산합니다. 

```java
LocalDateTime dateTime1 = LocalDateTime.of(2023, 6, 1, 12, 0);  // 첫 번째 시간
LocalDateTime dateTime2 = LocalDateTime.of(2023, 6, 2, 12, 0);  // 두 번째 시간

long diffInDays = ChronoUnit.DAYS.between(dateTime1, dateTime2);  // 두 시간 사이의 일(day) 단위 차이 계산
long diffInHours = ChronoUnit.HOURS.between(dateTime1, dateTime2);  // 두 시간 사이의 시간(hour) 단위 차이 계산
```

* 다양한 시간 단위: `ChronoUnit` 클래스는 다양한 시간 단위를 제공합니다. 
  *  `YEARS`, `MONTHS`, `WEEKS`, `DAYS`, `HOURS`, `MINUTES`, `SECONDS`, `MILLIS`, `MICROS`, `NANOS` 등.

`ChronoUnit` 클래스는 Java 8 이상에서 사용할 수 있으며, 날짜와 시간의 간격을 다룰 때 유용합니다. 

다양한 시간 단위를 제공하므로 두 시간 사이의 차이를 원하는 단위로 측정하고 계산하는 데 활용할 수 있습니다.