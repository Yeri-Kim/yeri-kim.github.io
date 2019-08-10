---
title: Jekyll에 Google Analytics 붙이기
date: "2019-05-06T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/google-analytics-jekyll/"
category: "jekyll"
tags:
  - "jekyll"
  - "ga"
---


요즘 우리 wecoders! 1일 1포스팅에 열정적인데 :heart_eyes: 아마 본인 블로그가 검색이 되는지?
된다면 얼마나 들어오는지 궁금하실 것 같아요.

Google Analytics(이하 GA)를 추가해서 알아보도록 하겠습니다.

GA를 직접 추가하려고 봤더니 minima 테마에 `_includes/google-analytics.html` 이라는 파일이 있더라구요.
아마도 쓰는 사람이 많아서 추가하기 좋게 미리 세팅되어있는 것 같습니다.

어떻게 알았냐구요? 다시 한 번 minima 테마 위치를 찾아서
```
bundle show minima
```
_includes 디렉토리 밑에 있에 google-analytics.html 파일이 있습니다!<br/>
코드도 한 번 살펴봅시다.
```html
<script>
if(!(window.doNotTrack === "1" || navigator.doNotTrack === "1" || navigator.doNotTrack === "yes" || navigator.msDoNotTrack === "1")) {
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', '{ site.google_analytics }', 'auto');
  ga('send', 'pageview');
}
</script>
```
네! 원래는 위의 코드를 body 태그 안에 직접 넣어주어야 하는데, jekyll에서 미리 만들어 놓은 소스코드를 보니 `{ site.google_analytics }` 이 부분만 넣어주면 되는군요.

`_config.yml` 파일에 google_analytics 변수에 trackingID를 넣어주면 될 것 같은 느낌적인 느낌 :sunglasses:

내 블로그를 위한 GA의 trackingID를 얻기 위해 GA를 가입합시다!

### GA 가입하기

[GA 홈](https://analytics.google.com)에서 Sign Up을 누르고 아래와 같이 세팅했습니다.
<img src="/media/190506-ga.png" width="340" />

가입하자마자 첫 화면에 Tracking ID가 나올거예요.<br/>
저는 `UA-1397***33-1` 입니다. (노출되도 상관은 없지만 조금 가려봅니다 :kissing_closed_eyes:)

### _config.yml 파일에 google_analytics 추가
`_config.yml` 파일 마지막 어딘가에 아래 코드를 추가해주었습니다.
```
# GA
google_analytics: UA-1397***33-1
```
`_config.yml` 파일에 이렇게 추가해주면, jekyll의 html에서 site 변수로 접근이 가능합니다.
그래서 google-analytics.html 파일에 `site.google_analytics`가 있었던 것이죠.

여기까지 잘 따라오셨으면 git push 해주세요! 그래야 사이트에 올라가게 되고 그 때부터 GA가 체크하게 됩니다.
만약 본인의 github.io에 google_analytics를 추가한 _config.yml이 올라간게 확실하다면, GA에서 Home을 눌러주세요.

아래와 같이 나오시죠? 참 쉽죠?
![](/media/190506-ga1.png)

GA 추가하고, 하루이틀, 일주일 지났는데 나 혼자 밖에 안 들어와..<br>
그래도 좌절하지 말고 1일 1포스팅 열심히 합시다!<br/>
제가 곧 검색노출이 잘 되도록 여러 가지 방안(SEO 최적화)을 들고 나타나겠습니다.
