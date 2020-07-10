---
title: "[JavaScript] Primitive Types"
date: 2020-05-18 18:00:00
category: 'JS'
draft: false
---


### Primitive Type이란 object나 methods가 아닌 data를 의미한다.

자바스크립트에서는 string, number, bigint, boolean, undefined, symbol이라는
6가지 primitive data type들이 존재한다.

#### undefined: 선언되었을때 자동적으로 갖게되는 value

```javascript
let data;
console.log(data); // undefined
```

#### bigint: 2<sup>53</sup> -1 보다 큰 수

```javascript
console.log(typeof BigInt(Math.pow(2, 53))); //bigint
console.log(BigInt(Math.pow(2, 53))); //9007199254740992n
```

#### Symbol: es6에서 새로 생긴 data type이다.

심볼은 property로 사용되었을 때 unique하고 private한 특성을 갖는다.

code를 통해 확인해보자

```javascript
let sym = Symbol("key"); // Symbol(key)
let sym = Symbol(); // Symbol()
```

다음과 같이 문자열을 사용하여 심볼을 생성할 수 있다.

```javascript
Symbol("foo") === Symbol("foo"); // false
```

매번 새로운 심볼을 만드는 것을 알 수 있다.

```javascript
let sym = new Symbol(); // TypeError
```

primitive data type이기 때문에 에러가나는 것을 알 수 있다.

```javascript
const a = {
  [Symbol("foo")]: 10,
};
console.log(a); //{ [Symbol(foo)]: 10 }
console.log(a[Symbol("foo")]); // undefined
```

property로 심볼이 사용된 경우 심볼에 대하여 직접 접근하는것이 불가능하다.

```javascript
const data = {
  [Symbol("name")]: "JiHwan",
  age: 20,
  language: "JavaScript",
};
for (const e in data) {
  //undefined
  console.log(e, data[e]); //age 20
} //language JavaScript
```

property에 직접 접근이 불가능하기 때문에, for in 을 이용하여 출력하면
undefined로 나타나는 것을 확인할 수 있다.

```javascript
console.log(Symbol() + 1); // TypeError: Cannot convert a Symbol value to a number
```

심볼은 Type Coercionn이 일어나지 않는다.
