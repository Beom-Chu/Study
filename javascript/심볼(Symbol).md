# 심볼(Symbol)

* 심볼 값은 객체 프로퍼티에 대한 **식별자**로 사용.

* 모든 심볼값은 **고유**함.

* **심볼(symbol)** 데이터 형은 원시 데이터 형의 일종.
* 선언시 `new`를 사용하지 않음.
* 비열거형

```javascript
const symbol1 = Symbol();
const symbol2 = Symbol(7);
const symbol3 = Symbol('apple');

console.log(typeof symbol1); //"symbol"
console.log(symbol2 === 7); // false
console.log(symbol3.toString()); //"Symbol(apple)"
console.log(Symbol('foo') === Symbol('foo')); //false
```

---

## 심볼의 사용

```javascript
const taste = Symbol('taste');
const taste2 = Symbol('taste');
const fruit = {
    name : 'Apple',
    [taste] : 'sweet',
    [taste2] : 'fresh'
}

console.log(fruit); //{ name: 'Apple', [Symbol(taste)]: 'sweet', [Symbol(taste)]: 'fresh' }

console.log(Object.keys(fruit)); //[ 'name' ]
console.log(Object.values(fruit)); //[ 'Apple' ]
console.log(Object.entries(fruit)); //[ [ 'name', 'Apple' ] ]

for(let f in fruit) {
    console.log(f);
}
// 'name'
```

* Symbol은 유일한 값이므로 다른 어떠한 프로퍼티와도 충돌하지 않는다.

* Symbol은 keys, values, entries와 같은 **나열형 함수에서 무시**된다.
* for in 반복문에서도 무시된다.

---

## 심볼 속성 찾기

```javascript
const taste = Symbol('taste');
const taste2 = Symbol('taste');
const fruit = {
    name : 'Apple',
    [taste] : 'sweet',
    [taste2] : 'fresh'
}

console.log(taste.description); // 'taste'
console.log(Object.getOwnPropertySymbols(fruit)); //[ Symbol(taste), Symbol(taste) ]
```

---

## 전역 심볼 [Symbol.for()]

* **하나의 심볼만 보장** 받을 수 있음

* Symbol 함수는 매번 다른 Symbol값을 생성하지만,

  Symbol.for 함수는 하나를 생성한 뒤 키를 통해 같은 Symbol을 **공유**

```javascript
const id1 = Symbol.for('id');
const id2 = Symbol.for('id');

console.log(id1 === id2); //true
```

