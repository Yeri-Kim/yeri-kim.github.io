---
title: React 5 - 이벤트 핸들링
date: "2019-04-18T22:46:32.169Z"
template: "post"
draft: false
slug: "/posts/react-event/"
category: "React"
tags:
  - "react"
  - "event"
---

회원가입 페이지에서 비밀번호를 제대로 입력했는지 `addEventListener` 를 사용해서 key 이벤트로 validation check 로직을 구현해봤었습니다.
DOM을 찾아 addEventListener으로 event handler를 추가했었죠.

React에서는 React element에 `onClick` 이벤트를 추가해서 event handler 함수를 넘겨주겠습니다.
사실 React 라이브러리를 사용하지 않아도 같은 방식으로 event handler를 추가하는 방법이 있지만, 지난 번에는 실습하지 않았습니다.
일반 DOM element에는 어떻게 하는지 [여기를](https://www.w3schools.com/jsref/event_onclick.asp) 참고해주세요.

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
지난 [state 포스팅](https://yeri-kim.github.io/react/2019/04/17/react-state.html)에 있던 코드를 그대로 가져왔습니다.
Button 컴포넌트의 `<div>` 태그에 `onClick`이벤트에 `()=>{this.setState({ clicked: !this.state.clicked })}` 라고 event handler를 넘겨주었습니다.

해당 요소를 click 할 때마다 event handler 함수가 실행됩니다. 즉, 클릭할 때마다 `this.state.clicked` 값이 반대의 boolean 값으로 clicked state를 업데이트 합니다


그런데 render 메서드 내부의 React 요소에 함수가 포함되어서 그런지 코드가 깔끔하지 않습니다.
event handler 부분을 분리해서 함수를 밖으로 빼겠습니다.

```jsx
class Button extends React.Component {

  constructor() {
    super();

    this.state = {
      clicked: false
    }

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState({
      clicked: !this.state.clicked
    });
  }

  render() {
    return (
      <div
        className={`btn ${this.props.type === 'like' ? 'like-btn' : ''}`}
        onClick={this.handleClick}
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
Button 컴포넌트에 handleClick 메서드를 추가했고,
`onClick` 이벤트에 `this.handleClick` event handler 함수를 넘겨주었습니다.

constructor에 추가된 코드를 주목해주세요. (this, bind 등에 대한 자세한 설명은 다음 offline 세션에서 진행하도록 하겠습니다.)
```js
this.handleClick = this.handleClick.bind(this);
```
`this.handleClick`가 `this.handleClick.bind(this)` 라는 뜻입니다.
원래는 아래의 코드처럼 써야하는데 표현이 길어지니까 constructor에서 this.handleClick에 이미 this를 bind시킨  handleClick 메서드를 대입해주었습니다.
```jsx
 render() {
    return (
      <div
        className={`btn ${this.props.type === 'like' ? 'like-btn' : ''}`}
        onClick={this.handleClick.bind(this)}
      >
        {this.state.clicked ? '좋아요' : '싫어요'}
      </div>
    );
  }
```
handleClick 메서드 내에서 this 키워드를 사용하고 있는데, 이 this는 Button 컴포넌트의 context여야 합니다.
handleClick.bind(this)라고 작성해준 위치에서 그 this를 handleClick 에 넘겨서 handleClick 메서드 내에서도 같은 this를 쓰겠다는 소리입니다......? ㅎㅎ

그래야만 Button 컴포넌트의 this.state에 접근하고, setState 함수도 쓸 수 있기 때문이죠.

이벤트에 event handler 함수를 넘길때 bind를 해주지 않으면
event handler 함수내에서 this의 context를 잃어버려서 this가 `undefined`가 됩니다.

offline 심화 내용에서 자세하게 다룰 예정입니다. React 요소에 이벤트를 추가할 경우 bind(this)를 해줘야한다는 것만 알고 계세요!
