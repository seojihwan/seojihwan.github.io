---
title: "[JS] Vanilla JavaScript day 1: Drum Kit"
date: 2020-07-22 18:16:00
category: 'JS'
draft: false
---

### day 1 Drum Kit

day 1은 keydown 이벤트를 활용하여 특정 키를 눌렀을 때 드럼의 소리가 나오도록 만드는 과정을 해보았다.



키보드의 A,S,D,F,G,H,J,K,L을 눌렀을 때 서로 다른 audio가 동작한다.

먼저 각 키보드 입력의 keyCode와 동작시킬 사운드 파일의 이름을 배열로 만들어주었다.
```javascript
// array for keyCode
const keys = [65, 83, 68, 70, 71, 72, 74, 75, 76];
// array for soundFile
const keyNames = [
  'boom',
  'clap',
  'hihat',
  'kick',
  'openhat',
  'ride',
  'snare',
  'tink',
  'tom',
];
```

element로는 입력 키보드 자판을 알려주는 keyIdx, 동작할 드럼의 이름인 drumName
이를 감싸는 각각의 keyboard, 모든 키를 감싸는 keysWrapper 라는 div들과
src로 wav파일을 갖는 audio를 사용하였다.

```javascript
// wrapper Div
const keysWrapper = document.createElement('div');
keysWrapper.className = 'keysWrapper';
document.body.appendChild(keysWrapper);

// create keyboard and audio element.
keys.forEach((key, idx) => {
  const keyboard = document.createElement('div');
  const audio = document.createElement('audio');
  const keyIdx = document.createElement('div');
  const drumName = document.createElement('div');
  drumName.innerHTML = keyNames[idx];
  drumName.className = 'drumName';
  keyIdx.innerHTML = String.fromCharCode(key).toUpperCase();
  keyIdx.className = 'keyIdx';
  keyboard.className = 'key';
  keyboard.appendChild(keyIdx);
  keyboard.appendChild(drumName);
  keyboard.setAttribute('keyCode', key);
  audio.src = `./sound/${keyNames[idx]}.wav`;
  audio.setAttribute('keyCode', key);
  document.body.appendChild(audio);
  keysWrapper.appendChild(keyboard);
});
```
array.forEach는 필수 옵션으로 array의 element를 갖고, 선택 옵션으로 ,idx와 배열 전체의 요소를 갖을 수 있다.

이를 통해 반복요소를 줄일 수 있다.

innerHTML은 dom을 rebuild하는 method로 기존의 content는 사라진다.

appendChild는 해당 element의 마지막 자식 뒤에 element를 추가하는 method이다.

setAttribute는 임의의 property를 지정하여, 2번째 요소로 값을 바꿀수 있다.


window에 keydown 이벤트리스너를 추가하고, keydown이 발생하였을 경우
눌러진 키에 playing이라는 classList를 추가하여, 화면에서 해당 key를 표현하고, audio가 실행되도록 하였다.
```javascript
// add classList to downKey and play audio using event.keyCode.
const playAudio = (e) => {
  const downKey = document.querySelector(`.key[keyCode="${e.keyCode}"]`);
  if (!downKey) return;
  downKey.classList.add('playing');
  const audio = document.querySelector(`audio[keyCode="${e.keyCode}"]`);
  if (!audio) return;
  audio.currentTime = 0;
  audio.play();
};


// window has a keydown EventListner
window.addEventListener('keydown', playAudio);
```

querySelectorAll을 이용하여 NodeList에 접근하고, 각각의 요소에 transitionend라는 이벤트리스너를 추가하였다.

```javascript
const removeTransition = (e) => {
  if (e.propertyName !== 'transform') return;
  e.target.classList.remove('playing');
};

// each keyboards has an transitionend EventListner.
const keyboards = document.querySelectorAll('.key');
keyboards.forEach((keyboard) => {
  keyboard.addEventListener('transitionend', removeTransition);
});
```

이를통해 playAudio함수가 동작하고 playing이라는 classList가 추가되면
CSS에 의해 transition이 발생하고, transition이 end되었을 때 
transitioned 이벤트 리스너가 동작하여, playing이라는 classList를 다시 제거해 
주게 된다.

```CSS
.playing {
  transform: scale(1.1);
  border-color: #ffc600;
  box-shadow: 0 0 1rem #ffc600;
}
```

결과 화면

![image](/image/drumkit.gif)

