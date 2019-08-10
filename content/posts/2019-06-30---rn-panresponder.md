---
title: "[React Native] 4장 모바일 컴포넌트"
date: "2019-06-30T13:42:32.169Z"
template: "post"
draft: false
slug: "/posts/rn-PanResponder/"
category: "React Native"
description: "4장 뒷 부분 PanResponder 파헤치기"
tags:
  - "react-native"
  - "panresponder"
---

이 번에 공부한 내용은 터치와 제스처 관련한 PanResponder이다.
일반적인 앱을 구현할 때는 PanResponder 으로 터치 핸들링을 구현할 필요가 없다고 하지만,
웹에서도 워낙 애니메이션, interactive 컴포넌트 구현에 관심이 많았기에 좀 더 열심히 파보기로 했다.

소스코드는 [github](https://github.com/Yeri-Kim/learning-react-native/blob/master/ch4/TouchGesture.tsx)에 올려두었고,
이번에도 typescript로 구현하였다.

### PanResponder
PanResponder는 react-native에서 제공하는 클래스이며, 특정 제스처에 해당하는 handler를 세팅할 수 있다.
handler에는 native event와 gestureState 객체를 인자로 받는다.

gestureState 객체를 통해 현재 제스처의 속도, 누적 이동거리 등과 같은 위치 데이터에 접근할 수 있다.
PanResponder를 사용하기 위해서는 PanResponder객체를 생성하고 컴포넌트의 랜더 함수에 결합해야 한다.
아래의 여섯가지 함수를 통해 터치 이벤트의 전체 라이프사이클에 접근할 수 있다.

각 이벤트의 handler는 우측과 같으며 event, gestureState 인자를 받는다.
```js
onPanResponderMove: (event, gestureState) => {}
```

나는 constructor에서 PanResponder을 생성해주었다.
```js
this._panResponder = PanResponder.create({
  //아래 두가지는 주어진 터치 이벤트에 반응할지를 결정한다.
  onStartShouldSetPanResponder: this._handleStartShouldSetPanResponder,
  onMoveShouldSetPanResponder: this._handleMoveShouldSetPanResponder,
  //터치이벤트가 발생할 떄 실행
  onPanResponderGrant: this._handlePanResponderGrant,
  //터치이벤트가 진행중일 때 실행
  onPanResponderMove: this._handlePanResponderMove,
  //아래 두개는 터치 이벤트가 끝날 때 실행
  onPanResponderRelease: this._handlePanResponderEnd,
  onPanResponderTerminate: this._handlePanResponderEnd
});
```
native event는 원래 native app touch 정보를 주는 것같고 [공식문서](https://facebook.github.io/react-native/docs/panresponder.html)에서 보는 것이 좋을 것 같다.
`gestureState` 객체에는 아래와 같은 정보가 들어온다.
- stateID - 제스처 id
- moveX - 마지막(최신,latest)에 이동한 요소의 좌표
- moveY - 마지막(최신,latest)에 이동한 요소의 좌표
- x0 - 이동직전의 좌표. 즉 터치이벤트가 발생한 순간의 좌표인 것 같다.
- y0 - 이동직전의 좌표. 즉 터치이벤트가 발생한 순간의 좌표인 것 같다.
- dx - 터치가 시작된 이후 제스처의 누적(움직인) 거리
- dy - 터치가 시작된 이후 제스처의 누적(움직인) 거리
- vx - 제스처의 현재 속도
- vy - 제스처의 현재 속도
- numberActiveTouches: 현재 화면에 표시된 터치 수, 세손가락을 올리면 3이다.

`moveX, moveY`에 대해 알아두어야 할 것이 있다면, 이 좌표는 요소의 한가운데점 또는, 좌측상단의 좌표가 아니라!
내가 터치하고 있는 그 점의 좌표이다.
나는 요소의 가운데 점의 좌표인줄 알았는데 요소가 같은 위치에 있더라도 내가 어느 영역을 터치하는지에 따라 좌표가 달랐다.

그리고, `x0 + dx = moveX`인듯 하다.

`vx, vy`는 정확한 단위는 모르겠지만, 우측/아래로 가면 +(vx/vy가 플러스)이다.

<img src="/media/190704.gif" width="230">

소스코드는 [github](https://github.com/Yeri-Kim/learning-react-native/blob/master/ch4/TouchGesture.tsx)에 올려두었다.
```jsx
import React from 'react';
import {
  StyleSheet,
  Text,
  View,
  PanResponder
} from 'react-native';

const CIRCLE_SIZE = 80;
const CIRCLE_COLOR = 'blue';
const CIRCLE_HIGHLIGHT_COLOR = 'green';

type StateType = {
  stateID?: string;
  numberActiveTouches: number;
  moveX: number;
  moveY: number;
  x0: number;
  y0: number;
  dx: number;
  dy: number;
  vx?: number;
  vy?: number;
}

class TouchGesture extends React.Component<{}, StateType> {

  _panResponder: any = {};
  _previousLeft = 0;
  _previousTop = 0;
  _circlesStyles = {
    style: {
      left: 100,
      top: 500
    }
  };
  circle = null;

  constructor(props) {
    super(props);

    this.state = {
      numberActiveTouches: 0,
      moveX: 0,
      moveY: 0,
      x0: 0,
      y0: 0,
      dx: 0,
      dy: 0
    };

    //PanResponder gesture state 객체를 통해 현재 제스처의 속도, 누적 이동거리 등과 같은 원시 위치 데이터에 접근할 수 있다.
    //PanResponder를 사용하기 위해서는 PanResponder객체를 생성하고 컴포넌트의 랜더 함수에 결합해야 한다.
    //아래의 여섯가지 함수를 통해 터치 이벤트의 전체 라이프사이클에 접근할 수 있다.
    this._panResponder = PanResponder.create({
      //아래 두가지는 주어진 터체 이벤트에 반응할지를 결정한다.
      onStartShouldSetPanResponder: this._handleStartShouldSetPanResponder,
      onMoveShouldSetPanResponder: this._handleMoveShouldSetPanResponder,
      //터치이벤트가 발생할 떄 실행
      onPanResponderGrant: this._handlePanResponderGrant,
      //터치이벤트가 진행중일 때 실행
      onPanResponderMove: this._handlePanResponderMove,
      //아래 두개는 터치 이벤트가 끝날 때 실행
      onPanResponderRelease: this._handlePanResponderEnd,
      onPanResponderTerminate: this._handlePanResponderEnd
    });
  }

  componentDidMount () {
    this._updatePosition();
  }

  render() {
    console.log('render');

    return (
      <View style={styles.container}>
        <View
          ref={circle => {
            this.circle = circle;
          }}
          style={styles.circle}
          {...this._panResponder.panHandlers}
        />
        <Text>
          {this.state.numberActiveTouches} touches
        </Text>
        <Text>
          toucheId: {this.state.stateID},
        </Text>
        <Text>
          moveX: {this.state.moveX},
        </Text>
        <Text>
          moveY: {this.state.moveY},
        </Text>
        <Text>
          dx: {this.state.dx},
        </Text>
        <Text>
          dy: {this.state.dy},
        </Text>
        <Text>
          x0: {this.state.x0},
        </Text>
        <Text>
          y0: {this.state.y0},
        </Text>
        <Text>
          vx: {this.state.vx},
        </Text>
        <Text>
          vy: {this.state.vy},
        </Text>
      </View>
    );
  }

  _highlight = () => {
    console.log('_highlight');

    this.circle && (
      this.circle.setNativeProps({
        style: { backgroundColor: CIRCLE_HIGHLIGHT_COLOR }
      })
    )
  };

  _unHighlight = () => {
    console.log('_unHighlight');

    this.circle && (
      this.circle.setNativeProps({
        style: { backgroundColor: CIRCLE_COLOR }
      })
    )
  };

  _updatePosition = () => {
    console.log('=====_updatePosition=====');
    console.log(this.circle);

    this.circle && this.circle.setNativeProps(this._circlesStyles);
  };

  _handleStartShouldSetPanResponder = (event, gestureState) => {
    console.log('_handleStartShouldSetPanResponder');

    // 사용자가 원 부분을 누르기 시작할 때 responder 활성화 여부
    return true;
  };

  _handleMoveShouldSetPanResponder = (event, gestureState) => {
    console.log('_handleMoveShouldSetPanResponder');

    // 사용자가 원 위로 터치하여 움직일 때 responder를 활성화할까요?
    return true;
  };

  _handlePanResponderGrant = (event, gestureState) => {
    console.log('_handlePanResponderGrant');

    //터치이벤트가 발생할 떄 실행
    this._highlight();
  };

  _handlePanResponderMove = (event, gestureState) => {
    console.log('_handlePanResponderMove');

    //터치이벤트가 진행중일 때 실행
    //여기서 screen은 터치하고자 하는 요소(circle)을 의미한다
    this.setState({
      //gestureState id
      stateID: gestureState.stateID,
      //마지막(최신,latest) screen 최표
      //이 좌표는 요소의 가운데 좌표가 아니라, 내가 터치하고 있는 좌표이다.
      moveX: gestureState.moveX,
      moveY: gestureState.moveY,
      //responder grant의 screen 좌표(아마 움직인 최초 거리를 의미하는듯)
      //x0+dx = movxX인듯 하다.
      x0: gestureState.x0,
      y0: gestureState.y0,
      //터치가 시작되고 나서부터의 축적된(움직인) 거리
      dx: gestureState.dx,
      dy: gestureState.dy,
      //현재 움직임의 속도
      //정확한 단위는 모르겠지만, 우측/아래로 가면 +(vx/vy가 플러스) 이다,
      vx: gestureState.vx,
      vy: gestureState.vy,
      //screen에 발행한 터치 수, 세손가락이면 3
      numberActiveTouches: gestureState.numberActiveTouches
    });

    //이동할 값만큼 현재 위치 계산
    this._circlesStyles.style.left = this._previousLeft + gestureState.dx;
    this._circlesStyles.style.top = this._previousTop + gestureState.dy;
    this._updatePosition();
  };

  _handlePanResponderEnd = (event, gestureState) => {
    console.log('_handlePanResponderEnd');

    this._unHighlight();
    this._previousLeft += gestureState.dx;
    this._previousTop += gestureState.dy;
  };
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: 64
  },
  circle: {
    width: CIRCLE_SIZE,
    height: CIRCLE_SIZE,
    borderRadius: CIRCLE_SIZE/2,
    backgroundColor: CIRCLE_COLOR,
    position: 'absolute'
  }
});

export default TouchGesture;
```

#### reference
* https://facebook.github.io/react-native/docs/panresponder
* 빠른 모바일 앱 개발을 위한 React Native 2e
