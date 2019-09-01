---
title: React - Context
date: "2019-08-29T02:46:32.169Z"
template: "post"
draft: false
slug: "/posts/react-context/"
category: "React"
description: "app 전체에서 데이터(state)를 관리하고, 어느 컴포넌트에서나 쓸 수 있도록 하는 Context를 배워봅니다."
tags:
  - "react"
  - "context"
---

저는 그 동안 React로 app을 만들 때, 전체 데이터를 관리하기 위해 주로 redux를 사용하였습니다.
redux를 설명할 때마다, 어떤 종류의 데이터를 redux로 관리 해야될지 모르겠다고 하던데,
저는 이제까지
user token, modal 상태, 언어 설정 등을 전역으로 관리한적이 있습니다.

redux가 나온지 꽤 되어서 그런지 아직까지 redux를 사용하는 회사가 많긴하지만, 요즘 채용공고를 보면 context를 사용한다는 곳도 점차 나타나기 시작했습니다.
저는 지난 JavaScript 컨퍼런스에서 Alice 회사 대표이신 유윤재님 발표를 듣고 굉장히 감명깊었는데!!
이번에 채용공고를 보니 역시나 React Native + Context + React Hooks 조합이 있더라고요.
조만간 React Hooks도 자세히 포스팅하도록 하겠습니다.

--------------

