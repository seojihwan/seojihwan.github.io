---
title: "[woowa-course] 리뷰 피드백 정리 (레이싱 경주 게임)"
date: 2021-02-18 22:36:59
category: 'woowa-course'
draft: false
---

### 레이싱 경주 게임 리뷰 정리


#### 2. 

document.querySelector와 같이 반복되는 코드는 shorcut 만들기

```js
const $ = selector => document.querySelector(selector);
```

Private class fields는 Chrome과 Edge만 지원하고 있다.

내가 작성하는 코드의 브라우저 지원범위를 생각해보자

테스트는 주어진 상황에서 예측가능한 상태(predictable state)를 검증하기위한 방법이기 때문에 무작위성 자체를 테스트하기 어렵다.

#### #3.
테스트의 설명은 어떤 것을 디테일하게 써보자

#### #5.
객체생성 function vs class
취향이긴 하지만 class가 은닉 및 상속에 좋다

#### #6.
함수의 이름은 isValid... 이라서 검증 하는것처럼 작성되었지만
alert라는 사이드이펙트를 발생시키고 있음
함수의 이름에 맞게 한가지 동작만 시켜보자

#### #7.
유저는 개발자가 생각한대로 움직이지 않는다.
유저가 입력한 값에 대한 검증은 빡빡하게 해야한다.
단순 for문은 실수하기 쉽고 실무에서도 잘 사용되지 않는다. 배열의 메소드를 이용하자

#### #8.
중복이 허용되지 않는 자료구조에서 Set을 활용해보자

