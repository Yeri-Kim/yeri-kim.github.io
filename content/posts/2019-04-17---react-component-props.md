---
title: React 2 - Component와 Props
date: "2019-04-17T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/react-component-props/"
category: "React"
tags:
  - "react"
  - "props"
description: "프론트앤드 개발에서 Component 라는 단어를 많이 사용합니다. component(컴포넌트)란 재사용 가능한 UI 단위입니다."
---

_[React 공식문서](https://reactjs.org/docs/components-and-props.html)를 참고하여 번역하고 내용을 추가하였습니다._

## Component
프론트앤드 개발에서 Component 라는 단어를 많이 사용합니다. component(컴포넌트)란 재사용 가능한 UI 단위입니다.

pre course 프로젝트에서 로그인, 회원가입 페이지를 만들 때 input 태그가 여러개 필요하셨죠?
그래서 signup.html, login.html 에서 input 요소를 위해 똑같은 html코드를 ~~복사 붙여넣기 하고..~~작성하고,
input 디자인을 위해 signup.css, login.css에도 각각 같은 css를 작성했습니다.

만약 ‘내정보수정' 페이지가 추가되어 password input이 필요하다면, 또 똑같은 html, css를 작성해주어야합니다.
이럴 때 동일 코드가 반복되는 부분을 하나의 component로 만들어서 같은 디자인의 input이 필요한 곳마다 재사용할 수가 있습니다.

컴포넌트를 하나만 만들고 여기저기서 재사용하면, input 디자인이 바꼈을 때 css 한줄만 수정하면 로그인, 회원가입, 내정보수정 페이지에 바뀐 디자인이 모두 반영될 것입니다.

컴포넌트는 독립적으로, 재사용가능한 코드로 관리할 수 있습니다. 하나의 컴포넌트에 필요한 html, css, js(validation check)를 모두 합쳐서 만들 수 있습니다.

컴포넌트는 함수랑 비슷합니다. 함수도 기능이 독립적이고 재사용할 수가 있죠?
그리고 컴포넌트도 input을 받아서 return 할 수 있습니다.
React 컴포넌트에서는 input을 `props`라고 말하고 return은 화면에 보여져야할 `React요소`가 return됩니다.

## Component 만들기
React는 Component를 만들고 관리하기 정말 좋은 라이브러리 입니다. React에서는 컴포넌트를 class나 함수로 만들 수 있습니다.
어떤 때에는 함수로 만들면 좋고, 어떤 때에는 class로 만들어야만 합니다. 일단은 컴포넌트를 어떻게 만들 수 있는지 알아봅시다 :)


