---
title: '[JS] Vanilla JavaScript day 4: Array Carido Day1'
date: 2020-07-26 22:20:00
category: 'JS'
draft: false
---

### day 4: Array Carido Day1

day 4에서는 Array의 method(filter, map, reduce, sort)들을 사용해 보았다.

주어진 data는 다음과 같다.

```javascript
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
  { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
  { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
  { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
  { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
  { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
  { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
  { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
  { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
  { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
  { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 },
]
```

```javascript
const people = [
  'Beck, Glenn',
  'Becker, Carl',
  'Beckett, Samuel',
  'Beddoes, Mick',
  'Beecher, Henry',
  'Beethoven, Ludwig',
  'Begin, Menachem',
  'Belloc, Hilaire',
  'Bellow, Saul',
  'Benchley, Robert',
  'Benenson, Peter',
  'Ben-Gurion, David',
  'Benjamin, Walter',
  'Benn, Tony',
  'Bennington, Chester',
  'Benson, Leana',
  'Bent, Silas',
  'Bentsen, Lloyd',
  'Berger, Ric',
  'Bergman, Ingmar',
  'Berio, Luciano',
  'Berle, Milton',
  'Berlin, Irving',
  'Berne, Eric',
  'Bernhard, Sandra',
  'Berra, Yogi',
  'Berry, Halle',
  'Berry, Wendell',
  'Bethea, Erin',
  'Bevan, Aneurin',
  'Bevel, Ken',
  'Biden, Joseph',
  'Bierce, Ambrose',
  'Biko, Steve',
  'Billings, Josh',
  'Biondo, Frank',
  'Birrell, Augustine',
  'Black, Elk',
  'Blair, Robert',
  'Blair, Tony',
  'Blake, William',
]
```

```javascript
const data = [
  'car',
  'car',
  'truck',
  'truck',
  'bike',
  'walk',
  'car',
  'van',
  'bike',
  'walk',
  'car',
  'van',
  'car',
  'truck',
]
```

#### 1. filter()를 이용해 15세기에 태어난 발명가들을 구하기

```javascript
// Array.prototype.filter()
// 1. Filter the list of inventors for those who were born in the 1500's
const fifteen = inventors.filter(
  inventor => inventor.year >= 1500 && inventor.year < 1600
)
console.table(fifteen)
```

![image](/image/day4-1.PNG)

filter를 이용하여 각 element에 통과할 수 있는 조건을 arrow function으로 전달한다면 해당 조건이 만족하는 element로 만들어진 배열이 반환됨을 확인할 수 있다.

#### 2. map()을 이용하여 발명가의 첫번째와 마지막 이름에 대한 배열을 얻기

```javascript
// Array.prototype.map()
// 2. Give us an array of the inventor first and last names
const names = inventors.map(inventor => [inventor.first, inventor.last])
console.table(names)
```

![image](/image/day4-2.PNG)

map함수에 각 element의 first와 last라는 속성을 배열로 감싸주어 반환하는 arrow function을 전달하였다.

#### 3. sort()를 이용하여 발명가 생년월일 순 정렬하기

```javascript
// Array.prototype.sort()
// 3. Sort the inventors by birthdate, oldest to youngest
const orderedInventors = inventors.sort((a, b) => (a.year > b.year ? 1 : -1))
console.table(orderedInventors)
```

![image](/image/day4-3.PNG)

JavaScript에서 sort함수는 elements들을 string으로 변환후 UTF-16 code에 따라 오름차순 정렬된다.

따라서 생일에 따라 정렬하기 위해서 compareFunction을 전달하였다.
compareFunction에서 0보다 큰 수를 반환하면 b가 a 보다 앞의 index를 갖게되고, 0보다 작은 수를 반환하면 그 반대, 0을 반환하면 a,b의 자리 이동은 없다.

또한 a,b를 출력해보면 a는 배열의 두번째 요소 부터 마지막, b는 1번 부터 a의 전까지의 요소를 확인하기 때문에 최악의 경우 시간복잡도는 O(n<sup>2</sup>)임을 알 수 있다.

#### 4. reduce()를 이용하여 발명가의 생애 나이 합하기

```javascript
// Array.prototype.reduce()
// 4. How many years did all the inventors live all together?
const sumOfYears = inventors.reduce((accmulator, current) => {
  return accmulator + current.passed - current.year
}, 0)
console.log(sumOfYears) //861
```

reduce는 accumulator의 초기값을 지정하고, 각 element에 접근한 후
acuumulator를 반환할 수 있게 해주는 함수이다.

각 element의 생애 나이들을 accumulator에 더해주고 반환하며 초기 accumulator 값을 0으로 설정하여 861이라는 최종 결과를 얻었다.

#### 5. sort()를 이용하여 발명가의 생애 나이로 정렬하기

```javascript
// 5. Sort the inventors by years lived

const orderedLivedInventors = inventors.sort((a, b) =>
  a.passed - a.year > b.passed - b.year ? 1 : -1
)
console.table(orderedLivedInventors)
```

![image](/image/day4-4.PNG)

3번과 유사한 문제이다.

#### 6. 밑의 url에 접속하여 Boulevard in Paris들에 대하여 이름에 'de'가 포함된 목록 만들기

```javascript
// 6. create a list of Boulevards in Paris that contain 'de' anywhere in the name
// https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris
const links = Array.from(
  document.querySelector('.mw-category').querySelectorAll('a')
)
const de = links
  .filter(link => link.innerText.includes('de'))
  .map(link => link.innerText)
console.table(de)
```

![image](/image/day4-5.PNG)

Boulevard 이름들은 a tag안에 텍스트로 저장되어있는데,
상위 category를 querySelector로 접근하고 모든 a tag들을 querySelectorAll을 이용하여 NodeList로 반환 받았다.

NodeList는 filter와 map method가 없기 때문에 Array.from을 이용하여 변환하였다.

String에 존재하는 includes함수를 이용하여, 'de'를 포함하는지 filter로 걸러내고, 해당 Boulevard 이름을 mapping하여 반환하였다.

#### 7. last name의 알파벳 순서로 정렬하기

```javascript
// 7. sort Exercise
// Sort the people alphabetically by last name

const alphaOrdered = people.sort((a, b) => {
  return a.split(', ')[1] > b.split(', ')[1] ? 1 : -1
})
console.table(alphaOrdered)
```

people에서 각 element 들의 이름은 'Beck, Glenn' 이런식으로 기록되어 있기 때문에 split(', ')을 통해 last name을 추출할 수 있다.
이를 이용하여 정렬하면 3,5번과 같이 정렬됨을 알 수 있다.

#### 8. data의 instance들을 reduce를 이용하여 object로 (key 와 count)로 표현하기

```javascript
// 8. Reduce Exercise
// Sum up the instances of each of these

const sumOfData = data.reduce((all, data) => {
  if (!all[data]) {
    all[data] = 1
  } else {
    all[data] += 1
  }
  return all
}, {})

console.log(sumOfData) // {car: 5, truck: 3, bike: 2, walk: 2, van: 2}
```

초기 accumulator는 빈 object를 주고, 각각의 key가 없다면 초기값 1을 부여하고, 있다면 1을 추가해 주는 방식을 사용한다.
