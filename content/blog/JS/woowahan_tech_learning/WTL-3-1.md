---
title: '[JS] 우아한 테크 러닝 3주차 - (1)' 
date: 2020-09-21 14:57:04
category: 'JS'
draft: false
---

### Redux와 미들웨어

```js

import { createStore } from "./redux";

function api(url, cb){
  setTimeout(()=> {
    cb({ type: "응답이야", data: []})
  }, 2000);
}

function reducer(state = { counter: 0 }, action) {
  switch (action.type) {
    case "inc":
      return {
        ...state,
        counter: state.counter + 1
      };
    case "fetch-user":
      api('/api/v1/users/1', users => {
        return { ...state, ...users}
      });
    default:
      return { ...state };
  }
}
```

비동기 작업은 store의 state를 업데이트하는 것이 불가능하다.

reducer는 동기적으로 동작하기 때문이다.

위의 코드를 보면 fetch-user라는 action을 reducer에 전달해주면,
api함수가 호출될 것이다. 



```js
api('/api/v1/users/1', users => {
        return { ...state, ...users}
      });
```
비동기 콜백안에 있는 state값은 클로저로 접근할 수 있지만 현재의 상태가 아닌 과거의 상태가 될것이다.
api함수를 호출해서 state를 바꾸어 준다면 api함수가 시작된 후 끝날 때 까지 발생한 dispatch들은 api함수가 끝난 후 덮어씌워져 사라질 것이다.

따라서 비동기적인 작업은 reducer를 통해 처리할 수 없다.

이점을 어떻게 해결해야 할까??
reducer 밖에서 처리 해야할 필요가 있다.

`미들웨어`를 통해 해결해 보자

https://dobbit.github.io/redux/advanced/Middleware.html 참조

먼저 dispatch를 로깅 해보자

```js
function dispatchAndLog(store, action) {
  console.log('dispatching', action);
  store.dispatch(action);
  console.log('next state', store.getState());
}
```
위의 방식으로 로깅 할 수 있지만 추후에 코드의 수정이 필요할 수 있다.
코드 수정 => 다시 빌드 => QA => 배포 
막대한 비용이 발생하게 된다.


#### 미들웨어: 액션이 흘러가고 데이터를 처리하는 곳

#### 커링: 사용자에게 인자와 인자사이에 개입할 여지를 주는 것
```js
const add1 = function(a,b){
  return a + b;
}
const add2 = function(a){
  return function(b){
    return a + b;
  }
}

add1(10,20); // 30
addTen = add2(10)
addTen(20); // 30
addTen(10); // 20
```
add2함수는 addTen을 이용해 사용자가 개입할 수 있음을 보여준다.

```js
const logger = (store) => (next) => (action) => {
  console.log("logger: ", action.type);
  if (action.type === "get user info") {
    api.call("/api/v1/users/1").then((res) => {
      next({
        type: "res user info",
        data: res
      });
    });
  } else {
    next(action);
  }
};
```
logger라는 미들웨어는 action.type을 확인하면 next를 보류하고 api호출이 끝났을 때 응답 값을 next함수로 전달한다.
api호출이 끝난 이후에 dispatch를 호출하기 때문에 비동기 업데이트가 가능해졌다.

```js
const monitor = (store) => (next) => (action) => {
  setTimeout(() => {
    console.log("monitor: ", action.type);
    next(action);
  }, 2000);
};

const store = createStore(reducer, [logger, monitor]);

```
또한 action.type을 알려주는 로깅 또한 미들웨어로 사용가능하다.
만들어진 미들웨어들을 createStore에 인자 전달해주면 action이 미들웨어들을 따라 흘러 진행된다.


```js
export function createStore(reducer, middlewares = []) {
  let state;
  let listeners = [];

  const publish = () => {
    listeners.forEach(({ subscriber, context }) => {
      subscriber.call(context);
    });
  };

  const dispatch = (action) => {
    state = reducer(state, action);
    publish();
  };

  const subscribe = (subscriber, context = null) => {
    listeners.push({
      subscriber,
      context
    });
  };

  const getState = () => ({ ...state });

  const store = {
    dispatch,
    getState,
    subscribe
  };

  middlewares = Array.from(middlewares).reverse();
  let lastDispatch = store.dispatch;

  middlewares.forEach((middleware) => {
    lastDispatch = middleware(store)(lastDispatch);
  });

  return { ...store, dispatch: lastDispatch };
}

export const actionCreator = (type, payload = {}) => ({
  type,
  payload: { ...payload }
});

```
만들어진 redux의 코드는 위와 같다.

index.js
```js
const store = createStore(reducer, [logger, monitor]);
```

createStore에 reducer와 미들웨어들을 인자로 전달하여 실행한다.

redux.js
```js
middlewares = Array.from(middlewares).reverse();
  let lastDispatch = store.dispatch;

  middlewares.forEach((middleware) => {
    lastDispatch = middleware(store)(lastDispatch);
  });

  return { ...store, dispatch: lastDispatch };
```
lastDispatch의 초기값은 store.dispatch이고, 전달 받은 미들웨어들은 Array.Prototype.reverse()를 통해 역순으로 호출한다.
미들웨어로 logger와 monitor를 주었으므로 lastDispatch 값은
lastDispatch = store.dispatch
lastDispatch = monitor(store)(store.dispatch)
lastDispatch = logger(store)(monitor(store)(store.dispatch))로 변할것이다.

```js
store.dispatch({
  type: "inc"
});
```
index.js에서 redux에서 가져온 dispatch를 실행해보자.
이걸 분석해보면 logger(store) => (next) => (action){...} 커링 함수의 next로는 monitor(store)(store.dispatch) 값이 해당하고
action으로는 {type: "inc"}를 전달했음을 알수 있다.

따라서 logger => monitor => store.dispatch(원본) 으로 action이 흐르게 됨을 알 수 있을 것이다.

즉 redux에서는 커링을 이용하여 주어진 미들웨어 순서대로 next(다음에 처리할 미들웨어)를 설정한다.
action이라는 데이터를 dispatch에 전달하여 실행하면 정해진 미들웨어 순서로 action이 흐르면서 데이터가 처리됨을 이해할 수 있었다.