---
title: React 6 - React router
date: "2019-04-18T23:40:32.169Z"
template: "post"
draft: false
slug: "/posts/react-router/"
category: "React"
tags:
  - "react"
  - "react-router"
description: "Create React App(이하 CRA)에 특별히 routing을 위한 로직이 들어있지 않기 때문에, 가장 인기 있는 routing solution인 react-router를 추가해서 routing을 구현하도록 하겠습니다."
---

Create React App(이하 CRA)에 특별히 routing을 위한 로직이 들어있지 않기 때문에, 가장 인기 있는 routing solution인 [react-router](https://reacttraining.com/react-router/web/guides/quick-start)를 추가해서 routing을 구현하도록 하겠습니다.

### react-router 설치
```
npm install react-router-dom --save
```

### react-router 사용
CRA로 만든 앱에 route를 추가해서 사용 하려면 `src/app.js`에서 <App /> 대신에 routing을 설정한 컴포넌트로 대치해야 합니다.

```jsx
ReactDOM.render(<Routes />, document.getElementById('root'));
```

### Routes 컴포넌트 구현하기
```jsx
import React from 'react';
import {
  BrowserRouter as Router,
  Route,
  Switch,
} from 'react-router-dom';
import Home from './Pages/Home';
import Signup from './Pages/Signup';

class Routes extends React.Component {
  render() {
    return (
      <Router>
        <Switch>
          <Route exact path="/" component={Home} />
          <Route exact path="/signup" component={Signup} />
        </Switch>
      </Router>
    )
  }
}

export default Routes;
```

### Route 이동하기

Route 이동하는 방법은 두 가지가 있습니다.

#### Link 컴포넌트를 사용하는 방법
Routes에서 설정한 path로 이동하도록 구현하려면 `Link` 컴포넌트를 사용합니다.
```jsx
import React from 'react';
import { Link } from 'react-router-dom';

class Login extends React.Component {
  render() {
    return (
      <div>
        <Link to="/signup">회원가입</Link>
      </div>
    )
  }
}

export default Login;
```
그런데 react-router-dom에서 제공하는 Link 컴포넌트는 dom에서 `<a>` 로 변환되므로, 혹시 a태그를 사용하고 싶지 않다면 Link를 사용하지 않으셔도 됩니다. (예를 들어 이미 다른 태그로 버튼 컴포넌트를 만들어 놓았을 경우..)

#### withRouter HOC로 구현하는 방법
Link를 사용하지 않고, 요소에 onClick 이벤트를 달아서 이동하고 싶은 곳으로 넘기는 방법도 있습니다.
아래 goToSignup 라는 event handler에서 구현한 것처럼 this.props의 history에 접근해서 이동할 수 있습니다.

받은 history의 push 메서드에 이동할 path를 인자로 넘겨주면, 해당 라우트로 이동할 수 있습니다.
```jsx
import React from 'react';
import { withRouter } from 'react-router-dom';

class Login extends React.Component {

  goToSignup() {
    this.props.history.push('/signup');
  }

  render() {
    return (
      <div>
        <div
          class="btn signup-btn"
          onClick={this.goToSignup.bind(this)}
        >
          회원가입
        </div>
      </div>
    )
  }
}

export default withRouter(Login);
```
이 컴포넌트에서 props에 route 정보(history)를 받으려면 export하는 class에 `withRouter`로 감싸주어야 합니다.
이렇게 withRouter같이 해당 컴포넌트를 감싸주는 것을 [higher-order component(이하 HOC)](https://reactjs.org/docs/higher-order-components.html) 라고 합니다.

HOC는 react 고급 기능입니다. 기능이라기보다는 컴포넌트의 공통부분을 구현하는 패턴이라고 생각하시면 됩니다.
간단히 설명하면 HOC는 함수입니다.
그런데 컴포넌트를 인자로 받고, 컴포넌트를 return하는 함수입니다.

자세한 내용은 react에 충분히 익숙해지고, 프로젝트 2개 끝난 후 공부하는 것이 좋을 것 같습니다.
이번에는 react-router에서 제공하는 withRouter라는 HOC를 사용하고, 우리는 props에서 라우팅 정보만 편하게 받으면 됩니다.
혹시 충분히 아신다면 [컴포넌트 공통화 - HOC](https://yeri-kim.github.io/posts/react-hoc/)에서 공부해주세요.

console.log로 this.props.history 에 어떤 정보들이 더 오는지 한 번 확인 해보세요  :)
