---
title: 내 블로그 구글검색에 노출 시키기
date: "2019-05-06"
template: "post"
draft: false
slug: "/posts/google-search-jekyll/"
category: "jekyll"
tags:
  - "jekyll"
---

아니 이런,<br/>
열심히 GA를 붙였는데.. jekyll로 만든 블로그는 자동 검색 노출이 되지 않는가보네요.

어서어서 수정하도록 합시다.

### Google Search Console
1.[Google Search Console](https://search.google.com/search-console/welcome) 접속 후 우측에 내 블로그 주소 작성 -> continue
![](/media/190506-search.png)
이 과정에서 해당 사이트 주인인지 verify가 필요한데 이미 설정한 GA를 통해 verify를 했습니다.<br/>
혹시 GA를 설정 안 했다면 어떤 식으로 verify 할지는 잘 모르겠네요?

2.사이트맵 만들기

jeykyll plugin을 통해 만들 수도 있지만, 어렵지 않으니 직접 만들어보도록 하겠습니다. 내 블로그의 root directory, 즉 Gemfile과 _config.yml이 있는 곳에 `sitemap.xml`이라는 이름으로 파일을 생성해주세요.

아래에 복붙할 수 있도록 코드를 작성했었는데, 쓴 그대로 나오는게 아니라 Liquid tag를 해석해서 나온 결과물이 나오므로 이미지 캡쳐만 드립니다.
코드는 [github](https://github.com/Yeri-Kim/yeri-kim.github.io/blob/master/sitemap.xml)에서 확인해주세요.
<img src="/media/190506-xml.png" width="400" />

3.위의 코드에서 보듯이 site.url을 사용하고 있으니 _config.yml에 url을 추가해주세요.
```
url: https://yeri-kim.github.io/
```
4.Google Search Console 에서 sitemap.xml url 추가 후 submit
![](/media/190506-sitemap.png)

끄읕! 참 쉽죠?

#### reference
* http://www.independent-software.com/generating-a-sitemap-xml-with-jekyll-without-a-plugin.html