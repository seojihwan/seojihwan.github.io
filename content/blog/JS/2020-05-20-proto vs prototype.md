---
title: "[JavaScript] __proto__ vs prototype"
date: 2020-05-20 12:00:00
category: 'JS'
draft: false
---


자바스크립트는 클래스 기반이 아닌 프로토타입 기반의 언어이다.
ES2015부터 class를 지원하였지만 class는 여전히 프로토타입 기반으로 동작한다.

```javascript
function person() {}
```

위와 같이 person이라는 함수를 정의하면 자바스크립트에서는 자동으로 person이라는 객체 뿐만아니라, person의 prototype이라는 객체가 생성된다.

```javascript
function person() {}
b = new person();
console.log(person === person.prototype.constructor); //true
```

person과 prototype은 서로 prototype과 constructor를 통해 참조 하고 있음을 확인 할 수 있다.

```javascript
function person() {}
b = new person();
console.log(b.__proto__ === person.prototype); //true
```

새로운 person객체를 만들고, 그 원형(\_\_proto\_\_)을 확인하면, person의 prototype과 같은것을 확인할 수 있다.
자바스크립트는 prototype chain을 통해 동작하는데, 이 때 말하는 prototype은 prototype property가 아닌 \_\_proto\_\_를 따라 움직인다고 생각하는 것이 좋다.

예시를 보면서 이해를 해보자.

```javascript
function person() {}
person.prototype.age = 21; //propertype에 age라는 속성 추가
b = new person();
c = new person();
c.age = 22;
console.log(b.age); //21
console.log(c.age); //22
```

위와같이 prototype에 age라는 속성을 추가 하고 그 값을 21로 설정하였다.
b.age를 호출하면 b라는 객체는 age라는 속성을 갖지 않기 때문에 \_\_proto\_\_를 따라 person.prototype으로 이동하여 age라는 속성을 찾는다. 이 때 값이 21이기 때문에 b.age는 21이 출력 되었다.
하지만 c는 age라는 속성을 갖기 때문에 person.prototype으로 이동하지 않고 age라는 속성값 22를 보여준다.
위의 예시는 prototype의 상속에 대한 기능과, prototype chain의 원리를 설명한것이다.

만약 prototype을 사용하지 않고, 다음과 같이 객체를 정의한다면 어떻게될까?

```javascript
function person() {
  //생성자에 property 추가
  this.age = 21;
  this.name = "Ji Hwan";
}
b = new person();
c = new person();
d = new person();
```

다음과 같이 새로운 person객체가 생성될 때마다, age, name속성에 메모리를 할당하고, 이는 메모리의 낭비로 이어질 것이다.

하지만 다음과 같이 property를 사용하면 생성 된 모든 객체가 공통적으로 prototype을 참조 하기 때문에, 메모리의 이점을 얻을 수 있다.

```javascript
function person() {} //prototype에 속성 추가
person.prototype.age = 21;
person.prototype.name = "Ji hwan";

b = new person();
c = new person();
d = new person();
d.age = 22;
d.name = "maria";
```

또한 다른 prototype에 정의된 속성들은 재정의가 가능하기 때문에 유지보수에 있어서 큰 도움이 된다.
