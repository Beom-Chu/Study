# 나머지 매개변수, 전개구문(Rest parameters, Spread syntax)

### 나머지 매개 변수(Rest parameters)

정해지지 않은 수의 인수를 배열로 나타냄.

함수의 마지막 파라미터 앞에 `...`을 붙여서 사용.

```javascript
function myFun(a, b, ...manyMoreArgs) {
  console.log("a", a);
  console.log("b", b);
  console.log("manyMoreArgs", manyMoreArgs);
}

myFun("one", "two", "three", "four", "five", "six");
// a, one
// b, two
// manyMoreArgs, [three, four, five, six]

/* 나머지 매개변수에 하나의 값만 있어도 배열 형태로 들어감 */
myFun("one", "two", "three");
// a, one
// b, two
// manyMoreArgs, [three]

/* 나머지 매개변수에 인수가 없어도 배열 형태(비어있음) */
myFun("one", "two");
// a, one
// b, two
// manyMoreArgs, []
```

---

### 전개구문(Spread syntax)

배열이나 문자열과 같이 반복 가능한 문자를 여러개의 요소로 확장.

```javascript
function print(a, b, c) {
    console.log(a);
    console.log(b);
    console.log(c);
}
const numbers = [1, 2, 3];
  
print(...numbers);
//1
//2
//3
```



#### 배열에서의 전개

`push()`,`splice()`,`concat()` 등을 대신 사용 가능.

```javascript
let abc = ['a','b','c'];
let arr1 = [1, 2, abc, 3];
let arr2 = [1, 2, ...abc, 3];

console.log(arr1); //[ 1, 2, [ 'a', 'b', 'c' ], 3 ]
console.log(arr2); //[ 1, 2, 'a', 'b', 'c', 3 ]
```



배열 복사 가능.

```javascript
var arr = [1, 2, 3];
var arr2 = [...arr]; // arr.slice() 와 유사
arr2.push(4);

console.log(arr); //[ 1, 2, 3 ]
console.log(arr2); //[ 1, 2, 3, 4]
```

*전개구문으로 배열 복사시 1레벨 깊이로 동작하기 때문에 다차원 배열을 복사시에는 부적합*



배열 연결 가능.

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
arr3 = [...arr1, ...arr2];

console.log(arr3); //[ 1, 2, 3, 4, 5, 6 ]
```



#### 객체에서의 전개

```javascript
let obj1 = { a: 'alpha', x: 1 };
let obj2 = { b: 'beta', y: 2 };

let obj3 = { obj1 };
let obj4 = { ...obj1 };
let obj5 = { ...obj1, ...obj2 };

console.log(obj3); //{ obj1: { a: 'alpha', x: 1 } }
console.log(obj4); //{ a: 'alpha', x: 1 }
console.log(obj5); //{ a: 'alpha', x: 1, b: 'beta', y: 2 }
```

---

### 나머지 매개변수와 전개구문

* 나머지 매개변수는 함수의 선언시 사용하여 호출시 **여러 인자를 배열 형태로** 넘겨주는 것.
* 전개구문은 **배열을 여러 인자 형태로** 풀어서 사용하는것.
* 나머지 매개변수는 파라미터의 제일 마지막에만 사용 가능

