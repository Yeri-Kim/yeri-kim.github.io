---
title: React Native - PanResponder 파헤치기
date: "2019-06-30T13:42:32.169Z"
template: "post"
draft: true
slug: "/posts/ts-interface-type/"
category: "React Native"
---

이 번에 공부한 내용은 터치와 제스처 관련한 PanResponder이다.
일반적인 앱을 구현할 때는 PanResponder 으로 터치 핸들링을 구현할 필요가 없다고 하지만,
웹에서도 워낙 애니메이션, interactive 컴포넌트 구현에 관심이 많았기에 좀 더 열심히 파보기로 했다.

소스코드는 [github](https://github.com/Yeri-Kim/learning-react-native/blob/master/ch4/TouchGesture.tsx)에 올려두었고,
이번에도 typescript로 구현하였다.


PanResponder gesture state 객체를 통해 현재 제스처의 속도, 누적 이동거리 등과 같은 원시 위치 데이터에 접근할 수 있다.
PanResponder를 사용하기 위해서는 PanResponder객체를 생성하고 컴포넌트의 랜더 함수에 결합해야 한다.
아래의 여섯가지 함수를 통해 터치 이벤트의 전체 라이프사이클에 접근할 수 있다.




#### reference
* https://facebook.github.io/react-native/docs/panresponder
* 빠른 모바일 앱 개발을 위한 React Native 2e