#### #13.
[의존 관계에 있는 코드의 물리적 거리는 가까울수록 좋다](https://github.com/woowacourse/javascript-racingcar/pull/13#discussion_r573681194)


#### #16
[validate 함수](https://github.com/woowacourse/javascript-racingcar/pull/16#pullrequestreview-588368246)
validate 함수는 보통 error 객체를 반환하고, 에러가없을 땐 undefined, 에러가 있을 땐 message를 error 객체에 담아서 반환합니다.

[의존적인 코드 배치](https://github.com/woowacourse/javascript-racingcar/pull/16#discussion_r574426849)
getWinners는 RacingGame이라는 도메인의 로직이라 util이라는 폴더에 있는게 어색해보여요.
어떤 폴더에 위치시키는게 더 적절할까요?

[일반적으로 React Component가 DOM tree에 붙을 때를 mount된다 라고 표현합니다.](https://github.com/woowacourse/javascript-racingcar/pull/16#discussion_r574435562)

[사이드 이펙트](https://github.com/woowacourse/javascript-racingcar/pull/16#discussion_r574445760)
setState라는 함수가 함수 이름대로 상태를 변경하는 것 외에 play()라는 다른 사이드이펙트를 일으키고있네요.
setState라는 함수이름만 보고 사용하는 사람 입장에서는 의도하지 않은 동작을 일으킬수도 있을 것같은데요? 👀

[함수 vs 클래스](https://github.com/woowacourse/javascript-racingcar/pull/16#discussion_r574457877)
이런 함수들은 this를 전혀 사용하지 않고 클래스 인스턴스 내부에 접근하지 않는 것 같은데요.
클래스 메소드로 선언할 필요가 있을까요?

[테스트의 제목](https://github.com/woowacourse/javascript-racingcar/pull/16#discussion_r574466572)

테스트의 제목은
주어진 상황(Given)에서
~~한 후(When),
~~한다(or 해야한다). (Then) 형태로 작성하는게 좋습니다.
(ref: https://kchanguk.tistory.com/40)

즉, "car-name-input에 아무것도 입력하지 않고, 확인 버튼을 클릭한 경우, 에러메시지를 출력한다."
라고 명확하게 작성하는것이 좋습니다.

테스트의 제목도 세가지를 모두 표현하도록 작성하고,
실제 테스트코드를 작성할 때에도 세가지를 기억하면서 작성하도록 의식적으로 노력해보세요 👍


[에러메시지 객체의 KEY 값은 "자연수가 아닙니다"로 해석되는데, 실제 Value 값은 "자연수를 입력해주세요."로
둘이 다르네요.
좋은 에러메시지는 어느 부분이 잘못되었는지 안내하고, 맞는 방향으로 제안하는 것이 좋은 에러메시지입니다.
(우리가 가끔 코딩하다가 만나는 에러메시지를 생각해보세요!)
즉, "입력한 값(N)은 자연수가 아닙니다. @@@는 자연수여야 합니다." 와 같은 에러메시지가 더 좋을 것같아요.
다른 에러메시지들도 같이 고쳐보세요 👍])https://github.com/woowacourse/javascript-racingcar/pull/16#discussion_r574777338)


#### #17.
한 곳에서 관리한다는것은 다른 곳에서는 관리를 하지 않고 값을 받아서 써야 하는데요! 예를 들면 RacingResult 이 this.cars 에 부모의 값을 다시 저장해서 같이 관리를 하는 모습이 보여요. 이런건 한 곳에서 관리한다고 말할 수 없다고 보여집니다 !
객체지향적으로 짜려고 했다면 부모(App)는 자식들을 관리하는 역할을 해야합니당 :)
리뷰 남긴 내용 고민해주셔서 감사합니다 👍

#### #18.
[end of line을 권장](https://github.com/woowacourse/javascript-racingcar/pull/18#discussion_r573697253)
개행문자가 없어서 한 줄이 끝나지 않은 것으로 인식하는 경우가 있을 수 있음 
가독성이 좀 더 좋아짐 위 2가지의 이유로 end of line을 권장


[함수 표현식, 함수 선언식 고민](https://github.com/woowacourse/javascript-racingcar/pull/18#discussion_r573838645)

pair와 함수 표현식, 함수 선언식중에 많은 고민을 했던것 같아요.
저희는 함수 표현식을 채택하여 사용했어요!
2개의 차이점은 호이스팅 여부로 이해했는데요.
호이스팅이 되면 함수를 사용하기 전에 반드시 함수를 선언해야한다.라는 규칙을 위반하고 가독성도 떨어지는 걸로 알고있어요!
그래서 var사용을 지양하는 것처럼 함수 선언식을 지양해야된다고 생각해요!]

#### #19.
[Lifecycle과 관련되어 생각해보기](https://github.com/woowacourse/javascript-racingcar/pull/19#discussion_r574365001)
요러한 것은 다양한 라이브러리에서 일컫는, Lifecycle과 관련되어 생각해보시면 좋아 보입니다.

우리가 흔히 많이쓰는 프레임워크는 render -> mount 형태로 dom 객체를 생성 후, mount 과정에서 이벤트를 바인딩하게 됩니다.
그러한 과정에서 현재 구현해주신 addListeners는 mount 함수와 같은 역할을 진행하는 것 같습니다 :)

추후에 개발하실 때는 지은님 라이브러리 만의 라이프사이클을 만들어서 객체의 추상화를 시키고, 고도화를 진행하면 좋을 것 같습니다!

답변에 맞게 좀 더 수정해서 답변을 드리면, addEventListener를 하나로 묶는 함수는 일반적이다, 라이프사이클 상에서 mount 등에서 많이 하는 방식이다 정도로 답변이 될 것 같습니다!

#### #20
[Input -> btn -> should 와 같은 패턴 체이닝으로 해결해보기](https://github.com/woowacourse/javascript-racingcar/pull/20#discussion_r574992460)
Input -> btn -> should 와 같은 패턴이 많이 보이네요~ 이런 것도 함수로 제공하면 좋을 것 같습니다 :)
아래와 같이 짤 수도 있겠네요!

```js
  class CypressWrapper {
    type = (name, value) => {
      try {
        this._getCy(name).type(value)
      } catch (err) {
        new Error(err)
      }

      return this
    }

    click = (name, params) {
      try {
        this._getCy(name).click(params)
      } catch (err) { 
        new Error(err)
      }
      
      return this
    }

    should(name, ...param) {
      try {
        this._getCy(name).should(...param)
      } catch(err) {
        new Error(err)
      }

      return this
    }

    _getCy(name) {
      return cy.get(name)
    }
  }

  const cw = new CypressWrapper()

  function testCar() {
    cw
      .type('#car-input', 'a,b,c,d')
      .click('#car-btn')
      .should('@count', 'have.css', 'display', 'block');
  }

  describe('ui-input-vaild-check', () => {
    ...
    it("입력한 시도 횟수가 소수면 alert 출력", () => {
      testCar()
      cw
        .type('#count-input', 4.21)
        .click('#count-btn')
        .should('@alertStub', 'be.calledWith', '시도 횟수는 소수가 될 수 없습니다.')
    })
  })
```


#### 후기

좋은 질문과 리뷰가 많아 리마인드 할 수 있는 점이 많았다.
반복되는 테스트 코드들을 chaning을 걸어 줄 수 있는 방법은 생각해 보지 못한 좋은 방식이라고 생각들었다.
코드를 안봐도 이해할 수 있는 네이밍, 네이밍에 맞는 동작을 설계해보자
의존적인 코드는 가까이, 의존성이 없는 코드는 분리 해보자
생각보다 간단한 원칙이지만 익숙하지 않아 비슷한 실수를 많이 작성한 것 같다.
디자인 패턴에 대해서도 리뷰어분들마다의 조금씩 의견이 다르고 호불호가 다른 것 같다.
항상 정답은 없지만 충분한 근거를 가지고 코드를 작성해 보아야 겠다.

생각보다 글이 많고 비슷한 맥락이 많아 모든 내용을 꼼꼼하게 보기 힘들었다. 전체의 모든 피드백을 다 보진 못하더라도 절반가량의 리뷰를 분석해보는 것은 나에게 큰 도움을 줄 수 있는것 같다.

