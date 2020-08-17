---
title: '[JS] Vanilla JavaScript day 26: Stripe Follow Along Dropdown' 
date: 2020-08-17 23:29:17
category: 'JS'
draft: false
---

이번 day에서는 각 태그들에 대한 dropdown 효과를 구현해 보았다.

![image](./image/day26.gif)

#### 이벤트리스너
- mouseenter: 마우스가 a태그에 올라가면 해당 컨텐츠들의 opacity와 display값을 바꾸어 transition 효과를 보여준다.

- mouseleave: 위의 transition을 제거한다.

#### mouseenter vs mouseover
mouseenter는 해당element 에서만 동작을 하게되고, mouseover는 자식 element에서도 동작하는 특성이 있다.


```js
function handleEnter() {
  this.classList.add('trigger-enter');

  // 먼저 display 옵션을 block으로 바꾸어 준 후 약간의 딜레이를 주어 opacity값을 변경하여 transition효과를 나타낼 수 있도록 하였다.

  setTimeout(() => {
    this.classList.contains('trigger-enter') &&
      this.classList.add('trigger-enter-active');
  }, 100);
  background.classList.add('open');

  const dropdown = this.querySelector('.dropdown');
  dropdown.getBindinga;

  //getBoundingClientRect()를 통해 element의 viewport 정보, width, height값을 얻을 수 있다.
  const dropdownCoords = dropdown.getBoundingClientRect();
  console.log(dropdownCoords);
  const navCoords = nav.getBoundingClientRect();

  const coords = {
    width: dropdownCoords.width,
    height: dropdownCoords.height,
    top: dropdownCoords.top - navCoords.top,  // 만약 a 태그들이 포함된 nav의 시작점이 페이지 최상단이 아니라면, dropdown을 감싸는 배경의 좌표가 정확하지 않을 수 있다. 이를 방지하기 위해 nav의 최상단 viewport의 top 좌표만큼을 빼 준다.
    left: dropdownCoords.left - navCoords.left,
  };


  // dropdown 을 감싸는 배경의 크기는 contents의 크기와 같아야 한다.
  // element.style.setProperty를 이용하여 width, height, transform 값을 바꾸어 준다.
  background.style.setProperty('width', `${coords.width}px`);
  background.style.setProperty('height', `${coords.height}px`);
  background.style.setProperty(
    'transform',
    `translate(${coords.left}px,${coords.top}px)`
  );
}
```


```js
// mouseleave 의 콜백함수로 classList의 class이름을 제거해준다.
function handleLeave() {
  this.classList.remove('trigger-enter', 'trigger-enter-active');
  background.classList.remove('open');
}
```

---
네비게이션바가 항상 최 상단이 아닐수 있기 때문에 상황에 맞게 적용될 수 있도록 시작 위치의 크기를 빼서 dropdown을 적용하여 ui가 깨지는 현상을 대비할 수 있다. 

또한 display: none에서 display: block을 적용하면서 동시에
opacity값을 바꾸어 주면 transition을 확인할 수 없다는 사실을 알게되었다.


```js
setTimeout(() => {
  this.classList.contains('trigger-enter') &&
    this.classList.add('trigger-enter-active');
}, 100);
background.classList.add('open');
```

콜백함수인 handleEnter함수의 내부에 있는 setTimeout에서
classList에 trigger-enter이 존재하는지 체크를 한 후 active효과를 나타낼 수 있도록 && 연산자를 사용하면 더욱 안정적인 구현이 가능하다.
