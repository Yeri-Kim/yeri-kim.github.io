---
title: React 3 - Component의 State
date: "2019-04-17T22:41:32.169Z"
template: "post"
draft: false
slug: "/posts/react-state/"
category: "React"
tags:
  - "react"
description: "state란 말 그대로 컴포넌트의 상태 값입니다."
---

## state
state란 말 그대로 컴포넌트의 상태 값입니다. <br/>
state와 props는 둘 다 object 이고, 화면에 보여줄 정보(상태)를 가지고 있다는 점에서 서로 비슷한 역할을 합니다.
props는 컴포넌트를 사용하는 부모쪽에서 전달해야만 사용할 수 있고(parameter 처럼), state는 컴포넌트 내에서 정의하고 사용합니다.

아래의 코드는 버튼을 눌렀을 때, state를 변화시켜서 보여줘야할 텍스트를 바꾸는 코드입니다.
click이라는 하나의 state만 있지만, 여러 개의 state를 추가할 수 있습니다. 그리고 state의 이름은 원하는대로 지을 수 있습니다. <br/>
코드는 [여기에서](https://codepen.io/yeri-kim/pen/vMrVVE) 확인해주세요.
```jsx
class Button extends React.Component {

  constructor() {
    super();

    this.state = {
      clicked: false
    }
  }

  render() {
    return (
      <div
        className="btn"
        onClick={()=>{this.setState({ clicked: !this.state.clicked })}}
      >
        {this.state.clicked ? '좋아요' : '싫어요'}
      </div>
    );
  }
}

ReactDOM.render(
  <Button />,
  document.getElementById('root')
);

```
`render()` 함수부터 보겠습니다.
div 태그의 onClick 내부에 있는 `()=>{}`이 어색하면
[여기에서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 꼭 공부하고 넘어가주세요.
함수의 ES6 표현입니다. `arrow function` 이라고 부릅니다.

`onClick`는 event 시간에 배울 예정입니다. 지금은 `<div />`를 클릭하면 `onClick`에 넘긴 함수를 실행한다고만 알아주세요.
```jsx
render() {
  return (
    <div
      className="btn"
      onClick={()=>{this.setState({ clicked: !this.state.clicked })}}
    >
      {this.state.clicked ? '좋아요' : '싫어요'}
    </div>
  );
}
```
#### onClick={()=>{this.setState({ clicked: !this.state.clicked })}}
- click하면 clicked 이라는 state를 수정합니다. `this.setState()` 함수로 state를 업데이트할 수 있습니다.
- `!this.state.clicked` 으로 업데이트 한다는 말은, 현재 clicked 의 반대로(true면 false로, false면 true로) 저장한다는 말입니다.
- onClick이 달려있는 `<div />`를 클릭할 때마다, clicked 상태가 `true`나, `false`로 업데이트 됩니다.

#### {this.state.clicked ? '좋아요' : '싫어요'}
- clicked state가 true면 '좋아요' 를 보여주고 false면 '싫어요'를 보여줍니다.

그 다음은 `constructor()`를 보겠습니다. constructor는 class의 instance가 생성될 때 항상 호출되는 함수(생성자)입니다.
초기화할 값들을 constructor에서 세팅해준다고 보면 됩니다.
```jsx
constructor() {
  super();

  this.state = {
    clicked: false
  }
}
```
Button 컴포넌트를 그리려면 `this.state.clicked` 값이 필요한데, 제일 최초에는 값이 없으므로 constructor 에서 값을 지정해줍니다.
`super()` 라는 키워드는 꼭 작성해주셔야 합니다. 그래야 `React.Component` class에 있는 메서드들(ex. render)을 사용할 수 있습니다.

## props와 state
이번에는 앞에서 배웠던 props도 넣어보겠습니다.
`<Button />` 에 type을 추가했고, Button 컴포넌트에서 props로 받을 수 있습니다.

코드는 [여기에서](https://codepen.io/yeri-kim/pen/axKQMd) 확인할 수 있습니다.
```jsx
class Button extends React.Component {

  constructor() {
    super();

    this.state = {
      clicked: false
    }
  }

  render() {
    return (
      <div
        className={`btn ${this.props.type === 'like' ? 'like-btn' : ''}`}
        onClick={()=>{this.setState({ clicked: !this.state.clicked })}}
      >
        {this.state.clicked ? '좋아요' : '싫어요'}
      </div>
    );
  }
}

ReactDOM.render(
  <Button type="like" />,
  document.getElementById('root')
);

```
`this.props.type`이 'like'이면 `like-btn` 이라는 class 속성이 추가됩니다. 미리 `.like-btn` 에 배경색이 나오도록 css는 추가해 두었습니다.
코드를 잘 이해하고 넘어갑시다 :)
