---
title: React - Hooks
date: "2019-09-19T02:46:32.169Z"
template: "post"
draft: false
slug: "/posts/react-hooks/"
category: "React"
description: "[실전 리액트 프로그래밍 5장 - 진화된 함수형 컴포넌트: 리액트 훅] 정리"
tags:
  - "react"
  - "hooks"
---

## 5.1 리액트 훅 기초 익히기
훅은 함수형 컴포넌트에서도 클래스형 컴포넌트의 기능을 사용할 수 있게 하는 기능이다.
새로 작성하는 컴포넌트는 되도록 클래스형 컴포넌트보다는 훅을 사용해서 함수형 컴포넌트로 작성하자.

### 5.1.2 함수형 컴포넌트에 상탯값 추가하기: useState
컴포넌트의 state를 바꾸는 훅

예제코드
```jsx
const InputElement = () => {
  const [inputText, setInputText] = useState('');
  const [historyList, setHistoryList] = useState([]);

  return (
	<input
		onChange={e=> {
			setInputText(e.target.value);
			setHistoryList([…historyList, e.target.value]);
		}}
	/>
	{inputText}

	<ul>
		{historyList.map(rec=><div>{rec}</div>}
	</ul>
   )
}
```

1. userState는 배열을 return 하는데 첫 번째 요소(`inputText`)는 state 값이며,
useState 함수의 인자에는 해당 state의 초깃값을 넣어주면 된다.

2. return된 배열의 두번 째 요소(`setInputText`)는 state를 변경할 수 있는 함수이다.

onChange의 속성값이 rendering 될 때마다 함수가 생성되므로 성능이 걱정될 수 있는데,
이 문제를 해결하기 위해 useCallback 훅을 제공한다고 한다.

#### useState 훅 하나로 여러 상탯값 관리하기
위 처럼 useState를 여러번 호출해도 되지만, 아래와 같이 하나로 여러 상탯값을 관리할 수도 있다.

예제코드
```jsx
const InputElement = () => {
  const [state, setState] = useState({ inputText: '', historyList: [] });

  return (
	<input
		onChange={e=> {
			setState({
			    inputText: e.target.value,
			    historyList: [...state.historyList, e.target.value]
            });
		}}
	/>
	{inputText}

	<ul>
		{historyList.map(rec=><div>{rec}</div>}
	</ul>
   )
}
```
이렇게 state를 하나의 객체로 관리하는 경우를 위해 useReducer 훅이 제공된다고 한다.


### 5.1.3 함수형 컴포넌트에서 생명 주기 함수 이용하기: useEffect
useEffect 훅을 통해서 함수형 컴포넌트에서도 생명 주기 함수를 이용할 수 있다.
useEffect 인자로 넘긴 함수는 렌더링할 때마다 호출된다.

```jsx
const Profile = ({ userId }) => {
    const [user, setUser] = useState(null);
    userEffect(() => {
        getUserApi(userId).then(data => setUser(data));
    }, [userId]);

    return (
        {user ? (
            <>
                <p>{user.name}</p>
                <p>{user.age}</p>
            </>
        ) : (
            <p>사용자 정보 가져오는 중..</p>
        )}
    )
}
```
위의 코드에서 (렌더 한 후) 최초에 한 번, 그리고 userId가 바뀔 때마다 한 번 호출된다.

#### componentDidMount, componentWillUnMount의 역할을 한 로직에서
useEffect의 장점은 비슷한 기능을 한 곳으로 모을 수 있어 가독성이 좋아진다는 점이다.
기존에 componentDidMount에 이벤트를 등록하고, componentWillUnMount에서 해제하여
라이프사이클 때문에 로직이 분산된 경우가 많았다. 아래의 예제를 보자.

