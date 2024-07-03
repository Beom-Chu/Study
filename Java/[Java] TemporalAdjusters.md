# [Java] TemporalAdjusters

java8 이후 시간을 조정하는데 사용되는 클래스.

사용법도 편하고 직관적이며, 제공하는 API도 많아서  사용하면 좋다.

* `LocalDate`, `LocalDateTime` 등 시간 관리 클래스로 날짜를 지정하고 지정된 날짜를 조정하는데 사용된다.

* `with` 메서드와 함께 사용 된다.
  
  
  
  ```java
  LocalDateTime now = LocalDateTime.now();
  now.with(TemporalAdjusters.firstDayOfYear());       // 이번 년도의 첫 번째 일(1월 1일)
  now.with(TemporalAdjusters.lastDayOfYear());        // 이번 년도의 마지막 일(12월 31일)
  now.with(TemporalAdjusters.firstDayOfNextYear());   // 다음 년도의 첫 번째 일(1월 1일)
  now.with(TemporalAdjusters.firstDayOfMonth());      // 이번 달의 첫 번째 일(1일)
  now.with(TemporalAdjusters.lastDayOfMonth());       // 이번 달의 마지막 일
  now.with(TemporalAdjusters.firstDayOfNextMonth());  // 다음 달의 첫 번째 일(1일)
  now.with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)); // 이번 달의 첫 번째 요일(여기서는 월요일)
  now.with(TemporalAdjusters.lastInMonth(DayOfWeek.FRIDAY));  // 이번 달의 마지막 요일(여기서는 마지막 금요일)
  now.with(TemporalAdjusters.next(DayOfWeek.FRIDAY));       // 다음주 금요일
  now.with(TemporalAdjusters.nextOrSame(DayOfWeek.FRIDAY)); // 다음주 금요일(오늘 포함. 오늘이 금요일이라면 오늘 날짜가 표시 된다.)
  now.with(TemporalAdjusters.previous(DayOfWeek.FRIDAY));     // 지난주 금요일
  now.with(TemporalAdjusters.previousOrSame(DayOfWeek.FRIDAY));// 지난주 금요일(오늘 포함)
  ```




