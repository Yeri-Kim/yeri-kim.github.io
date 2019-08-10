---
title: 탭 타이틀의 이미지(favicon) 교체하기
date: "2019-06-15T23:41:32.169Z"
template: "post"
draft: false
slug: "/posts/how-to-change-favicon/"
category: "gatsby"
description: "gatsby-starter-lumen 을 사용하면 브라우저 탭에 보이는 이미지가 수염난 아저씨로 되어있습니다. favicon 바꾸는 위치를 알아봅시다."
tags:
  - "gatsby"
---

탭에 보이는 이미지를 favicon(파비콘) 이라고 부릅니다.

![](/media/190615-1.png)

원래 favicon의 세팅은 index.html에서 하지만 gatsby에서는 따로 설정파일이 있습니다.

root의 gatsby-config.js에서 174번째 라인인 `icon`을 수정해주세요. 원래 photo.jpg으로 되어있던 것을 제 사진이 나오는 profile.png로 바꿨습니다.
profile.png의 위치는 root의 static/profile.png 입니다.

```json
{
  resolve: 'gatsby-plugin-manifest',
  options: {
    name: siteConfig.title,
    short_name: siteConfig.title,
    start_url: '/',
    background_color: '#FFF',
    theme_color: '#F7A046',
    display: 'standalone',
    icon: 'static/profile.png'
  },
}
```

성공!!
![](/media/190615-2.png)
