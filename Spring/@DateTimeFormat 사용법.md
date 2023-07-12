# @DateTimeFormat 사용법

Spring에서 Client로 부터 날짜 타입의 파라미터를 전달 받을 때 특정 포맷으로 받을 때 사용 한다.

속성으로 iso, pattern 등이 사용 가능 하다.

### 사용 샘플

```java
@RestController
@RequestMapping("/test")
public class DateTimeFormatController {
    
    @GetMapping("/date/time/format")
    HttpEntity execute(@RequestParam(name="startDate") @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate startDate, 
                       @RequestParam(name="endDate") @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate endDate) {
        // 생략
    }
}
```

## 속성

#### iso

RequestParam을 포맷하는데 사용할 ISO 패턴 enum 타입으로 정의되어 있다.

```java
enum ISO {

		/**
		 * The most common ISO Date Format {@code yyyy-MM-dd},
		 * e.g. "2000-10-31".
		 */
		DATE,

		/**
		 * The most common ISO Time Format {@code HH:mm:ss.SSSZ},
		 * e.g. "01:30:00.000-05:00".
		 */
		TIME,

		/**
		 * The most common ISO DateTime Format {@code yyyy-MM-dd'T'HH:mm:ss.SSSZ},
		 * e.g. "2000-10-31T01:30:00.000-05:00".
		 * <p>This is the default if no annotation value is specified.
		 */
		DATE_TIME,

		/**
		 * Indicates that no ISO-based format pattern should be applied.
		 */
		NONE
	}
```

### pattern

iso 형식으로 표시되지 않는 날짜 시간 패턴을 받고자 할 때 사용한다.

데이터 타입 사용자 정의 패턴으로 패턴은 엄격하게 지켜줘야 한다.

| pattern              | example                   |
| -------------------- | ------------------------- |
| yyyy-MM-dd           | 2022-02-20                |
| yyyy-MM-dd HH:mm:ss  | 2022-02-20 15:36:23       |
| yyyy-MM-dd HH:mm:ssZ | 2022-02-20 15:36:23+09:00 |

