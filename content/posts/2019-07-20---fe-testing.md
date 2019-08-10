---
title: "Frontend Testing"
date: "2019-07-20T13:42:32.169Z"
template: "post"
draft: false
slug: "/posts/fe-testing/"
category: "JavaScript"
description: "JavaScript Testing, 특히 react 테스팅에 대해 배워봅니다. - Unit Tests, Integration Tests, e2e Tests 가 있습니다."
tags:
  - "jest"
  - "enzyme"
  - "react"
  - "test"
---

[wecode](https://wecode.co.kr)에서 [unit test 수업](https://stackoverflow.com/c/wecode/questions/157)을 한 바 있지만, 백앤드(django)에 맞춰진 수업이었기에, 프론트앤드 testing을 위해 새로 작성하였다.
~~위의 수업 링크는 wecode 수강생만 볼 수 있음~~

----------

> 소프트웨어를 테스트하라. 그렇지 않으면 사용자가 테스트하게 될 것이다.<br>
-실용주의 프로그래머 Tip 49

테스트란 말그대로 프로그램이 잘 돌아가는지 테스트하는 것이다.
테스트는 개발자가 코드를 작성하면서도 할 수 있고, 개발이 어느정도 끝난 후, 사이트를 보며 일일이 눌러보며 테스트할 수도 있다.
이 중에서 제일 최악의 테스트는 위의 말처럼 사용자가 하는 테스트이다.

물론 모든 경우를 100% 대비하여 에러가 안나도록 개발을 할 수는 없지만, 웬만한 상황을 대비하려면 코드단계에서부터 테스트를 자동화 하는 것이 좋다.

나는 요 몇 년간 여러 서비스를 혼자 전담하여 개발하고, 처음부터 서비스가 종료될 때까지 참여했던 경우가 많아서
테스트의 중요성을 크게 느끼지 못하고 실무에 잘 적용하지 않았었다.
CTO님은 테스트를 엄청나게 중요하게 생각하는 분이었는데, 개발이 급한데 프론트앤드 테스트다 보니 테스트 코드에 허덕여하는 나를 보며 살짝 눈을 감아주셨던 것 같다.

![](/media/190723_1.PNG)
[위의 영상 보기](https://youtu.be/UttzAcbuk5k)

그 후에 잠시 새로운 회사에서 기존 서비스 유지보수 + 새로운 기능을 개발하면서 왜 테스트가 중요하고, TypeScript가 각광받는지 알게 되었다.
기존 소스코드를 조금만 수정해도 터져나오는 에러때문에 꽤나 고생을 했다. TypeScript 마저 세팅이 안 되어있었다면 멘붕이었을 것이다.

![](/media/190723_2.PNG)
[위의 영상 보기](https://youtu.be/UttzAcbuk5k)

참 공감가고 귀여운 그림이다ㅎㅎ. 레거시는 보통 다른 개발자에게 받은 코드인 경우가 많지만, 한 서비스를 오래 담당하는 개발자라면 몇 달전에 짠 코드를 100% 기억하고 있을리가 없다.
나를 믿기보다는, 테스트를 믿는 것이 더 낫다.

## TDD (Test-driven development, 테스트 주도 개발)
TDD는 소프트웨어 개발 프로세스이다. 영문 그대로 테스트가 '주'가 되는 개발 방법인것이다.
나처럼 개발먼저 하고, 테스트를 하나씩 붙여가는 것이 아니라 테스트 코드를 먼저 만들고 그 후에 로직을 짜는 것이다.

![](/media/190723-3.jpeg)
[위의 영상 보기~~(신입이라는데 어찌나 말을 잘하는지)~~](https://www.youtube.com/watch?v=1bTIMHsUeIk&t=248s)

테스트 코드에서 검증하고 싶은 로직이 제대로 짜여있지 않다면 fail이 떠서 red 상황이라고 생각하면 된다. 위의 캡쳐를 참고하자.
로직없이 테스트 코드를 먼저 짰으니 red에서 시작하는 것이 당연하다. 그 다음에 로직을 구현하고, refactoring을 하고, 테스트 코드를 수정하고, 로직을 발전시키고 하면 된다.

우측의 Test Last는 개발 먼저하고 나중에 테스트 코드를 붙이는 경우이다. 급한 개발 단계가 지나면 그때부터라도 테스트 코드를 짜면 된다.
코드 설계 단계부터 테스트 주도 개발을 하면 좋겠지만, 상황이 어쩔 수 없다면 늦게라도 붙이자.

#### 뭐 부터 테스트를 시작해야 할까?
테스트 할 로직은 너무 많고, 함수간의 의존성은 높고, 컴포넌트는 너무 방대해져버렸다면?
복잡하고, 제대로 설계하지 않은 코드일 수록 후에 테스트 코드 붙이기가 어려워진다.
괜찮다. 하나씩 리팩토링하고 나누면 되니깐!

일단,
- 자주 바뀌는 ui(dom검증) 부분은 테스트를 생략하자.
- 로직 위주로 테스트하자.
- input, output이 명확하고 짧은 utility 성 함수부터 하자.
- 의존성이 있는 함수라면 분리하자.


## 테스트 종류

![](/media/190723-4.png)
[위의 영상 보기](https://www.youtube.com/watch?v=r9HdJ8P6GQI)

테스트는 보통 Unit Test, Integration Test, E2E Test 이렇게 3가지로 나뉘다.

**Unit Test:** 단위테스트라고도 하는데, 검증할 코드가 독립적이어야 한다. 즉, 하나의 기능만 있는 함수 하나를 테스트하곤 한다.

**Integration Test:** 통합테스트라고 한다. 서로 의존하고 있고, 상호작용하는 함수의 호출이나 컴포넌트를 테스트하곤 한다.

**E2E (End to End) Test:** 전체 흐름을 테스트 하는 것이다. 사용자의 관점에서 실제 클릭하고, 클릭 후의 interaction 테스트하곤 한다.

React의 Unit Test, Integration Test를 위해 Jest 선택했고, E2E Test를 위해 Cypress를 사용하려고 한다.


## Jest

Create React App(이하 CRA)에서 이미 Jest로 테스트를 할 수 있도록 세팅이 잘 되어있다.
Jest로 DOM rendering test도 할 수 있지만,
Jest는 로직이나 component를 테스트하기 좋기에 위의 3가지 테스트 중에서 unit test를 위해서 사용한다.
CRA 문서에서 말하길, DOM testing을 하고 싶으면 e2e test 단계에서 하라고 권하고 있다.


#### 테스트 파일 생성
테스트를 돌릴 때, Jest는 테스트할 파일을 아래의 규칙으로 찾는다.

- ` __tests__` 폴더 아래에 있으면서 `.js` 확장자인 것.
- `.test.js` 확장자
- `.spec.js` 확장자

위의 파일이나 폴더는 src 폴더 밑에 아무데나 있어도 상관 없으며, 위의 규칙 중 아무거나 하나만 충족되어도 테스트 대상이 된다.

그러나 웬만하면 테스트 파일이나 폴더는, 테스트할 코드 파일 바로 옆에 두는 것이 좋다.
그래야 import할 때 경로가 짧아져서 좋고, 또한 테스트할 코드가 바로 옆에 있기 때문에 바로 원하는 파일을 찾을 수 있어서 유지보수하기 좋다.


#### 테스트 작성하기

아래의 코드는 CRA으로 앱을 생성할 때 기본으로 있는 `App.test.js` 파일이다. component를 mount하고,
다시 unmount시켜 component가 정상적으로 render 되는지 확인하는 간단한 smoke test이다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
```

테스트를 생성하려면 it() 을 추가하면 된다. 첫 번째 인자에는 test 이름을 넣고, 두 번째 인자인 콜백에는 테스트할 코드를 작성한다.
테스트를 돌리는 터미널에 테스트 코드는 보이지 않고, 테스트 이름과 통과 여부가 나오기 때문에 테스트 이름을 알아보기 쉽게 작성하는 것이 좋다.

참고로, 테스트를 하는 과정에서 react의 lifecycle 메서드가 잘 호출되었는지 확인하려고 console을 찍었는데, 아래와 같이 잘 나왔다.
<img src="/media/190720-1.png" width=300>

it() 하나에 테스트 코드 하나씩 작성하면 된다. 하나하나가 unit test인 것이다.
테스트 코드를 작성할 때, it() 들을 describe()로 다시 한 번 감싸서 비슷한 unit test끼리 grouping 지어서 테스트 설명(이름)을 작성할 수 있다.
좋다고 당연한 듯이 사용했었지만 CRA에서는 권하지 않고 있다. ~~그래놓고 본인들 docs에는 describe()쓴 예제 많음~~

아래의 코드는 [CRA docs](https://facebook.github.io/create-react-app/docs/running-tests)에 있는 예제이다.
```jsx
import sum from './sum';

it('sums numbers', () => {
  expect(sum(1, 2)).toEqual(3);
  expect(sum(2, 2)).toEqual(4);
});
```
assertion을 위한 expect() 함수는 따로 import할 필요 없이 Jest에서 global로 사용할 수 있게 제공한다.

unit test에서 말하는 assertion, assert란 내가 작성한 로직(함수)을 검증하기 위한 검증 구문이라고 생각하면 된다.
unit test를 위한 Assertion 라이브러리(Jest)에서 expect() 같은 [여러 검증 메서드(matchers)](https://jestjs.io/docs/en/expect)를 제공하고 있다.

#### 테스트하기
CRA인 경우, 아래의 명령어로 테스트를 시작하면 된다.
파일이 수정될 때마다 테스트가 재시작되므로 편하다.
```
npm test
```

## Enzyme
[Enzyme](https://airbnb.io/enzyme/)은 React Component를 테스트 하기 쉽도록 유용한 기능을 제공하는 JavaScript Testing utility이다.
Test Runner, Assertion Library와는 별도로 추가해서 사용하면 된다.

Jest가 이미 있는데, 왜 Enzyme을 추가로 써야하는지 모르겠다는 질문이 있는데, Enzyme의 많은 기능 중 하나는 [Shallow Rendering](https://facebook.github.io/create-react-app/docs/running-tests#option-1-shallow-rendering)이다.
Shallow Rendering은 자식 컴포넌트 존재에 상관 없이, 자기 자신의 component만 render하고 테스트를 할 수 있게 해준다.

Jest는 Test Runner이자 Assertion Library이다. 테스트가 돌게하고, 검증 구문으로 output을 체크한다면,
Enzyme은 React app에서 테스트를 더 쉽고 풍부하게 할 수 있도록 도와주는 역할을 한다.

#### Enzyme 설치
```
npm install --save-dev enzyme enzyme-adapter-react-16 react-test-renderer
```

Enzyme을 사용하려면 세팅이 필요하다.
```js
//src/setupTests.js
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

아래와 같이 component를 shallow render할 수 있다.
만약 App 컴포넌트 내에 Button 컴포넌트 같은 것을 사용하고 있다면, 자식 컴포넌트 까지 rendering하지 않는다.
오로지 App 컴포넌트의 작동만 보기에 Button에 에러가 나더라도 아래의 테스트는 통과한다.

Shallow rendering은 컴포넌트의 독립된 unit test를 하기에 딱 좋다.
만약 enzyme 없이 컴포넌트를 테스트하려면 자식의 자식의 자식 컴포넌트까지 모두 render하게 되어 제대로 unit test를 짤 수 없을 것이다.

```jsx
import React from 'react';
import { shallow } from 'enzyme';
import App from './App';

it('renders without crashing', () => {
  shallow(<App />);
});
```

wip...

#### reference
- https://www.youtube.com/watch?v=r9HdJ8P6GQI
- https://facebook.github.io/create-react-app/docs/running-tests
- https://airbnb.io/enzyme/
