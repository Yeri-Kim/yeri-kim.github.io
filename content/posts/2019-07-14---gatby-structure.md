---
title: Gatsby 블로그 구조
date: "2019-07-14T13:42:32.169Z"
template: "post"
draft: true
slug: "/posts/gatsby-structure/"
category: "gatsby"
description: "블로그를 수정하기 위해 gatsby-gatsby-starter-lumen 테마 소스의 디렉토리 구조를 분석해보았습니다."
---

현재 블로그는 Gatsby 라는 react 기반 framework을 사용했고, 테마는 gatsby-starter-lumen를 사용했습니다.
기본 Gatsby 구조와 조금 다르긴 하지만, 결국 해당 테마도 Gatsby 기본 코드에서 확장한 것이므로 거의 비슷합니다.

조만간 블로그 검색기능을 추가하기위해 코드를 건들여야 하는데,
저도 수정할 때마다 매번 폴더 구조를 다시 훑어보게되어 이번엔 정리하기로 하였습니다.

전부 다 작성하지 않고, 중요 디렉토리, 파일만 나열했습니다.

```
- content
  |- pages: about.md 같은 독립적인 페이지
  |- posts: 모든 블로그 글
|- gatsby
  |- pagination
    |- create-categories-pages.js
  |- create-pages.js
  |- on-create-node.js
```
