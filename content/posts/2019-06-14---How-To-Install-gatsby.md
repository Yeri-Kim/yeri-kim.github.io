---
title: Gatsby로 블로그 만들기
date: "2019-06-14T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/how-to-install-gatsby/"
category: "gatsby"
description: "Gatsby로 정적 블로그를 만들어 봅니다."
---

_[wecode](https://wecode.co.kr) 수강생 대상으로 작성한 포스팅입니다._

그 동안 배운 것들을 정리할 블로그를 만들어 보겠습니다. 블로그를 만들려면 어떤 페이지가 필요할까요?

바로 이전에 미니 트위터를 만들었는데, 그 것보다는 더 복잡할 것 같다는 느낌이 확실히 드네요!

* 블로그 홈(최신 글 모음)
* 카테고리 별로 포스팅 목록을 볼 수 있는 페이지
* 글 쓰는 페이지
* 로그인 페이지
* 등등..

HTML, CSS, JavaScript로 나만의 블로그를 만들려면 시간이 한참 걸릴 것 같습니다.

그래서...! 일일히 개발할 필요 없이 미리 어느 정도 만들어주는 site generator를 쓰려고 합니다.
여러 generator가 있는데 그 중에 [Gatsby](https://www.gatsbyjs.org/) 이라는 것을 사용하겠습니다.
몇 개의 명령어로, 단 몇 분만에 블로그 설치가 가능합니다.

우리는 블로그를 설치하고, 설정을 조금 수정하고 바로 배포해보겠습니다!
github를 이용해서 배포할 것이고, github에서 제공하는 도메인을 사용하려고 합니다.

#### 시작하기 전에
위에서 나온 새로운 단어 몇 개를 설명드리고 가겠습니다.

_`배포`_

`배포(deploy)한다.` or `디플로이한다.` 라는 말을 앞으로 자주 하고, 듣게 될 것입니다.

배포란, 그동안 개발하던 것을 세상(인터넷상)에 드디어 공개하고 모든 사람들이 접근해서 볼 수 있게 하는 것을 의미합니다.


_`도메인`_

도메인은 `wecode.co.kr`, `google.com`과 같은 영문 주소를 의미합니다.
원래는 110.13.109.111과 같은 숫자로 이루어진 ip주소인데, 사람들이 기억하기 쉽도록 영문주소로 바꿔 사용하고 있습니다.

나만의 블로그를 갖고 싶다면 도메인을 구입하고, 직접 ip 주소에 연결하는 과정을 거쳐야 하지만 github에서 Github Pages 라는 서비스를 사용할 예정입니다.
개인적으로 도메인을 구매했다면 자신만의 도메인을 연결할 수 있습니다.

#### 블로그 설치 순서
1. Gatsby 테마 고르기
2. Gatsby Starter로 블로그 설치(생성)
3. 내 컴퓨터(로컬환경)에서 블로그 띄우기
4. github.io repo만들기
5. Gatsby config 수정하기
6. 코드 올리기

### 0. 사전 준비

git과 npm이 깔려있어야 합니다.

### 1. Gatsby 테마 고르기
[Gatsby Starter](https://www.gatsbyjs.org/starters/?v=2) 사이트에서 테마를 하나 선택해주세요.
저는 [gatsby-starter-lumen](https://www.gatsbyjs.org/starters/alxshelepenok/gatsby-starter-lumen/) 으로 하겠습니다.
테마가 다르면 설정이 조금 달라집니다. 다음의 스텝을 그대로 따라하지 못 할 수도 있습니다.

### 2. Gatsby Starter로 블로그 설치(생성)

블로그를 만들고 싶은 directory에서 아래와 같이 실행합니다.
저는 blog라는 이름으로 만들어 주었습니다.
```
gatsby new blog https://github.com/alxshelepenok/gatsby-starter-lumen
```
위를 설치하면 앞으로 package manager를 yarn으로 할 것인지, npm으로 할 것인지 물어봅니다.
default가 yarn으로 설정되어있는 것 같은데 yarn을 따로 설치한적이 없는 분이라면 npm으로 선택해주세요.

https://github.com/alxshelepenok/gatsby-starter-lumen 는 gatsby-starter-lumen 의 github 주소입니다.

성공하면 현재 위치에 blog라는 디렉토리가 생성되고, 이 안에 각종 파일이 생성됩니다.
방금 만든 디렉토리로 가서 파일이 잘 생겼는지 확인해주세요.
```
cd blog
```
![](/media/190614-1.png)

blog 디렉토리 밑의 파일들은 모두 git에 올라갈 예정입니다.
그러므로 본인이 앞으로 소스코드를 모아놓을 곳에 생성해주세요.

저는 `/Users/AllieKim/Projects` 아래인
`/Users/AllieKim/Projects/blog`에 생성했습니다. 앞으로 `/Users/AllieKim/Projects/blog`를 블로그 루트(root)라고 표현하겠습니다.



### 3. 내 컴퓨터(로컬환경)에서 블로그 띄우기

설치가 제대로 되었는지 먼저 확인해봅시다.
```
yarn develop
```
아래와 같은 화면이 뜨면 성공입니다.
![](/media/190614-2.png)

브라우저를 열고 주소창에 localhost:8000으로 접속해보세요.

_`localhost`_

localhost, 로컬, 로컬환경 이라는 단어가 앞으로 많이 등장할 예정입니다. 로컬환경에 대해 잠깐 설명드리겠습니다.

웹상에 html, css, js를 작동시켜서 사이트를 보려면 서버가 필요합니다.
aws, cafe24 등에서 서버를 빌려서 우리의 블로그 파일들을 올리고 어느 누구든지 접근할 수 있게 하는 것을 호스팅이라고 합니다.
서버를 빌렸으니 렌탈비용을 내야하는데 이런 것을 호스팅 비용이라고 하죠. (참고로 우리는 github의 서버를 통해 호스팅하고, 물론 무료입니다!)

그런데 markdown으로 작성된 포스팅이 잘 나오는지 매번 github에 올린 후, 내 사이트에 접속해서 확인하기는 번거롭습니다.
아니면 블로그 css를 바꾸고 싶은데, 버튼 색 하나 바꾸고 잘 나오는지 확인하려면 github에 올린 후, 사이트에 접속해서 확인을 해야겠죠.

대신에 내 컴퓨터를 서버로 만들어서 사이트가 잘 나오는지 확인할 수 있습니다.
내 컴퓨터에서 볼 수 있기 때문에 `로컬환경` 이라고 말합니다.

개발할 동안, 내 컴퓨터에서 서버를 띄워서 `localhost:8000`으로 접속하면 html, css, js등을 올리고 잘 작동되는지 확인할 수 있습니다.
멈추고 싶으면 서버를 실행한 터미널에서 `ctrl c`를 입력해보세요.



### 4. github.io repo만들기
github에 접속해서 새로운 repository를 만들어 주세요. 아래와 같이 github의 `username` 뒤에 `.github.io` 을 붙여주세요.
제 github username은 `yeri-kim`이기 때문에 `yeri-kim.github.io` 으로 만들었습니다
![](/media/190227-5.png)

[yeri-kim.github.io](yeri-kim.github.io)의 도메인으로 접속할 수 있는 블로그 repository가 생성되었습니다!


### 5. Gatsby config 수정하기
코드를 git에 올리기 전에 몇 가지 수정할 것이 있습니다.

root 위치에서 package.json 파일을 찾아서 "script" 아래 부분의 "deploy"를 수정해주세요.
```
"deploy": "yarn run clean && gatsby build && gh-pages -d public -b master",
```

그리고 config.js 파일을 찾아서 나의 블로그 주소를 세팅해줍니다. module.exports 바로 밑에 url을 수정해줍니다.
github 주소가 아니라 블로그가 올라가는 주소입니다.

```json
module.exports = {
  url: 'https://yeri-kim.github.io/',

  // 아래 생략
```

### 6. 코드 올리기
이제 로컬에 있는 블로그 소스코드를 github에 올리겠습니다.

블로그 root에서 git을 세팅합시다. yeri-kim 부분은 각자의 username으로 해주세요.
```
git init
git remote add origin https://github.com/yeri-kim/yeri-kim.github.io.git
```


#### commit, push 하기
소스코드를 git에 올리는 과정입니다.
```
git add .
git commit -m “first commit”
git push -u origin master
```

내 github 의 해당 repo에 들어가서 소스코드가 모두 잘 올라갔는지 확인해주세요.

아직까지는 블로그에 선택한 gatsby 테마대로 올라가지 않은 상태입니다. 이제 배포를 해보겠습니다. 그래야 내 블로그 사이트에 제대로 올라갑니다.
```
yarn deploy
```
만약 npm 사용자라면
```
npm run deploy
```

네! 드디어 내 블로그가 세상에 공개되었습니다. 1~3분 정도 뒤에 블로그 주소에 접속해서 잘 나왔는지 확인해주세요.
