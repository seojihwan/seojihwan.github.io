---
title: '[JS] Vanilla JavaScript day 6: Ajax Type Ahead'
date: 2020-07-30 13:07:91
category: JS
draft: false
---

### day 6: Ajax Type Ahead

day 6에서는 api를 통해 도시들의 정보들을 가져오고 검색을 통해 원하는 도시를 검색해 보았다.

```js
const endpoint =
  'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json'
```

위의 url을 통해 data를 전달 받을 수 있다.
fetch는 비동기 처리를 돕기 위해, promise를 return한다.

```js
const cities = []
fetch(endpoint)
  .then(blob => blob.json())
  .then(data => cities.push(...data))
```

스트림을 모두 전달받고 JSON으로 바꾼다. 최종적으로 cities가 참조하는 배열에 데이터를 추가한다.
promise의 then을 이용해 위 작업은 동기적으로 수행된다.

[promise란](https://seojihwan.github.io/JS/2020-07-29-Promise/)

이제 추가된 정보를 form의 input에서 검색할 수 있도록 만들어보자.

```js
function findMatches(wordToMatch, cities) {
  return cities.filter(place => {
    const regex = new RegExp(wordToMatch, 'gi')
    return place.city.match(regex) || place.state.match(regex)
  })
}

function displayMatches() {
  const matchArray = findMatches(this.value, cities)
  const html = matchArray
    .map(place => {
      const regex = new RegExp(this.value, 'gi')
      const cityName = place.city.replace(
        regex,
        `<span class="hl">${this.value}</span>`
      )
      const stateName = place.state.replace(
        regex,
        `<span class="hl">${this.value}</span>`
      )
      return `
      <li>
        <span class="name">${cityName}, ${stateName}</span>
        <span class="population">${numberWithCommas(place.population)}</span>
      </li>
    `
    })
    .join('')
  suggestions.innerHTML = html
}

const searchInput = document.querySelector('.search')
const suggestions = document.querySelector('.suggestions')

searchInput.addEventListener('change', displayMatches)
searchInput.addEventListener('keyup', displayMatches)
```

input에 keyup과 change라는 이벤트 리스너를 추가하고, input의 value를 이용해
정규표현식을 만들고 데이터를 replace하는 작업을 거친다.

population 데이터의 3자리 수마다 콤마를 추가해 주기 위해 numberWithCommas 함수를 이용한다.

```js
function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
}
```

정규표현식에 관한 정보는 MDN페이지에서 참고할 수 있다.

[MDN: RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

결과화면

![image](/image/day6.gif)