```jsx
const WidthPrinter = () => {
    const [width, setWidth] = useState(window.innerWidth);
    useEffect(() => {
        const onResize = () => setWidth(window.innerWidth);
        window.addEventListener('resize', onResize);

        return () => { window.removeEventListener('resize', onResize); }
    }, []);

    return <div>width: {width}</div>;
}
```
1. useEffect의 인자로 넘긴 함수는 또 다른 함수를 return할 수 있다. 이 반환된 함수는 컴포넌트가 unmount 되거나, 첫 번째 매개변수로 입력된 함수가 호출되기 직전에 호출된다.
2. useEffect 의 두 번째 인자에 빈배열을 넣으면 컴포넌트가 마운트 될 때만 인자로 넘긴 함수가 호출되고, 컴포넌트가 언마운트 될 때 반환된 함수가 호출된다. 마치 클래스형 컴포넌트의 componentDidMount와 componentWillUnmount 메서드에서만 실행되는 것과 같은 효과가 있다.

### 5.1.4 훅 직접 만들기
리액트가 제공하는 훅을 이용해서 custom 훅을 만들 수 있다. 훅을 직접 만들어서 사용하면 HOC와 render 속성값 패턴처럼 로직을 재사용할 수 있다.
리액트의 내장 훅처럼 use로 시작하는게 좋다.

gatsby blog theme을 사용할 때 hook을 건드릴일이 있었는데, 소스코드를 보면 좋을 것 같다.

#### useWindowWidth 커스텀 훅
innerWidth를 가져오는 로직은 꽤 많이 쓰인다. 아래 코드를 잘 재사용하면 좋을 것 같다.

```jsx
const useWindowWidth = () => {
    const [width, setWidth] = useState(window.innerWidth);
    useEffect(() => {
        const onResize = () => setWidth(window.innerWidth);
        window.addEventListener('resize', onResize);
        return () => { window.removeEventListener('resize', onResize); }
    }, []);

    return width;
}
```
훅이라고 해서 꼭 배열로 리턴해야한다는 법은 없다.
위는 innerWidth가 바뀔 때마다 가로값을 반환해주는 훅이다.

## 5.2 리액트 내장 훅 살펴보기

### 5.2.1 Consumer 컴포넌트 없이 콘텍스트 사용하기: useContext
바로 이전에 context api를 다뤘다.
먼저 보고 오는 것이 좋을 것 같다.

```jsx
const UserContext = React.createContext();

//context 관련 코드 생략..

const ChildComponent = () => {
    const user = useContext(UserContext);
    console.log(user);
}
```


### 5.2.2 함수형 컴포넌트에서 돔 요소 접근하기: useRef
#### DOM 바로 접근할 때 사용. 클래스형 컴포넌트에서 createRef 함수를 사용했었다.

예제 1.
```jsx
const ImageToggerOnMouseOver = ({ primaryImg, secondaryImg }) => {
	const imageRef = userRef(null);

	return (
		<img
			onMouseOver={() => {
				imageRef.current.scr =  secondaryImg;
			}}
			onMouseOut={() => {
				imageRef.current.scr = primaryImg;
			}}
			src={primaryImg}
			ref={imageRef}
		/>
	)
}
```

예제 2.
```jsx
const FocusTest = () => {
    const inputEl; = useRef(null);
    const handleClick = () => {
        if (inputEl.current) {
            inputEl.current.focus();
        }
    }

    return (
        <div>
            <input ref={inputEl} type="text" />
            <button onClick={handleClick}>focus</button>
        </div>
    )
}
```

#### 클래스형의 멤버변수처럼 렌더링과 무관한 고유값 저장
함수형 컴포넌트는 인스턴스로 생성되지 않기 때문에 사용된 컴포넌트의 고유한 값을 저장할 방법이 없었다.
다음은 useRef 훅은 이용해서 이전 상탯값을 저장하는 코드

```jsx
import React, { useState, useRef, useEffect } from 'react;

const Profile = () => {
    const [age, setAge] = useState(20);

    // age 이전 상탯값을 저장하기 위한 용도
    const prevAgeRef = useRef(20);

    // age 값이 변경되면 prevAgeRef에 저장
    useEffect(() => {
        prevAgeRef.current = age;
    }, [age]);

    // 이전 age 값
    const prevAge = prevAgeRef.current;

    // 코드 일부 생략

    return (
        <div>
            <button
                onClick={() => {
                    const age = Math.floor(Math.random() * 50 + 1);
                    // age가 변경돼서 다시 렌더링할 때 useEffect 불림
                    setAge(age);
                }}
            >
                나이 변경
            </button>
        </div>
    )
}

```
이 외에도 setTimeout, setInterval 등의 함수가 반환하는 id 값처럼 클래스의 멤버 변수로 저장하던 값을 useRef 훅으로 저장할 수 있다.