context가 무엇인지 물어보면 대부분 잘 알고 있습니다.
**컴포넌트간에 데이터를 넘겨주려면 props 뿐인데, 너무 먼 component에 데이터를 전달할 경우
context를 사용하면 props의 props의 props를 줄 필요 없이 데이터를 전달할 수가 있습니다.**
[공식문서](https://reactjs.org/docs/context.html)에 설명이 잘 되어있고,
심지어 [한글 버전](https://ko.reactjs.org/docs/context.html)도 잘 번역되어 있어서 이해하는데 어렵지는 않습니다만,
처음 접하는 사람들에겐 역시나 응용이 어려운 것 같습니다.
방법이 없습니다. 지금 개발하고 있는 곳에 억지로 사용해보며 익히는 수밖에..!

context에 대한 개념과 기초 예제는 [공식문서](https://reactjs.org/docs/context.html)를 읽어주세요.
제 포스팅은 응용에 대한 설명입니다. 공식문서에 나와있는 기초 예제를 모두 이해하고 넘어와주세요.

context 스터디를 했는데도 어려워하는 분들이 있는데, 간단히 설명하자면 context는 크게 Provider와 Consumer로 나뉩니다.<br/>
**Provider**는 `전역 데이터를 정의하고, 전역 데이터를 update하는 로직`이 있으며, <br/>
**Consumer**로 `전역 데이터를 사용`할 수 있습니다.

저는 modal 을 띄우고 닫는 예제를 구현해보았습니다.<br/>
필요한 전역 데이터는 modal을 띄웠는지 아닌지입니다.
modal을 띄웠다가 닫는 상태를 업데이트할 메서드도 추가하겠습니다. update 로직은 마치 redux의 action과 갖다고 보시면 됩니다.

~~(여기서 부터 갑자기 반말)~~

## context 생성
context 생성은 어렵지 않다. react의 createContext 함수를 사용하면 된다.
```jsx
const ModalContext = createContext();
```
원래 createContext() 인자로 초기 값을 세팅하라고 하는데,
막상 세팅하니 어차피 Provider를 정의하는 곳에서 덮어 씌워지기에 createContext 인자로 초기값을 세팅할 필요가 없어졌다.
Provider를 따로 정의하지 않을 때나 초기 값을 세팅하면 될 것 같다.

modal용 context를 생성하면 Provider도, Consumer도 해당 context를 사용해야하기 때문에 export를 해야한다.
어차피 Provider를 정의할 때 context를 생성하게 되므로 Provider를 정의하는 곳에서 context를 생성하려고 한다.



## Provider 정의

Provider에 대해 조금 더 자세히 설명하자면,
- Provider는 context를 구독(Consumer)하는 컴포넌트들에게 context의 변화를 알린다.
- Provider의 value라는 prop에 전역 데이터를 전달하면 된다.
- Provider 하위(내부)에 있는 컴포넌트 중에 context를 구독(Consumer)하는 부분이
Provider의 value가 바뀔 때마다 다시 렌더링된다.
- Provider 하위에 있다고 value가 업데이트 될 때마다 모든 컴포넌트가 다시 렌더링된다는 소리는 아니다.
context를 구독이 포함되어 있는 컴포넌트도 다시 렌더링 되는 것은 아니다.
오로지 context.Consumer 부분만 re render 된다. (무슨 말인지는 뒤에 소스코드에서 확인..)
- 당연히, Provider 컴포넌트 외부에 있는 컴포넌트에는 변화를 감지하지 못한다.

```jsx
// src/providers/ModalProvider.js

import React, { Component, createContext } from 'react';
import Modal from 'components/Modal';

// context 생성. 구독(Consumer)시 필요하니까 export 해주었다.
export const ModalContext = createContext();

class ModalProvider extends Component {

  // 모달이 보일지 말지 전역 데이터로 관리하려고 한다.
  // 모달이 show 이면, Modal 컴포넌트를 mount할 예정이다.
  state = {
    show: false
  };

  // show를 업데이트 할 메서드이다.
  showModal = () => {
    this.setState({
      show: true
    });
  };

  closeModal = () => {
    this.setState({
      show: false
    });
  };

  render() {
    const { show } = this.state;

    return (
      {/* 전역으로 show 상태와 show를 업데이트할 함수를 전달하려고 한다. */}
      <ModalContext.Provider
        value={{
          show,
          showModal: this.showModal,
          closeModal: this.closeModal
        }}
      >

        {/* ModalContext.Provider 내부에 있는 자식만이 ModalContext 변화를 감지할 수 있다. */}
        {this.props.children}

        {/* show === true, Modal 컴포넌트를 마운트할 예정이다. */}
        {show && <Modal />}

      </ModalContext.Provider>
    )
  }
}

export default ModalProvider;
```

#### ModalProvider 사용

```jsx
import React from 'react';
import ModalProvider from 'context/ModalProvider';
import Routes from 'Routes';

const App = () => (
  <ModalProvider>
    <Routes />
  </ModalProvider>
);

export default App;
```


팝업은 모든 페이지에서 사용할 수도 있으므로 modal Provider는 `<Routes/>`를 감쌌다.
ModalContext.Provider 내의 this.props.children에  `<Routes/>`가 위치하게 된다.

그러면 `<Routes/>`와 `<Modal/>`는 나란히 있는 sibling의 구조이다.
팝업은 `position: fixed`, 페이지와 상관없는 별개의 레이아웃이므로 페이지(`<Routes/>`)랑 나란히 두었다.

이 때의 문제점은, Modal에서  withRouter를 사용할 수 없다는 것이다.
Modal 컴포넌트가 Routes 컴포넌트 내부에 있어야만 사용할 수 있다.
만약 팝업 내의 이벤트 중에서 페이지 이동이 있어야 한다면,
react-router의 Route로 다시 감싸서 사용하거나, 팝업내의 로직을 사용측에서 전달하는 방법을 사용하면 된다.

---------

## Modal 컴포넌트와 Consumer

Modal 스타일과 layout이 정의된 `<Modal >` 컴포넌트이다.<br/>
ModalContext의 Consumer 이기도 하다.
왜냐하면 모달 내의 '확인', '취소', 'X' 버튼을 누르면 show를 false로 업데이트 해야되기 때문이다.
Consumer 내부는 함수여야 한다. 인자로 context를 받는다.

이 때 ModalProvider의 value에 넘겨준 closeModal()을 사용할 것이다.
false로 업데이트 되면 ModalProvider에서 `<Modal >` 컴포넌트가 unmount 된다.

Modal 컴포넌트를 만들다 보니 공통적인 레이아웃과 디자인만 정의되었지 내용물이 없다.
Modal의 내용은 호출하는 곳마다 다 다를 예정이니 props로 넘겨 받기로 하고 구현하였다.
이러면 ModalProvider 에서 `<Modal/>` 컴포넌트를 사용하는 부분에 content를 넘길 수 있도록 수정해야 한다.
```jsx
// src/components/Modal/index.js

import React from 'react';
import { ModalContext } from 'context/ModalProvider';
import './Modal.scss';

const Modal = ({ content }) => (
    <ModalContext.Consumer>
      {context => (
          <div className="modal">
            <div className="modal-content">{content}</div>
            <button
              className="modal-close"
              onClick={context.closeModal}
            >
              확인
            </button>
          </div>
      )}
    </ModalContext.Consumer>
)


export default Modal;
```

위의 방식으로 할지, 아래의 방식으로 할지 아직 뭐가 올바른 방식인지는 잘 모르겠다.
어차피 ModalProvider에서 show or not에 의해 Modal 컴포넌트가 unmount 될 예정이므로 굳이 button 부분에만 달아야하나 싶었다가도,
결국 context의 closeModal을 사용할 곳은 button이므로 button만 감싸면 되는지 고민해야했다.

#### 고민한 다른 방식의 Modal 컴포넌트
```jsx
// src/components/Modal/index.js

import React from 'react';
import { ModalContext } from 'context/ModalProvider';
import './Modal.scss';

const Modal = ({ content }) => (
  <div className="modal">
    <div className="modal-content">{content}</div>
    <ModalContext.Consumer>
     {context => (
        <button
          className="modal-close"
          onClick={context.closeModal}
        >
          확인
        </button>
     )}
    </ModalContext.Consumer>
  </div>
)


export default Modal;
```

----

## 다시 수정한 ModalProvider

위에서 설명했다시피 Modal 컴포넌트에 props로 Modal의 내용을 전달해야할 것 같아서 조금 수정하였다.
```jsx
// src/providers/ModalProvider.js

import React, { Component, createContext } from 'react';
import Modal from 'components/Modal';

export const ModalContext = createContext();

class ModalProvider extends Component {

  state = {
    show: false,
    modalContent: null
  };

  // showModal을 호출할 때, modal 내용을 인자로 전달하려고 한다.
  showModal = (modalContent) => {
    this.setState({
      show: true ,
      modalContent
    });
  };

  closeModal = () => {
    this.setState({
      show: false,
      modalContent: null
    });
  };

  render() {
    const { modalContent, show } = this.state;

    return (
      {/* value는 변화가 없다. 전역으로 관리할 필요없이, Popup 컴포넌트의 props로 넘겨줄 것이다. */}
      <ModalContext.Provider
        value={{
          show,
          showModal: this.showModal,
          closeModal: this.closeModal
        }}
      >
        {this.props.children}

        {/* Modal 컴포넌트에 content props를 추가하였다. */}
        {show && <Modal content={modalContent} />}
      </ModalContext.Provider>
    )
  }
}

export default ModalProvider;
```


--------------

## Modal을 띄워줄 컴포넌트 예시

드디어 모달을 호출할 컴포넌트이다!
SurveyPage 컴포넌트에서 "설문시작하기" 라는 버튼을 누르면 Modal을 띄울 것이다.

참고로 ModalContext가 바뀌면 `<ModalContext.Consumer>` 부분만 re-render 된다.
`<SurveyPage />` 전체가 re-render 되는 것은 아니다.
솔직히 `<ModalContext.Consumer>` 내부도 단지 showModal 메서드만 사용하므로 context가 변화됐다고 해서
re-render 되지 않아도 되는 부분이지만, 어쨋든 Consumer는 해당 context가 바뀌면 re-render 되므로 어쩔 수 없다.

```jsx
// src/Pages/Survey/index.js

import React, { Component } from 'react';
import { ModalContext } from 'context/ModalProvider';
import HobbyModal from './HobbyModal';
import './Modal.scss';

class SurveyPage extends Component {
    render() {
        return (
            <div>
                <p>안녕하세요! 설문에 참여하신 것을..</p>

                <div> 설문에 참여시.. </div>

                {/* 내용 생략 */}

                <ModalContext.Consumer>
                    {context => (
                        <button
                          className="survey-start-btn"
                          onClick={()=>context.showModal(<HobbyModal />}
                        >
                          설문시작하기
                        </button>
                    )}
                </ModalContext.Consumer>
            </div>
        )
    }
}

export default SurveyPage;
```


showModal()에 인자로 넘겨줄 모달 내용이 담긴 컴포넌트. 모달을 호출하는 곳마다 모달 내용은 다 다를 것이다.

```jsx
// src/Pages/Survey/HobbyModal.js
// 간단히 작성하였습니다.

import React from 'react';

const HobbyModal = () => (
    <div>취미를 선택해주세요.</div>
);

export default HobbyModal;
```




#### reference
- https://reactjs.org/docs/context.html
