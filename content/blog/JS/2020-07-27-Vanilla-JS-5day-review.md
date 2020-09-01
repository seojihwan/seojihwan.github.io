---
title: '[JS] Vanilla JavaScript day 5: Flex Panels Image Gallery'
date: 2020-07-27 16:07:91
category: JS
draft: false
---

### day 5: Flex Panels Image Gallery

CSS의 Flexbox layout을 이용하여 이미지 갤러리를 만들어 보았다.

[Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

![image](/image/day5-1.png)

부모 요소에 display: flex, flex-direction을 row로 설정하면 하나의 행안에 여러 열의 box형태로 layout을 구성할 수 있다.

또한 flexbox 내부의 요소들은 길게 늘어나는 특성을 갖고있고, 자식요소에 flex: 1을 부여함으로 써 각각의 비중을 갖게 해 주었다.

![image](/image/day5-2.png)

align-items: center를 이용해 내부 p태그들을 가운데 정렬하고
text-align: center를 이용해 p태그들의 innerText들을 가운데 정렬하였다.
justify-content: space-between을 이용해 처음과 마지막 p 태그는 위 아래 끝에 붙이고, 가운데 p는 중간에 배치하였다.

![image](/image/day5-1.png)

위의 그림처럼 초기의 위 아래 p 태그들은 숨겨져 있는 상태에서, 클릭되었을 때 transform을 통해 나타나도록 설정해보자.

```css
.panel p:first-child {
  transform: translateY(-100%);
}

.panel p:last-child {
  transform: translateY(100%);
}
```

먼저 first-child와 last-child를 이용해 Y축으로 이동하고
panel이 클릭되었을 때 classList를 추가하여 transform값을 0으로 덮어씌운다.

```js
const panels = document.querySelectorAll('.panel')

function toggleOpen(e) {
  this.classList.toggle('open')
}

// transition이 일어나면 이벤트의 propertyName이 브라우저에 따라 flex 또는 flex-grow로 나타나게 된다.
// 크로스 브라우징을 고려하여 includes('flex')를 이용해 모든 브라우저에서 동작할 수 있도록 한다.
function toggleActive(e) {
  if (e.propertyName.includes('flex')) {
    this.classList.toggle('open-active')
  }
}

// 클릭이 일어나면 panel의 classList에 open을 추가하여 flex비중과 font-size를 바꾸고, transition이 끝난 이후에, 양 끝의 숨겨진 p tag들이 나타나도록 설정하였다.
panels.forEach(panel => panel.addEventListener('click', toggleOpen))
panels.forEach(panel => panel.addEventListener('transitionend', toggleActive))
```

```css
.panel.open-active p {
  transform: translateY(0);
}
```

classList.toggle은 해당 클래스 이름이 없을 경우에는 만들어주고 true반환, 있을 경우에는 제거하고 false를 반환해주기 때문에 두 가지 상태를 번갈아가며 바꾸어줄때 편리하다.

[w3schools](https://www.w3schools.com/jsref/prop_element_classlist.asp)

최종 결과

![image](/image/day5-4.gif)
