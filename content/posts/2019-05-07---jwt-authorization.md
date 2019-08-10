---
title: jwt으로 로그인 구현하기
date: "2019-05-07"
template: "post"
draft: false
slug: "/posts/jwt-authorization/"
category: "JavaScript"
description: "로그인 기능을 구현하기 위해 access token을 Local Storage에 저장하고 request header에 보내는 방법을 알아봅니다."
tags:
  - "jwt"
---

이번 시간에는 access token을 local storage에 저장하고, api 호출 시 request header에 담아 보내는 법을 배우도록 하겠습니다.

지금 로그인 api에서는 로그인이 성공하면 jwt로 만든 access token을 받고 있습니다.
프론트앤드에서는 이 access token을 잘 갖고 있다가, 사용자 정보가 필요한 api를 호출할 때
해당 access token을 보내주어야 합니다.

이미 jwt에 대한 수업을 들었기에 위의 문장이 무슨 말인지 다 이해하셨길 바라지만.. 한 번 더 프로세스를 보겠습니다.

jwt는 `Json Web Token`의 약자로 access token을 만드는 방법 중 하나입니다.
백앤드에서 jwt를 생성할 때, 해당 user가 누구인지 식별할만한 정보를 담을 수 있습니다.

예를 들어 백앤드에서 받은 아래의 access token을 decode해보니 `user_id: 5`라는 정보를 담고 있었다는 것을 알 수 있습니다.
([https://jwt.io/](https://jwt.io/))
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo1fQ.Dy3PPHIOFFe3ScGTb8x3hewItnpOcgC9YDdW7v27XHg
```
<img src="/media/190507-jwt.png" width="500" />

페이지를 이동할 때 마다, 아니면 브라우저를 끄고 켤때마다 매번 로그인을 할 수 없기 때문에 이렇게 access token을 주고 받아서 사용자 인증을 진행합니다.
로그인을 하면 access token을 받아서, 프론트앤드는 어딘가에 보관하고 있다가 사용자 정보가 필요한 api에 해당 access token을 백앤드에 보내줍니다.

예를 들어, 인스타 좋아요한 사진을 모아놓은 페이지에 들어간다면? <br/>
해당 페이지에서 좋아요한 사진을 가져오는 api를 호출할 때, access token까지 같이 보내주면, 백앤드에서
1. 아! 이 token은 유효하구나
2. user_id는 5구나
3. user_id 5가 좋아요한 사진을 보내주자

이런 과정을 거쳐서 사진 목록을 response 해주는 것입니다.

### access token을 local storage에 저장하기
access token을 어디에 저장하면 좋을까요? 저희는 한 번만 로그인하면 더 이상 안 해도 되도록 Local Storage에 저장하겠습니다.

* **Local Storage**: 해당 도메인에 영구 저장하고 싶을 때
* **Session Storage**: 해당 도메인의, 한 세션에서만 저장하고 싶을 때. 창을 닫으면 data가 날라간다.
* **Cookie**: 해당 도메인에 날짜를 설정하고 그 때까지만 저장하고 싶을 때

세 가지 차이점에 대해 좀 더 자세히 알고 싶으면 [여기](https://www.quora.com/What-is-the-difference-between-sessionstorage-localstorage-and-Cookies)를 참조해주세요.

이제 로그인 후에 받아온 access token을 local storage에 저장해봅시다. `wtw-token`라는 키로 저장해보겠습니다. setItem 메서드를 사용하면 됩니다.
```javascript
fetch('http://localhost:8000/login/', {
  method: 'POST',
  headers: {
      'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    'id': 'kim',
    'password': '1234'
  })
})
.then(response => response.json())
.then(response => {
  if (response.token) {
    localStorage.setItem('wtw-token', response.token);
  }
})
```
개발자도구 -> Application tab -> Local Storage 에서 wtw-token 이름으로 된 data가 저장 됐으면 성공!
<img src="/media/190507-local-storage.png" width="500" />

### request header에 access token 보내기
이제는 api를 호출할 때마다 access token을 보내보도록 하겠습니다.
일단 백앤드에게 access token을 어디에 보내줘야 하냐고 물어봐야 합니다.

보통은 request header의 `Authorization`에 넣어 보내줍니다.

1. Local Storage에 `wtw-token` 존재하는지 확인
2. 있으면 request header의 `Authorization`에 추가하기

get api를 예제로 보겠습니다.
```javascript
let token = localStorage.getItem('wtw-token') || '';

fetch('http://localhost:8000/likes/', {
  headers: {
      'Authorization': token,
  }
})
.then(response => response.json())
.then(response => {
   console.log(response.data);
})
```
네! 이렇게 하면, 현재 로그인된 사람이 좋아요한 list를 가져올 것입니다! 끝!
