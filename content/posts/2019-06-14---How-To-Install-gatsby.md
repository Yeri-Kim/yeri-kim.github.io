---
title: Gatsby로 블로그 만들기
date: "2019-06-14T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/how-to-install-gatsby/"
category: "gatsby"
description: "Gatsby로 정적 블로그를 만들어 봅니다."
tags:
  - "gatsby"
  - "localhost"
  - "호스팅"
---

_[wecode](https://wecode.co.kr) 수강생 대상으로 작성한 포스팅입니다._

그 동안 배운 것들을 정리할 블로그를 만들어 보겠습니다. 블로그를 만들려면 어떤 페이지가 필요할까요?

바로 이전에 미니 트위터를 만들었는데, 그 것보다는 더 복잡할 것 같다는 느낌이 확실히 드네요!

- 블로그 홈(최신 글 모음)
- 카테고리 별로 포스팅 목록을 볼 수 있는 페이지
- 글 쓰는 페이지
- 로그인 페이지
- 등등..

HTML, CSS, JavaScript로 나만의 블로그를 만들려면 시간이 한참 걸릴 것 같습니다.

그래서! 일일이 개발할 필요 없이, 블로그 사이트를 만들어주는 site generator를 쓰려고 합니다.
원하는대로 블로그 커스터마이징도 가능합니다.
여러 generator가 있는데 그 중에 [Gatsby](https://www.gatsbyjs.org/) 이라는 것을 사용하겠습니다.
몇 개의 명령어로, 단 몇 분만에 블로그 설치가 가능합니다.

우리는 블로그를 설치하고, 설정을 조금 수정하고, 글을 작성하고 배포하면 됩니다.
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
`username.github.io` 라는 도메인을 갖게됩니다.
개인적으로 도메인을 구매했다면 자신만의 도메인을 연결할 수 있습니다.

#### 블로그 설치 순서

1. Gatsby 테마 고르기
2. Gatsby Starter로 블로그 설치
3. 내 컴퓨터(로컬환경)에서 블로그 띄우기
4. Gatsby config 수정하기
5. github.io repo만들기
6. 배포하기

### 0. 사전 준비

git과 npm이 깔려있어야 합니다.

### 1. Gatsby 테마 고르기

[Gatsby Starter](https://www.gatsbyjs.org/starters/?v=2) 사이트에서 테마를 하나 선택해주세요.
저는 [gatsby-starter-lumen](https://www.gatsbyjs.org/starters/alxshelepenok/gatsby-starter-lumen/) 으로 하겠습니다.
테마가 다르면 설정이 조금 달라집니다. 다음의 스텝을 그대로 따라하지 못 할 수도 있습니다.

### 2. Gatsby Starter로 블로그 설치(생성)

이왕 gatsby로 블로그를 쓰기로한거, 터미널에서 gatsby 명령어를 사용할 수 있도록 `gatsby-cli`를 전역에 설치합니다.

```
npm install -g gatsby-cli
```

그다음에는 우리가 정한 theme의 source code를 가져오려고 합니다.
블로그를 만들고 싶은 directory에서 아래와 같이 실행합니다.
gatsby 명령어를 사용하여 blog라는 디렉토리에 블로그의 소스코드를 가져온다는 의미입니다.

```
gatsby new blog https://github.com/alxshelepenok/gatsby-starter-lumen
```

우측의 https://github.com/alxshelepenok/gatsby-starter-lumen 는 gatsby-starter-lumen이라는 theme의 github 주소입니다.

위를 설치하면 앞으로 package manager를 yarn으로 할 것인지, npm으로 할 것인지 물어봅니다.
default가 yarn으로 설정되어있는 것 같은데 yarn을 따로 설치한적이 없는 분이라면 npm으로 선택해주세요.

yarn을 설치하려면 아래와 같이 하면 됩니다.

```
npm install -g yarn
```

성공하면 현재 위치에 blog라는 디렉토리가 생성되고, 이 안에 각종 파일이 생성됩니다.
방금 만든 디렉토리(blog)로 가서 파일이 잘 생겼는지 확인해주세요.

```
cd blog
```

![](/media/190614-1.png)

### 3. 내 컴퓨터(로컬환경)에서 블로그 띄우기

설치가 제대로 되었는지 확인해봅시다.
앞으로 블로그 글을 작성할 때마다 중간 점검을 하기 위해서는 아래 명령어로 로컬 서버를 띄워줘야합니다.

```
yarn develop
```

아래와 같은 화면이 뜨면 이제까지의 스텝을 잘 따라왔다는 뜻입니다.
![](/media/190614-2.png)

이제 브라우저(크롬)를 열고 주소창에 localhost:8000으로 접속해보세요. 아래와 같은 화면이 떠야합니다.
제가 선택한 테마의 메인 화면입니다.
![](/media/190614-5.png)

잠시 용어를 설명하고 넘어가겠습니다.

_`localhost`_

localhost, 로컬, 로컬환경 이라는 단어가 앞으로 많이 등장할 예정입니다. 로컬환경에 대해 잠깐 설명드리겠습니다.

웹상에 html, css, js를 작동시켜서 사이트를 보려면 서버가 필요합니다.
aws, cafe24 등에서 서버를 빌려서 우리의 블로그 파일들을 올리고 어느 누구든지 접근할 수 있게 하는 것을 호스팅이라고 합니다.
서버를 빌렸으니 렌탈비용을 내야하는데 이런 것을 호스팅 비용이라고 하죠. (참고로 우리는 github의 서버를 통해 호스팅하고, 물론 무료입니다!)

그런데 markdown으로 작성된 포스팅이 잘 나오는지 매번 github에 올린 후, 내 사이트에 접속해서 확인하기는 번거롭습니다.
또는 블로그 css를 바꾸고 싶은데, 버튼 색 하나만 바꾸더라도 잘 나오는지 확인하려면 github에 올린 후, 사이트에 접속해서 확인을 해야겠죠.

이렇게 매번 복잡한 과정을 거쳐서 실제 사이트에 반영된 결과물을 보는 대신에,
수정 직후 내 컴퓨터에 서버를 띄워서 바로바로 확인할 수도 있습니다.

내 컴퓨터에 서버를 띄웠기 때문에 `로컬환경` 이라고 말합니다.
블로그를 작성하는 도중에 생각대로 markdown이 잘 적용됐는지, 아닌지를 확인하려면
내 컴퓨터에서 `yarn develop` 명령어로 서버를 띄워서 `localhost:8000`으로 접속하면 됩니다.

위 방법으로 서버를 띄우지 않으면 `localhost:8000` 로 중간 결과물을 볼 수가 없습니다.
또한 로컬서버가 켜있는 터미널을 닫으면 서버가 종료되므로 더이상 `localhost:8000`에 접속할 수 없고요.
포스팅 작성 시작할때부터 배포 직전까지 로컬서버를 띄우고 터미널을 계속 열어두면 됩니다.

만약 멈추고 싶으면 서버를 실행한 터미널에서 `ctrl c`를 입력해보세요.

### 4. Gatsby config 수정하기

블로그 세팅 몇 가지 수정하겠습니다.

#### config.js

`config.js` 파일을 찾아주세요.
title, subtitle, author 등등 본인 정보로 수정해주시면 됩니다.

이 중에서 url은 블로그 주소를 쓰면 되는데 `https://깃헙유저네임.github.io/` 입니다.
5번을 참고해서 작성해주시면 됩니다.

```json
module.exports = {
  url: 'https://yeri-kim.github.io/',

  // 아래 생략
```

#### package.json

`src/package.json` 파일에서 "script" 아래 부분의 "deploy"를 수정해주세요.

```
"deploy": "yarn run clean && gatsby build && gh-pages -d public -b master",
```

앞으로 포스팅 작성을 완료하고, 로컬에서 잘 확인했으면 `yarn deploy` 로 배포합니다.
이때 소스코드 빌드 후의 public 폴더를 github master 브랜치에 푸시한다는 뜻이고,
이 소스코드로 https://yeri-kim.github.io/ 에 배포됩니다.

### 5. github.io repo만들기

블로그 설치는 거의 끝나갑니다. 이제 자신의 github에 소스코드만 올리면 됩니다.

github에 접속해서 새로운 repository(이하 repo)를 만들어 주세요.
그런데 repo 이름에 유의하셔야 합니다!
아래와 같이 github의 `username` 뒤에 `.github.io` 을 붙여서 만들어주세요.
제 github username은 `yeri-kim`이기 때문에 `yeri-kim.github.io` 으로 만들었습니다.

![](/media/190227-5.png)

[yeri-kim.github.io](yeri-kim.github.io)의 도메인으로 접속할 수 있는 블로그 repo가 생성되었습니다!

### 6. 배포하기

이제 로컬에 있는 블로그 소스코드를 github에 올리겠습니다.

블로그 root에서 git을 세팅합시다. yeri-kim 부분은 각자의 username으로 해주세요.

```
git init
git remote add origin https://github.com/yeri-kim/yeri-kim.github.io.git
```

origin git과 잘 연결되어도 아무 반응이 없습니다. 아래의 명령어로 origin 주소가 제대로 나오는지 확인해주세요.

```
git remote -v
```

푸시가 잘 되는지 확인해보겠습니다.

```
git add .
git commit -m "first commit"
git push origin master
```

github에 가셔서 소스코드가 잘 올라왔는지 확인해주세요.

잘 됐으면 배포 명령어를 실행합니다. 성공적으로 끝나면 1~2분 뒤에 블로그 주소로 들어가보세요.

```
yarn deploy
```

정상적으로 배포 됐다면 github 사이트의 블로그 repo 가서 master에 올라온 소스코드르 봐주세요.
네 저희가 원래 받았던 소스코드 구조와 완전히 다릅니다.
![](/media/190614-6.png)

해당 소스는 포스팅을 작성할 수 있고, 커스터마이징 할 수 있는 개발 소스가 아니라
블로그 사이트에 배포될 수 있도록 md 파일이 모두 html, js로 바뀐 빌드된 파일입니다.

`yarn deploy` 라는 명령어가 알아서 build도 해주고, git의 master에 push도 해주고 배포도 해준것입니다.

앞으로 항상 deploy만 해도 되지만, 혹시 컴퓨터를 바꾸거나 다른데서 포스팅 작성을 할 경우를 대비하여
개발코드도 보존하기로 하죠. develop이라는 브랜치를 따로 만들어서 여기에다만 올리도록 하겠습니다.

브랜치 생성 및 이동 (모든 명령어는 한줄씩 작성하고 엔터로 실행)

```
git branch develop
git checkout develop
```

소스코드를 git에 올리는 과정.

```
git add .
git commit -m “blog posting~~”
git push -u origin develop
```

내 github 의 develop repo에 들어가서 소스코드가 모두 잘 올라갔는지 확인해주세요.

잘 올라갔다면 github default 브랜치를 develop으로 바꾸겠습니다.
어차피 빌드 결과물인 master 브랜치의 빌드 파일들을 파악할 필요도 없으니까요.
github의 해당 블로그 repo 페이지에 들어가서 Settings -> Branches 메뉴에서 Default branch 를 develop으로 바꾸고 update 버튼을 눌러주세요!

블로그 생성 끝~~~~~~~~~~
