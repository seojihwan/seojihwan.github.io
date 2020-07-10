---
title: "[JavaScript] Value vs Reference"
date: 2020-05-19 12:00:00
category: 'JS'
draft: false
---


```javascript
console.log([10] === [10]); //false
```

왜 [10]은 서로 같지 않을까?
이를 알기 위해서는 value와 reference의 차이를 알아야한다.

#### value

```javascript
a = 5;
b = a;
b = 10;
console.log(a); // 5
console.log(b); // 10
```

value는 말그대로 변수가 갖는 데이터를 말한다.
b = a 에서 b의 value는 a의 value와 갖게된다.
하지만 b의 value를 바꾸더라도 a의 value는 그대로 남아있음을 확인할 수 있다.

#### reference

```javascript
a = [1, 2, 3];
b = a;
b.push([4, 5, 6, 7]);
console.log(a); // [ 1, 2, 3, [ 4, 5, 6, 7 ]
```

```javascript
a = { a: 1, b: 2 };
b = a;
b.c = [4, 5, 6, 7];
console.log(a); // { a: 1, b: 2, c: [ 4, 5, 6, 7 ] }
```

array, object, function에 대해서는 value와 다른일이 발생한다.
변수는 해당 데이터와 같은 것이 아니라 메모리 상에 해당 데이터가 존재하고 그 메모리를 변수가 참조하게된다.
두번째 예시를 보면 b = a; 를 통해 b는 a와 같이 { a: 1, b: 2} 라는 object를 참조한다.
b.c = [4, 5, 6, 7]를 통해 object에 데이터를 추가하였다.
여기서 a도 같은 object를 참조하기 때문에 a를 출력했을때 c: [ 4, 5, 6, 7 ]라는 데이터가 추가됬음을 확인할 수 있다.

이를 통해 value와 reference의 차이를 알게 되었다.
