---
title: "[JS] Vanilla JavaScript day 2: Clock"
date: 2020-07-24 15:59:00
category: 'JS'
draft: false
---

### day 2: Clock

div로 원 표현하기위해 height와 width를 같게 지정하고 border-radius값을 50%로 설정한다.

```css
.circle {
  height: 250px;
  width: 250px;
  border: solid;
  border-width: 15px;
  border-color: #ffffff;
  border-radius: 50%;
  display: inline-block;
  position: relative;
}
```


![image](/image/clock.png)

시침,분침,초침의 position은 absolute로 설정한다.
부모의 position을 relative로 설정하고, 시침,분침,초침의 top 값을 50%로 설정하고 적절한 width 값과 left값을 설정하면 모든 침이 중심에서 9시방향을 가르키게 된다.

[w3shools링크](https://www.w3schools.com/cssref/trycss3_transform-origin_inuse.htm)

```css
.circle > div {
  position: absolute;
  top: 50%;
  height: 5px;
  transform-origin: 100% 50%;
  border: none;
  transition: transform 0.5s cubic-bezier(0.27, 1.57, 0.58, 1);
}
```
회전을 나타내기 위해서는 transform의 rotate를 이용한다.
이때 기준점은 해당 element의 중심이 기본이기 때문에
transform-origin을 100% 50%로 설정하여, div의 오른쪽 중간지점(원점)을 중심으로 회전할 수 있도록 설정하였다.

transition을 이용하여 침이 역동적으로 움직이게 할 수 있다.

![image](/image/cubic-bezier.png)

개발자도구 에서 기본 옵션과 움직임을 설정할 수 있다.

setInterval 함수를 이용해서 1초마다 각각의 transform style 속성값을 변경하여 변화하는 시계의 침을 표현할 수 있다.

하지만 시,분,초가 0이 되는 시점에 deg값이 급변하므로 각각의 침의 움직임이 부자연스러운 것을 확인하였다. 

![image](/image/clock.gif)

이를 해결하기 위해서 inline style 방식으로, 침이 12시를 가리키는 각도에서 transition 옵션을 제거해주었다. 

```javascript
const secondHand = document.querySelector('.sec');
const minuteHand = document.querySelector('.min');
const hourHand = document.querySelector('.hour');
const setTransition = (hand, degree) => {
  if (degree === 90) {
    hand.style.transition = '';
  } else {
    hand.style.transition = 'transform 0.5s cubic-bezier(0.27, 1.57, 0.58, 1)';
  }
};

const setDate = () => {
  const now = new Date();
  const seconds = now.getSeconds();
  const minutes = now.getMinutes();
  const hours = now.getHours();

  const secondsDegree = (seconds / 60) * 360 + 90;
  const minutesDegree = (minutes / 60) * 360 + 90;
  const hoursDegree = (hours / 60) * 360 + 90;

  setTransition(secondHand, secondsDegree);
  setTransition(minuteHand, minutesDegree);
  setTransition(hourHand, hoursDegree);
  
  secondHand.style.transform = `rotate(${secondsDegree}deg)`;
  minuteHand.style.transform = `rotate(${minutesDegree}deg)`;
  hourHand.style.transform = `rotate(${hoursDegree}deg)`;
};

setInterval(setDate, 1000);
```

시,분,초가 0으로 초기화 되지 않고 계속해서 늘어나는 방식을 사용할 수도 있었지만 숫자가 계속해서 커지기 때문에 사용하지 않았다.

![image](/image/clock2.gif)