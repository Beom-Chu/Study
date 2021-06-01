# 변수, 호이스팅, TDZ(Temporal Dead Zone)

* var, let, const의 차이

  |          |   var    |  let  | const |
  | :------: | :------: | :---: | :---: |
  |  재선언  |    O     |   X   |   X   |
  |  재할당  |    O     |   O   |   X   |
  | 호이스팅 |    O     |   O   |   O   |
  |  스코프  | function | block | block |

<br>

* 호이스팅

  각 변수의 스코프 안에서 최상위에 선언 되어지는 것.

  ```javascript
  name = 'apple';
  var name;
  
  // 위 아래가 동일
  
  var name;
  name = 'apple';
  ```

<br>


* Temporal Dead Zone

  `TDZ`란 스코프의 시작지점부터 초기화 지점까지의 구간.

  `let`/`const`는 `TDZ`에 의해 제약을 받는다.

  변수가 초기화되기 전에 액세스하려고 하면, `var`처럼 `undefined`를 반환하지 않고, `ReferenceError`가 발생한다.

  이는 코드를 예측가능하고 잠재적 버그를 쉽게 찾아낼 수 있도록 한다.
