---
title: Jekyll 수정하기
date: "2019-04-16T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/update-jekyll"
category: "jekyll"
tags:
  - "jekyll"
description: "Jekyll 의 기본테마는 너무 많은 사람들이 사용하고 있기에, 조금 수정해보도록 하겠습니다!"
---

# Jekyll 수정하기

Jekyll 의 기본테마는 너무 많은 사람들이 사용하고 있기에, 조금 수정해보도록 하겠습니다!

수정을 하려면 jekyll의 디렉토리 구조를 먼저 파악해봐야겠죠?

### 1. jekyll의 디렉토리 구조 파악하기
jekyll의 디렉토리는 jekyll이 정해진대로 짜여있습니다.
우리가 직접 블로그를 개발했다면 디렉토리를 마음대로 수정했겠지만, 지금은 마음대로 수정하면 블로그가 제대로 뜨지 않을 수도 있습니다.

앞으로 이렇게 미리 정해진 디렉토리 구조를 많이 접할텐데, 구조를 먼저 파악하면 개발 속도가 빨라집니다.

* **`_config.yml`** 설정파일입니다. 블로그 제목이나 이메일 등을 수정하면 실제 블로그에 적용이 됩니다. 각자 설정에 맞게 수정해주세요.
* **`_includes/`** 여러 페이지에서 반복할 코드의 템플릿이 있는 디렉토리입니다. footer, header등을 넣을 수 있습니다.
* **`_posts/`** 이 디렉토리 밑에 글을 쓰면 됩니다. jekyll이 정한 이름 convention을 잘 지켜주셔야 포스팅이 올라갑니다. _`yyyy-mm-dd-title.markdown`_
* **`_layouts/`** 글의 디자인이 적용된 템플릿이 있는 디렉토리 입니다.
* **`_site`** 자동으로 생성되는 디렉토리입니다! 수정하셔도 반영이 되지 않습니다. 블로그 사이트에 최종적으로 올라갈 결과물입니다. md로 작성한 포스트가 html파일로 변환되어 자동으로 생성됩니다.

### 2. 간단한 HTML/CSS 변경

현재 보여지는 디자인은 default theme입니다. theme이름은 `minima`입니다. theme에는 어떻게 보여줄지 구조를 잡아놓은 html과 디자인을 입혀줄 css 파일들이 있습니다.

최초에 설치하고나서는 theme관련 디렉토리가 존재하지 않을 것입니다. theme관련 파일들은 해당 디렉토리에 생성되지 않습니다. 다른 어딘가에 저장되어있습니다. default theme인 minima가 어디에 위치해있는지 확인해보겠습니다.

```
bundle show minima
```

저의 경로는 `/Users/AllieKim/.rvm/gems/ruby-2.4.2/gems/minima-2.5.0`라고 나오네요.
해당경로로 이동하면 아래의 파일들이 있습니다.
<img src="/media/190227-3.png" width="500"/>

이중에서 수정하고 싶은 요소를 가져와서 변경하면, jeykyll에서 overwrite하여 보여주게 됩니다.
이 중에서 `_includes/footer.html` 을 가져와서 수정해보겠습니다.

`/Users/AllieKim/.rvm/gems/ruby-2.4.2/gems/minima-2.5.0/_includes/footer.html`파일을 복사해서 현재 블로그 root 밑에 `_includes/footer.html`에 붙여넣어주세요.

css도 수정하려고 하니 `_sass/minima/_layout.scss`, `_sass/minima.scss`도 가져와주세요.


#### html 수정
footer부분을 수정하겠습니다. `.wecode-logo` 부분을 그대로 추가해주세요.
```html
 <!-- file: _includes/footer.html -->

 <div class="footer-col footer-col-3">
    <p>{{- site.description | escape -}}</p>

    <div class="wecode-logo">
      <span>Powered by</span>
      <a href="https://wecode.co.kr" target="_blank">
        <img src="https://s3.ap-northeast-2.amazonaws.com/cdn.wecode.co.kr/logo/wecode_logo.png" alt="wecode" />
      </a>
    </div>
</div>
```

#### css 수정
아래 내용을 `_sass/minima/_layout.scss` 맨 아래쪽에 추가해주세요.
```scss
// file: _sass/minima/_layout.scss

.wecode-logo {
  img {
    width: 90px;
    margin-top: -3px;
    background-color: black;
  }
}
```
아래와 같이 powered by wecode가 추가되면 성공!
![](/media/190416-2-1.png)
