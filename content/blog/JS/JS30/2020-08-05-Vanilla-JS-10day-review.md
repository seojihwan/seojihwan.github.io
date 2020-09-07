---
title: '[JS] Vanilla JavaScript day 10: Hold Shift to Check Multiple Checkboxes'
date: 2020-08-05 15:36:14
category: JS
draft: false
---

### day 10: Hold Shift to Check Multiple Checkboxes

day 10에서는 리스트들을 한번에 체크할 수 있는 함수를 만들어 보았다.

결과 화면
![image](./day10.gif)

시프트 키를 누른상태에서 2번째 리스트를 클릭하게되면 해당 범위의 리스트들이 모두 체크되는 것을 보여준다.

이를 위해서 클릭된 1번째 2번째의 리스트 정보를 저장하고 시프트 키 입력을 탐지하여 두 리스트 사이의 정보를 check 해줘야 한다.

```js
const checkboxes = document.querySelectorAll('.inbox input[type="checkbox"]');

let lastChecked;  // 전역변수로 사용
```

```js
checkboxes.forEach((checkbox) => checkbox.addEventListener('click', handleCheck));
```
각각의 checkbox들은 querySelectorAll을 이용하여 NodeList를 만들고
forEach를 이용하여 각각의 dom에 click 이벤트 리스너를 추가한다.

```js
function handleCheck(e) {
  let inBetween = false;  //콜백함수 내에서 각 checkbox의 상태를 확인
  if (e.shiftKey && this.checked) {
    checkboxes.forEach((checkbox) => {
      if (checkbox === this || checkbox === lastChecked) {
        inBetween = !inBetween;
      }

      if (inBetween) {
        checkbox.checked = true;
      }
    });
  }
  lastChecked = this.checked ? this : undefined;
}

```
lastCheckd변수는 전역변수로 사용하고
inBetween변수는 콜백함수 내에서 두 아이템 사이에 있는지의 상태를 확인할 때 사용한다.

forEach는 순차적으로 element에 접근한다. 첫번째 아이템에 접근하였을 때 inBetween을 true로 두번째 아이템에 접근했을 때 false로 바꾸어주면 아이템의 상태를 올바르게 파악할 수 있다.

각 아이템의 상태에 맞게 checked처리를 해주면 위의 동작을 구현할 수 있다.