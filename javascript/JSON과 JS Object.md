# JSON과 JS Object

유사한 구조로 인해서 헷갈릴수 있으니 주의.

---

- JS Object는 **JS Engine 메모리 안에 있는 데이터 구조**
- JSON은 **객체의 내용을 기술하기 위한 text **

```javascript
/* 자바스크립트 객체의 문법 */
{
  name: "apple",
  color: "red"
}

/* JSON 형태의 문법 */
{
   "name" : "apple",
   "color": "red"
}
```



### Json

* 객체키는 문자열
* 값의 종류
  * 숫자(Number)
  * 문자열(String)
  * true/false(Boolean)
  * 배열(Array)
  * Json 객체
  * null

### JS Object

* 문자열 리터럴, 숫자 리터럴, 식별자 이름을 키로 사용.

* 값은 함수 및 JavaScript 표현식도 포함.

  

---

## JSON 객체 메서드

* JSON.parse

  JSON 문자열을 JS Object로 변환

* JSON.stringify

  JS Object를 JSON 문자열로 변환

