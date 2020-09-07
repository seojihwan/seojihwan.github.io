---
title: "[JS] Vanilla JavaScript day 3: Update Css Variables with JS"
date: 2020-07-25 22:46:00
category: 'JS'
draft: false
---

### day 3: Update CSS Variables with JS

day 3을 통해 CSS에 변수를 처음 사용해 보았다.

관련 링크
[w3schools](https://www.w3schools.com/css/css3_variables.asp)

:root에 --를 이용하여 global custom property를 정의하고,

var()함수를 이용해 property를 사용할 수 있다.

```css
:root {
  --base: #ffc600;
  --spacing: 10px;
  --blur: 10px;
}

img {
  padding: var(--spacing);
  background: var(--base);
  filter: blur(var(--blur));
}

.hl {
  color: var(--base);
}
```
해당 property를 JavaScript로 적절히 수정해보자

각각의 property값을 변경하기 위한 type: range input 2개를 type: color input 1개를 만들어 주었다.

드래그를 통해 수정해 주기 위해서는 2개의 이벤트 리스너(change, mousemove)가 필요하다. 

```javascript
const inputs = document.querySelectorAll('.controls input');

//변경 후 값을 바꾸기 위한 change 리스너
inputs.forEach((input) => input.addEventListener('change', handleUpdate));
// 드래그 도중 값을 바뀌는 것을 보기 위한 mousemove 리스너
inputs.forEach((input) => input.addEventListener('mousemove', handleUpdate));

```

querySelectorAll은 NodeList를 반환하는데 array와의 차이점은 사용할 수 있는 method가 다르다는 점이 있다.

```javascript
console.log(inputs);
console.log(Array.from(inputs));
```

NodeList의 method
![image](/image/day3-1.png)

array의 method
![image](/image/day3-2.png)

처음에 이벤트리스너의 콜백함수로 arrow function을 사용하였더니 this가 dom요소를 나타내지 않고 Window를 나타내었다. 
공부 끝에 arrow function은 lexical context에의해 this가 정적으로 결정됨을 알게 되었다. 

[자바스크립트 this란](https://seojihwan.github.io/JS/2020-07-25-this/)

따라서 function을 콜백함수로 사용하였다.

```javascript
function handleUpdate() {
  console.log(this);
  console.log(this.dataset);
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty(
    `--${this.name}`,
    this.value + suffix
  );
}
```
dataset은 Web API가 제공하는 read-only의 property이다. html에서 'data-*'으로 기존에 설정된 데이터를 suffix로 받아왔다.

```html
<input
        id="blur"
        type="range"
        name="blur"
        min="0"
        max="25"
        value="10"
        data-sizing="px"
      />

```
또 document.documentElement를 통해 HTML에 접근하고   document.documentElement.style.setProperty를 이용해 style 값을 설정해 주어 CSS 변수의 값을 수정하였다.

![image](/image/day3-3.png)

결과 화면
![image](/image/day3.gif)

