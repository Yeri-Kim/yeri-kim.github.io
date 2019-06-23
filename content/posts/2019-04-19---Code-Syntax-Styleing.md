---
title: code block styling
date: "2019-04-19T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/code-syntax-styleing/"
category: "jekyll"
tags:
  - "jekyll"
---

# code block 꾸미기

_[wecode](https://wecode.co.kr) 수강생을 위해 작성했습니다 :)_

default theme인 minima의 code block 스타일이 어색해서 좀 바꿔보겠습니다.
<img src="/media/190419-1.png" width="400">

제가 사용하는 IDE는 Webstorm이고 테마는 Darcula을 사용하고 있습니다. 워낙 Webstorm 색깔에 익숙해져서 최대한 비슷하게 수정해보려고 합니다.
일단 minima에서 code syntax highlighting에 해당하는 css를 가져와야 합니다.

아래의 명령어로 minima의 위치를 가져오면 저는 `/Users/AllieKim/.rvm/gems/ruby-2.4.2/gems/minima-2.5.0` 라고 나옵니다.
```
bundle show minima
```

해당 디렉토리의 `_sass/minima.scss`와 `_sass/minima/_syntax-highlighting.scss` 파일을 복사해서 내 블로그 root밑에 똑같은 경로로 넣어주세요.

제 블로그 디렉토리는 myblog이기 때문에 `myblog/_sass`밑에 두 파일을 넣어주었습니다.

이제부터 개발자도구를 열고, 바꾸고 싶은 구문의 class를 찾아서 color를 하나씩, 하나씩 바꿔줍니다!
![](/media/190419-2.png)
아! "태그p"에 해당하는 class는 `nt` 니까 `_syntax-highlighting.scss`에서 `.nt`부분을 찾아 color속성을 바꿔주면 되겠군!

참 쉽죠?

제 [_syntax-highlighting.scss 파일](https://github.com/Yeri-Kim/yeri-kim.github.io/blob/master/_sass/minima/_syntax-highlighting.scss)은 여기서 확인해주세요!
css 귀찮은데 검정 배경을 원하시는 분은 그대로 가져다 쓰세요. 저도 하다보니 jsx, javascript, python 위주로만 color를 바꿨습니다.
다른 코드를 작성하다가 어울리지 않은 색깔이 나오면 직접 바꿔주세요 :)