### 5.2.3 메모이제이션 훅: useMemo, useCallback
useMemo, useCallback 은 이전 값을 기억해서 성능을 최적화하는 용도로 사용된다.

#### useMemo
useMemo는 계산량이 많은 함수의 반환값을 재활용하는 용도로 사용된다.
로직이 엄청 길고 복잡하다면, 로직을 분리하여 반환하게 하고 그 값을 기억하도록 한다.
복잡한 로직함수의 인자가 같다면, 기억한 값을 사용할 수 있도록 하는 훅이다.

```jsx
import React, { useMemo } from 'react';
import { runExpensiveJob } from './util';

const MyComponent = ({ v1, v2 }) => {
    const value = useMemo(() => runExpensiveJob(v1, v2), [v1, v2]);
    return <p>{value}</p>;
}

```
useMemo 훅은 이 함수가 반환환 값을 기억한다. useMemo의 두 번째 매개변수로 입력된 배열의 값이 변경되지 않으면 이전에 반환된 값을 재사용 한다.

#### useCallback
리액트의 렌더링 성능을 위해 제공된다.
자식 컴포넌트에 넘기는 props에 함수 정의를 작성해놓으면,
부모 컴포넌트가 render할 때마다 매번 새로운 함수가 생성되므로 자식 컴포넌트도 rerender 하게 된다.
이럴 때 useCallback을 사용한다고 한다.

```jsx
const Profile = () => {
    const [name, setName] = useState('');
    const [age, setAge] = useState(0);
    const onSave = useCallback(() => saveToserver(name, age), [name, age]);

    return (
        <div>
            <p>{name}</p>
            <p>{age}</p>
            <UserEdit onSave={onSave} setName={setName} setAge={setAge} />
        </div>
    )
}
```
onSave의 props의 값으로 `() => saveToserver(name, age)`를 직접입력하지 말고 위와 같이 작성한다.
useMemo와 비슷하다. 인자가 변경되지 않으면 UserEdit 컴포넌트의 onSave 속성 값으로 항상 같은 함수가 전달되어 rerender를 막을 수 있다.

### 5.2.4 컴포넌트의 상탯값을 리덕스처럼 관리하기: useReducer
p235

## 5.3 클래스형 컴포넌트와 훅
클래스형 라이프사이클 메서드 대부분을 훅으로 구현할 수 있는 예제가 있다. 재밌다.

- componentDidMount, componentWillUnmount 메서드는 useEffect, useLayoutEffect 훅으로 대체할 수 있다.
- 클래스의 멤버 변수는 useRef 훅으로

### 5.3.1 constructor 메서드

```jsx
const Profile = ({ firstName, lastName }) => {
    // props로부터 받은 데이터 초기 state로 세팅
    const [name, setName] = useState(`${firstName} ${lastName}`);

    // 컴포넌트 최초 호출 시에만 callApi 함수를 호출하게 됨
    const isFirstRef = useRef(true);
    if (isFirstRef.current) {
        isFirstRef.current = false;
        callApi();
    }
}
```

이런 로직이 자주 사용된다면, 아래와 같이 커스텀 훅을 만들어서 사용할 수 있다.
```jsx
// constructor 역할의 커스텀 훅
// useOnFirstRender.js
const useOnFirstRender(func) {
    const isFirstRef = useRef(true);

    if (isFirstRef.current) {
        isFirstRef.current = false;
        func();
    }
}


// 사용측
// Profile.js
const Profile = ({ firstName, lastName }) => {
    const [name, setName] = useState(`${firstName} ${lastName}`);
    useOnFirstRender(callApi);
}
```

