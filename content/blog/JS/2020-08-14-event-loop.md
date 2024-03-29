---
title: "[JS] Event loop"
date: 2020-08-14 02:27:26
category: 'JS'
draft: false
---

### 자바스크립트 동작

- asyncronous
- single-thread

### Event loop
- Call Stack
- 백그라운드
- Task Queue

![image](./image/architecture.png)

[블로그 링크](https://medium.com/front-end-weekly/javascript-event-loop-explained-4cd26af121d4)

MDN(https://developer.mozilla.org/ko/docs/Web/JavaScript/EventLoop)

#### 설명 

함수 호출은 스택을 형성한다. 호출된 함수는 종료된 후 스택에서 사라진다.
비동기로 처리할 함수들은 백그라운드에서 실행되고, 반환할 콜백을 큐에 넣어준다.
큐에 있는 콜백함수들은 스택이 빌때 까지 기다렸다가 스택으로 이동하고 종료된 후 스택에서 사라진다.



```javascript
setTimeout(() => {
  console.log("hello");
}, 1000); //delay >= 1000ms
```

따라서 위와 같은 code를 실행하였을 때 최소 delay인 1000ms는 백그라운드에서 `보장`되지만, 큐에서 스택이 비어있을때 까지 기다려야하므로, 추가적인 delay가 발생할 수 있다.

### 정리

- 백그라운드와 큐는 자바스크립트로 이루어져 있지않다.
-  JS Engine이 백그라운드와 큐의 동작을 처리해주기 때문에 여러가지 일을 동시에(`concurrency`) 처리할 수 있어 `비동기 처리`가 가능해진다.
- 자바스크립트 자체는 `single-thread`라는 특성을 갖고 있다.

---
### 결론
`Event Loop`에 의해 자바스크립트는 `비동기적`이면서 `싱글 쓰레드`로 동작한다는 것을 이해할 수 있다.