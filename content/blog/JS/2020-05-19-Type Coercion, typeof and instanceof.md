---
title: "[JavaScript] Type Coercion, typeof and instanceof"
date: 2020-05-19 14:00:00
category: 'JS'
draft: false
---

#### Type Coercion

- 자바스크립트는 type coersion을통해, 형태가 다른 데이터의 연산을 처리한다.

```javascript
console.log(123 + "hello"); // 123hello
console.log(11 + 11 + "hello"); // 22hello
console.log(3 + true); // 4
console.log(3 * false); // 0
console.log(true + "hello"); // truehello
```

연산에 따라 true 는 1 false는 0이 될수도, string이 될수도 있음을 확인할 수 있다.

```javascript
console.log(1 == "true"); // false
console.log(11 == "11"); // true
console.log(1 == true); // true
console.log("" == false); // true
console.log(null == undefined); // true
```

따라서 type coersion을 방지하기위해 항상 ===과 !==을 사용하는 습관을 갖는것이 좋다.

```javascript
console.log(11 === "11"); // false
console.log(1 === true); // false
console.log("" === false); // false
console.log(null === undefined); // false
```

#### Typeof

- Typeof를 통해 primitive 데이터, function의 type을 확인할 수 있다.

```javascript
console.log(typeof 42); // number
console.log(typeof "abc"); // string
console.log(typeof true); // boolean
console.log(typeof undelcaredVar); // undefined
console.log(typeof (() => {})); // function
```

```javascript
console.log(typeof []); // object
console.log(typeof {}); // object
console.log(typeof null); // object
```

하지만 primitive data가 아닌 array, object, null은
모두 object로 나타나는 것을 알 수 있다.

이럴경우에는 instanceof 를 사용해주면된다.

```javascript
console.log([] instanceof Object); // true
console.log({} instanceof Array); // false
console.log((() => {}) instanceof Function); // true
```

하지만 [] instanceof Object가 true라는 예상치 못한 결과가 나타난다. 이를 이해하기 위해서는 prototype과 \_\_proto\_\_의 차이에 대해 알아야한다.

```javascript
console.log([] instanceof Object); //true
console.log(Object instanceof Function); //true
console.log(Function instanceof Object); //true
console.log({} instanceof Array); //false
console.log([] instanceof Array); //true
console.log(Array instanceof Array); //false
```

A instanceof B
instanceof는 A의 prototype chain에서 constructor인 B의 prototype이
존재하는지 판별하는 연산자이다. 자바스크립트에서 Object, Function, Array의생성자는 모두 Function으로 이루어져있기 때문에 원형('\_\_proto\_\_\')은 Function이다. prototype chain에서 모든 prototype의 원형은 object prototype이기 때문에 prototype chain을 따라가면, object prototype을 만나게된다.

즉 Object, Function, Array 모두 원형이 Function prototype이고, chain을 따라가면 Object prototype이기 때문에, Object instanceof Function, Function instanceof Object 모두 참인것이다.
내용이 좀 생소해서 이해가 안될 수 있는데 위의 예제가 하나라도 이해가 안되는 것이 있다면 다음 post를 참조하길 바란다.

[proto 와 prototype의 차이](https://seojihwan.github.io/web/proto-vs-prototype/)
