---
title: FCM으로 React Native Push Notification 구현하기
date: "2019-06-16T11:42:32.169Z"
template: "post"
draft: true
slug: "/posts/fcm-react-native/"
category: "React Native"
description: "React Native에서 FCM으로 Push Notification 을 구현하는 방법을 배워봅니다."
---

FCM은 firebase cloud messing의 줄임말로, FCM을 이용해서 push notification을 구현하는 방법을 알아보겠습니다.
FCM을 사용하지 않고 구현하는 법은 좀 더 알아보고 공유하도록 하겠습니다.
주변 RN이나 안드로이드 개발하는 분들에게 물어보니 전부 FCM을 사용한다고 하여 저도 다른 것은 알아보지 않고 바로 FCM을 사용하기로 하였습니다.

### fireabase project 추가
먼저 할 것은 [firbase에서 project를 추가](https://console.firebase.google.com/u/0/)하는 것입니다.
저는 siren-order 라는 이름으로 프로젝트를 하나 생성했습니다.

![](/media/190616-1.png)

그 동안 firebase 를 참 많이 사용했네요. FCM 만으로 firebase를 사용하기는 처음입니다.
이전에 ionic으로 hybrid app을 만드는 프로젝트를 했었는데 제가 push notification 담당이 아니라 옆에서 구경만 했었습니다.

그 때 푸쉬를 구현하거나 배포할 때 고생했던 기억이 있습니다. 앱개발자가 아니라는 이유로 인증체계 이해를 덜하고 프로젝트를 끝냈는데요,
이렇게 react native로 앱 개발을 시작할지 알았더라면 열심히 적어놓을걸 그랬습니다. 다시 차근히 처음부터 시작을..

### Apple developer 등록

expo 로 init한 RN 프로젝트 경우, android는 쉽게 세팅할 수 있는 것 같습니다.
저는 아이폰 사용자이므로 아이폰 구현을 먼저 하려고 합니다. 그런데 아직 ios는 지원하지 않는 것 같.. ㅡㅜ

> [Using FCM for Push Notifications](https://docs.expo.io/versions/latest/guides/using-fcm/)
> Firebase Cloud Messaging is required for all standalone Android apps made with Expo. To set up your Expo Android app to get push notifications using your own FCM credentials, follow this guide closely.
  **Note that FCM is not currently available for Expo iOS apps.**

firebase에서 ios 세팅을 하려고 하니 iOS bundle id도 필요하고, 여기에서 제공하는 plist 파일도 세팅해야하고.. (자세히 이따 보겠습니다)
이런 것들이 가능하려면 [apple developer 사이트](https://developer.apple.com/account/)에서 개발자와 앱, 둘 다 등록해야합니다.

####
