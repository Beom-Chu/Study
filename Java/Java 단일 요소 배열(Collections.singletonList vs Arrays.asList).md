# Java 단일 요소 배열(Collections.singletonList vs Arrays.asList)

개발을 하다보니 List에 하나의 객체만 가지고 사용하는 경우가 있는데 이때 어떤 메소드를 사용하는게 좋을까?

### Collections.singletonList()

* 변경 여부 : 불변
* 사이즈 : 1로 고정 (지정된 단일 객체를 가르키는 주소값을 가짐)
* **값 및 구조적 변경**시 UnsupportedOperationException 발생

### Arrays.asList()

* 변경 여부 : 값 변경 가능(set 메소드 허용)
* 사이즈 : 소유한 배열의 고정된 사이즈 목록을 반환
* **구조적 변경**시 UnsupportedOperationException 발생

## 결론

1. **메모리를 효율적**으로 사용하는 `Collections.singletonList`를 사용.
2. 여러 객체를 가져야하거나 변경이 필요한 경우에는 `Arrays.asList`를 사용.