### 5.3.2 componentDidUpdate 메서드
componentDidUpdate는 원래 최초 rendering 후, state나 props가 바뀌었을 때만 호출한다.
하지만 useEffect 훅은 최초 rendering 후에도 호출되므로 componentDidUpdate 기능을 구현하기 위해
useEffect 말고, useRef 훅을 사용하면 된다.

componentDidUpdate 에서는 인자로 이전 상태값, 이전 속성값을 받았지만,
함수형 컴포넌트는 인스턴스가 없기 때문에 이전 값이 필요하다면
멤버변수처럼 관리하게 해줄 수 있는 useRef 훅으로 직접 관리해야 한다.

```jsx
// usePrevious 커스텀 훅
const usePrevious = value => {

    // 이전 값을 기억하기 위해
    const valueRef = useRef();

    // 렌더링 후에는 현재 값을 이전 값으로 받는다.
    useEffect(() => {
        valueRef.current = value;
    }, [value]);

    // 이전 값 반환
    return valueRef.current;
}



// 사용 예
const Profile = props => {
    const [name, setName] = useState(props.name);
    const prevUserId = usePrevious(props.userId);
    const isMountedRef = useRef(false);

    useEffect(() => {
        if (isMountedRef.current) {
            // 이전 props, 지금 props 비교해서 다를 때만 state 변경
            if (prevUserId !== props.userId) {
                setName(props.name);
            }
        } else {
            isMountedRef.current = true;
        }
    });
}

```


위와 같은 로직이 반복되면 커스텀 훅으로 만들 어 쓸 수 있다!
```jsx
const useOnUpdate = func => {
    const isMountedRef = useRef(false);
    useEffect(() => {
        if (isMountedRef.current) {
            func();
        } else {
            isMountedRef.current = true;
        }
    });
}
```

### 5.3.3 getDerivedStateFromProps
여기까지 블로그 작성하다가 드는 생각은, 이 커스텀 훅을 저자가 제공한 것인지, tutorial에서 제공한것인지 ㅎㅎ
재밌다.

```jsx
const SpeedIndicator = ({ speed }) => {
    const [isFaster, setIsFaster] = useState(false);
    const [prevSpeed, setPrevSpeed] = useState(0);

    // props가 바뀌었을 때만 state를 변경한다.
    if (speed !== prevSpeed) {
        setIsFaster(speed > prevSpeed);
        setPrevSpeed(speed);
    }

    return <p>getting faster: {isFaster ? 'y' : 'n'}</p>
}
```

리액트는 렌더함수에서 상탯값을 변경하면 변경된 상탯값으로 렌더함수를 다시 호출한다.
그래서 getDerivedStateFromProps 보다 비효율적이라고도 한다.

주의할 점은, 잘못하면 렌더 함수가 무한대로 호출될 수 있다는 점이다. 위에서 prevSpeed를 usePrevious 커스텀 훅으로
관리했다면 렌더 함수가 무한대로 호출될 수 있다.

### 5.3.4 forceUpdate
이 메서드는 지양해야 하므로 정리하지 않겠다.. ㅋㅋ

---------

#### 훅의 장점
- 재사용 가능한 로직을 쉽게 만들 수 있다: 리액트의 내장 훅과 다른 사람들이 만든 여러 커스텀 훅을 레고처럼 조립해서 쉽게 새로운 훅을 만들 수 있다.
- 훅을 사용하면 같은 로직을 한곳으로 모을 수 있어서 가독성이 좋다.


### 훅 사용 시 지켜야 할 규칙
- 하나의 컴포넌트에서 훅을 호출하는 순서는 항상 같아야 한다.
- 훅은 함수형 컴포넌트 또는 커스텀 훅 안에서만 호출되어야 한다.
    - 우리가 useState 훅에 전달한 정보는 상탯값의 기본값밖에 없다. 리액트가 age와 name 상탯값을 구분할 수 있는 유일한 정보는 훅이 사용된 순서다.


#### reference
- [실전 리액트 프로그래밍](http://www.yes24.com/Product/Goods/74223605?Acode=101) : 강추
