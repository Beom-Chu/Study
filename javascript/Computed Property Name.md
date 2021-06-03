# Computed Property Name

#### 표현식(expression)을 사용해 객체의 key값을 정의하는 문법.

```javascript
let fruit = {
    ['이'+'름'] : '사과'
}

console.log(fruit); //{이름 : "사과"}
```

property key로 변수도 사용 가능.

```javascript
const name = '이름';
const color = '색상';

let fruit = {
    [name] : '사과',
    [color] : '빨강'
}

console.log(fruit); //{이름: "사과", 색상: "빨강"}
console.log(fruit[name]); //사과
```

```javascript
let i = 1;
let fruit = {
    ['과일' + i++] : '사과',
    ['과일' + i++] : '딸기',
    ['과일' + i++] : '바나나'
}
console.log(fruit); //{과일1: "사과", 과일2: "딸기", 과일3: "바나나"}
```



