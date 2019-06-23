---
title: Jekyll에 Emoji 추가하기
date: "2019-05-06T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/emoji-jekyll/"
category: "jekyll"
tags:
  - "jekyll"
---

블로그에 감정표현을 더 풍부하게 하기 위해 emoji를 추가해보겠습니다. <br/>
애플 emoji에만 익숙한데, 막상 설치하고 보니 낯설어..:worried: 아마도 구글 emoji 같네요.

### `_config.yml`에 [jemoji](https://github.com/jekyll/jemoji) 추가
```
plugins:
  - jekyll-feed
  - jemoji
```
원래 jekyll-feed만 있었는데 `jemoji`를 추가했습니다. `jemoji`는 jekyll을 위한 emoji 플러그인 입니다.

추가 후, server 재실행 했는데 문제 없으면 성공!

### 에러가 났을 경우
저는 아래와 같은 에러가 났습니다.
```
Configuration file: /Users/AllieKim/WebstormProjects/myblog/_config.yml
  Dependency Error: Yikes! It looks like you don't have jemoji or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jemoji' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!
jekyll 3.8.5 | Error:  jemoji
```

해당 emoji 플러그인을 사용하려면 로컬에 github-pages이 깔려 있어야 하는 것 같습니다.

#### Gemfile 수정
Gemfile에서 아래 부분 # 삭제하여 주석처리 제거
```
gem "github-pages", group: :jekyll_plugins
```
수정 후,
```
bundle install
```

윽. 또 한 번의 에러가 났습니다.
<img src="/media/190506-error.png" width="500" />
아마도 gem이나 jekyll의 버전이 맞지 않는가봐요? 검색을 해보니 update를 한 번 하라고 합니다.

```
bundle update
```

서버 재실행 후 확인하니? finally... :+1:

### Emoji 사용
emoji 코드를 모두 기억하긴 어렵고요. 아래에서 쓰고 싶은거 찾아서 본문 중간에 `:+1:` 이런식으로 넣어주면 됩니다.

[emoji 목록](https://gist.github.com/rxaviers/7360908)


### reference
* https://help.github.com/en/articles/emoji-on-github-pages