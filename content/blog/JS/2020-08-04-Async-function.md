---
title: '[JS] Async-function'
date: 2020-08-04 12:26:11
category: JS
draft: false
---

Async function: 비동기 함수를 처리하기 위한 함수로, Promise를 사용하여 결과를 반환한다.

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)

MDN의 예시를 통해 이해해보자

#### Description

- async 함수에는 await식이 포함될 수 있다.
- await는 async 함수의 실행을 일시 중지하고 전달 된 Promise의 해결을 기다린 다음 async 함수의 실행을 다시 시작하고 완료후 값을 반환
- await 키워드는 async 함수에서만 유효

```js
var resolveAfter2Seconds = function () {
  console.log('starting slow promise');
  return new Promise((resolve) => {
    setTimeout(function () {
      resolve(20);
      console.log('slow promise is done');
    }, 2000);
  });
};
```
위 함수는 Promise를 만들어 반환하는 함수이다.
promise가 시작됬다는 메시지가 출력되고 나서 setTimeout에 의해 해당 Promise는 2초후에 resolve상태가 되고 20의 값을 갖게된다. 그 이후 promise가 끝났다는 메시지를 보여준다.

```js
var resolveAfter1Second = function () {
  console.log('starting fast promise');
  return new Promise((resolve) => {
    setTimeout(function () {
      resolve(10);
      console.log('fast promise is done');
    }, 1000);
  });
};
```
이전 함수와 promsise상태가 바뀌고 값을 갖는데 걸리는 시간이 1초라는 것 외에 차이가 없다.


```js
var sequentialStart = async function () {
  console.log('==SEQUENTIAL START==');

  // If the value of the expression following the await operator is not a Promise, it's converted to a resolved Promise.
  const slow = await resolveAfter2Seconds();  // -- A
  console.log(slow);

  const fast = await resolveAfter1Second();  // -- B
  console.log(fast);
};
```

위의 sequentialStart함수는 async 함수로 선언되어 있고 위의 Description처럼 A와 B는 각각 완료가 된이후에 async함수가 진행될것이다.

```js
sequentialStart() 
/*
starting slow promise
slow promise is done
20
starting fast promise
fast promise is done
10
*/
```



```js
var concurrentStart = async function () {
  console.log('==CONCURRENT START with await==');
  // starts timer immediately
  const slow = resolveAfter2Seconds();  // - A
  const fast = resolveAfter1Second(); // - B

  console.log(await slow);
  console.log(await fast); // waits for slow to finish, even though fast is already done!
};
```

sequentialStart과는 달리 promise를 생성하는 A B에 await가 없기 때문에 promise를 생성한 후 각각의 timer는 동시에 비동기로 수행된다.

따라서 B가 먼저 promise가 완료될 것이다.
생성된 promise를 호출할 때 await가 붙었기 때문에 promise<pending>으로 출력 되지않고 완료된 A 그리고 완료된 B가 순차적으로 출력될 것이다.

B는 A보다 빠르게 resolved되지만 완료된 A의 value인 20이 먼저 출력될 것이다. 
또한 전체 처리시간은 2 + 1 이 아닌 2초임을 이해해야한다.

```js
concurrentStart()
/*
starting slow promise
starting fast promise
fast promise is done
slow promise is done
20
10
*/
```

```js
var stillConcurrent = function () {
  console.log('==CONCURRENT START with Promise.all==');
  Promise.all([resolveAfter2Seconds(), resolveAfter1Second()]).then((messages) => {
    console.log(messages[0]); // slow
    console.log(messages[1]); // fast
  });
};
```


Promise.all()은 iterable 가능한 객체에 주어진 promise들을 모두 이행한 후 결과를 iterable 가능한 객체로 반환한다. 각각의 promise들은 비동기로 처리되기 때문에, 
수행시간은 2 + 1이 아닌 2초이다.

Promise.all을 이용하여 여러가지 promise들을 동시에 처리하고 결과를 반환할때 사용할 수 있다.


```js
stillConcurrent()

/*
starting slow promise
starting fast promise
fast promise is done
slow promise is done
20
10
*/
```


```js
var parallel = function () {
  console.log('==PARALLEL with Promise.then==');
  resolveAfter2Seconds().then((message) => console.log(message));   // - A
  resolveAfter1Second().then((message) => console.log(message));  // - B
};
```

마지막으로 parallel은 promise.then을 통해 각각의 과정을 비동기 처리 한 것이다.

1초후 B가먼저 메시지를 출력하고, 늦게 완료된 A가 출력된다.

```js
parallel()

/*
starting slow promise
starting fast promise
fast promise is done
10
slow promise is done
20
*/
```