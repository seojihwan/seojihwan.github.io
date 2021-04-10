---
title: "[woowa-course] 리뷰 피드백 정리 (로또)"
date: 2021-04-10 12:10:34
category: 'woowa-course'
draft: false
---

### 로또 리뷰 정리
https://github.com/woowacourse/javascript-lotto/pulls

#### [#2. style을 직접 수정하지 않기](https://github.com/woowacourse/javascript-lotto/pull/2#discussion_r578443918)

style의 display값을 직접 수정하는 것보다 다음의 클래스를 toggle하는 방식을 사용하자
```css
.hidden {
  display: none;
}
```

#### [#8. 파일 네이밍 컨벤션](https://github.com/woowacourse/javascript-lotto/pull/8#discussion_r579612687)


컨벤션은 코드에서 일관성만 유지된다면 크게 상관은 없지만, 섞어서 쓴다면 문제가 있을 수 있다.
PascalCase는 클래스명, 컴포넌트명 등 특수한 경우에 사용되고 대부분의 자바스크립트 스타일가이드와 lint에서 camelCase를 사용을 권장한다. snake_case는 잘 쓰지 않는다.

#### [#13. 함수 네이밍](https://github.com/woowacourse/javascript-lotto/pull/13#pullrequestreview-594800936)

메서드의 네이밍에 로직을 드러내지마라
택시기사가 목적지에 도달하기위해 액셀, 브레이크, 핸들 돌리기 등등 많은 동작을 수행한다.
이 동작에 대한 네이밍은 accelAndTurnHandle... 보다는 drive가 적절하다.
하나하나의 행위들은 사용자의 관심사가 아니다.

단일 책임이라는 것은 한 가지 일이아니라 한 가지 책임이라는 뜻이다.

#### [#13. 의존성주입](https://github.com/woowacourse/javascript-lotto/pull/13#discussion_r579741113)

constructor에서 객체를 생성하여 할당하는 것이 아니라 constructor외부에서 객체를 생성하고 인자로 넘겨주는 것이다.

이렇게 할 경우, 의존성을 유연하게 가져갈 수 있어 해당 클래스의 재사용성을 높일 수 있을 것이다.

#### [#25. 함수 작성 순서](https://github.com/woowacourse/javascript-racingcar/pull/25#discussion_r576201158)
파라미터의 값이 undefined, null인 경우 예외처리를 어떻게 해야할까?

함수를 작성하는 순서

- 어떤 행동을 하는 함수인지 정의
- 어떤 parameter가 필요한지 정의
- parameter에 대한 예외에 대하여 bad case를 정의하고 예외처리

TDD를 할 때 많이 쓰이는 기법이며 알고리즘에서도 유용하다.

#### [#31. form reset](https://github.com/woowacourse/javascript-lotto/pull/31#discussion_r584526090)

form element의 reset메서드 또는 button type="reset"을 통해 form을 초기화 할 수 있다.
```HTML
<!-- 폼 태그에서 id 부여 -->
<form id="winning-number-form">
</form>

...
<!-- 모달버튼에서 form 속성 부여 (연결할 form의 id와 같아야 함) -->
<button type="reset" form="winning-number-form">다시 시작하기</button>
```

#### [#48](https://github.com/woowacourse/javascript-lotto/pull/48#pullrequestreview-600209699)

##### 크로스 브라우징
브라우저 지원률도 중요하지만 비즈니스의 사용자가 어떤 디바이스를 많이 사용하는지도 파악할 필요가 있다.

##### 내장객체에 메서드 추가

- Object.assign:
기존 객체에 새로운 메서드를 추가하거나 덮어씌울 수 있는 방식이다.
이때 덮어씌움을 방지하려면 key값으로 Symbol을 이용할 수 있다.
- proxy:
기존 객체를 wrapping하는 객체를 생성한다.

#### [#54. 클래스내 멤버변수, 멤버함수 선언 순서](https://github.com/woowacourse/javascript-lotto/pull/54#discussion_r584237983)

사용처를 기준으로 생각해보면, 클래스가 어떤 함수를 가지고 있는지 알아야 하니, 가장 우선으로는 public method - 상속받는 곳에서 알아야하니 protected method - 내부에서 작업할 때에만 쓰이는 private method - 변수는 외부에서 알아야 할 필요 없고, 내부에서만 확인하면 되니 member variable순서로 작성해 보자

- public
- protected
- private
- member variable

#### [#59. 변수 선언 상황](https://github.com/woowacourse/javascript-lotto/pull/59#discussion_r585387933)

변수는 어떤 상황에 선언되어야 할까?

- 2번 이상 사용될 때 성능을 위해서
- 변수이름을 통한 의미 전달을 위해

단 한번만 사용되더라도 명확한 의미전달을 위해서는 변수로 선언 하는 것이 좋다.

#### 후기

자동차 경주 게임 미션 리뷰를 정리할 때 보다 글의 양이 많이 줄었다.
기존의 피드백에서 중복되는 것들이 많았고 내가 새로 깨닫는 것들이 줄었음을 느꼈다.

점점 투자하는 시간대비 효율은 떨어진다고 생각되었지만, 여전히 새로 얻어가는 것들은 충분히 많다.

다른 크루들의 리뷰를 열심히 보는 것도 중요하지만 내 코드에 대한 리뷰와 정리를 소홀히 하고 있음을 깨달았다.

추후 내 코드와 리뷰에 대해서 집중적으로 정리하는 시간을 갖아야 겠다.