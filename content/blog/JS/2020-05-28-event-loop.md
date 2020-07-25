---
title: "[JS] Event loop"
date: 2020-05-24 12:00:00
category: 'JS'
draft: false
---

### 자바스크립트 동작

- asyncronous
- single-thread

### Event loop

![image](/image/Architecture.png)

[블로그 링크](https://medium.com/front-end-weekly/javascript-event-loop-explained-4cd26af121d4)

- Call Stack
- Web API
- Message Queue

실행된 code는 CallStack에 push된다.
CallStack에 쌓인 code가 실행될 때 WebAPI가 제공하는 DOM, ajax, setTimeout을 사용한다면 web Api는 코드를 처리후에 Message Queue에 콜백을 추가한다.
Message Queue에 있는 콜백은 call Stack이 비어있는 상태가 될 떄 까지 기다렸다가 call Stack에 추가된다.

```javascript
setTimeout(() => {
  console.log("hello");
}, 1000); //delay >= 1000ms
```

따라서 위와 같은 code를 실행하였을 때 최소 delay인 1000ms는 web Api에서 보장되지만, Message queue에서 Call Stack이 비어있을때 까지 기다려야하므로, 추가적인 delay가 발생할 수 있다.