#### 함수로 Welcome 컴포넌트 구현하기
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
[여기에서](https://codepen.io/yeri-kim/pen/bJZZwm) 확인할 수 있습니다.

#### class로 Welcome 컴포넌트 구현하기
class로 컴포넌트를 만드려면 `React.Component` 를 extend해서 생성합니다. 컴포넌트를 생성할 때 `render()` 메서드는 무조건 정의해야하고, `return`도 해주어야 합니다.
`render()` 메서드는 무조건 정의해야한다는 말은, component를 만들 때 필요한 메서드가 원래 더 있다는 말입니다. 그런데 그 중에서 `render()` 만 필수입니다.
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## Component 사용

위처럼 정의한 컴포넌트는 함수/class 이름으로 사용할 수 있습니다.
태그처럼 `<Welcome />` 으로 작성합니다.

우리가 정의한 컴포넌트를 사용할 때, 원하는 attribute를 얼마든지 추가할 수 있습니다.
그러면 Welcome 컴포넌트(함수)에서 parameter로 해당 attribute를 받아서 사용할 수 있습니다. 이것을 `props`라고 말합니다. `props`는 property의 줄임말입니다.<br/>
.(dot)으로 속성명에 접근가능하고, props.속성명 으로 속성 값을 가져올 수 있죠.
```jsx
// 1. Welcome 컴포넌트 정의
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 2. App 컴포넌트 정의
function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

// 3. 화면에 React 요소 그려주기
ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
1. Welcome 컴포넌트: Welcome 컴포넌트를 사용한 측(부모)에서 name이라는 attribute를 부여했나봅니다. `props.name`의 값을 사용하네요.
2. App 컴포넌트를 보니 div로 감싸져있고,  `<Welcome />` 컴포넌트를 세번 사용했습니다. name이라는  attribute를 부여해주었네요.
3. `ReactDOM.render` 함수로 React 요소를 그려줍니다. root라는 id를 찾아 `<App />` 컴포넌트를 그려주네요.

[여기에서](https://codepen.io/yeri-kim/pen/vMPPNZ) 테스트하고 수정할 수 있습니다. 코드를 보고 이해만 하지 말고, 직접 수정하고 결과를 확인해보세요.



## 더 작은 Component로 분리하기

아래는 Comment라는 컴포넌트입니다.<br/>
컴포넌트는 재사용 가능한 코드 단위라고 했죠. Comment 컴포넌트 내에도 더 재사용할 수 있는 요소들이 보이네요.

.avatar 요소를 컴포넌트로 만들겠습니다. avatar 컴포넌트는 분명 댓글창 이외에도 사용자정보 등 여기저기 많이 쓰일 것 같습니다.
```jsx
function Comment(props) {
  return (
    <div className="comment">
      <div className="user-info">
        <img className="avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="user-info-name">
          {props.author.name}
        </div>
      </div>
      <div className="comment-text">
        {props.text}
      </div>
      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

```
.avatar 부분을 그대로 떼와서 Avatar라는 이름으로 컴포넌트를 만들어줍니다.
Comment 컴포넌트에서는 `props.author`로 접근해서 avatarUrl과 name을 가져왔는데, Avatar 컴포넌트에서는 좀 더 직관적으로 사용할 수 있도록 `user` 라는 이름으로 받아오겠습니다.

`props.user` 에서 avatarUrl, name 값을 가져오도록 하겠습니다.  `<Avatar />`를 사용하는 측에서 user라는 attribute를 추가해야겠네요.
```jsx
function Avatar(props) {
  return (
    <img className="avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
Avatar 컴포넌트에서 user의 avatarUrl과 name이 필요하므로, Comment 컴포넌트에서 `props.author` 정보를 `user`라는 attribute로 넘겨주었습니다.
`props.author`의 avatarUrl, name 값이 user를 통해 전달되었습니다.
```jsx
function Comment(props) {
  return (
    <div className="comment">
      <div className="user-info">
        <Avatar user={props.author} />
        <div className="user-info-name">
          {props.author.name}
        </div>
      </div>
      <div className="comment-text">
        {props.text}
      </div>
      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

한 번만 더 분리해보겠습니다! 이제는 .user-info 부분을 컴포넌트로 만들어볼까요. 재사용할 가능성이 1이라도 있다면 컴포넌트로 만들어주는 것이 좋습니다.
.user-info 부분을 그대로 떼어다가 UserInfo 라는 컴포넌트로 만들었습니다.
```jsx
function UserInfo(props) {
  return (
    <div className="user-info">
      <Avatar user={props.user} />
      <div className="user-info-name">
        {props.user.name}
      </div>
    </div>
  );
}
```
Comment 컴포넌트는 아래처럼 확 간결해졌습니다!!
```jsx
function Comment(props) {
  return (
    <div className="comment">
      <UserInfo user={props.author} />
      <div className="comment-text">
        {props.text}
      </div>
      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

코드는 [여기에서](https://codepen.io/yeri-kim/pen/YMggZr)확인할 수 있습니다. 한줄 한줄 이해하셔야 합니다. 이해하지 못하면 stack overflow에 물어봐주세요.


#### reference
* react docs
