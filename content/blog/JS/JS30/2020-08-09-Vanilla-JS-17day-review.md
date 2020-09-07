---
title: '[JS] Vanilla JavaScript day 17: Sort Without Articles' 
date: 2020-08-09 20:53:12
category: JS
draft: false
---

### day 17: Sort Without Articles

다음 band들을 Article을 제외한 String으로 정렬해보자

```js
const bands = [
  'The Plot in You',
  'The Devil Wears Prada',
  'Pierce the Veil',
  'Norma Jean',
  'The Bled',
  'Say Anything',
  'The Midway State',
  'We Came as Romans',
  'Counterparts',
  'Oh, Sleeper',
  'A Skylit Drive',
  'Anywhere But Here',
  'An Old Dog',
];
```

The, A, An으로 시작되는 문자를 제외하고 나머지의 문자로 정렬을 해야한다.
하지만 `Anywhere But Here`는 A로 시작하지만 제외해서는 안된다.
따라서 각 Article 뒤에 공란을 포함한 상태로 문자열을 체크해야 한다.

replace에 정규식을 이용하여 간단하게 표현할 수 있다.

```js
function strip(name) {
  return name.replace(/^(a |the |an )/i, '').trim();
}
```
strip이라는 함수는 문자열을 받고 'a ', 'the ', 'an '으로 시작 되는지를 대소문자에 상관없이 확인하고, 공백을 없애서 반환해준다.

^를 통해 input의 시작점을 확인하고, i 플래그는 대 소문자를 구별하지 않기위해 사용된다.


```js
const sortedBands = bands.sort((a, b) => (strip(a) > strip(b) ? 1 : -1));

document.querySelector('#bands').innerHTML = sortedBands
  .map((band) => `<li> ${band} </li>`)
  .join('');
```

정렬된 band들의 정보들을 map을통해 li태그로 구성된 array를 만들고 join하여 하나의 스트링으로 만든다.

li태그들을 innerHTML을 이용해 ul의 child로 구성한다.


#### 결과화면
![image](./image/day17.PNG)