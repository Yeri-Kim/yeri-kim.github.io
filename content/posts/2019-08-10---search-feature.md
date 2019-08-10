---
title: "gatsby 블로그에 검색기능을 추가할 수 있을까?"
date: "2019-08-10T05:43:32.169Z"
template: "post"
draft: false
slug: "/posts/blog-search/"
category: "gatsby"
description: "gatsby로 만든 블로그에 검색기능을 추가해보려고 합니다. 어떻게 추가할 수 있을까요?"
tags:
  - "gatsby"
  - "graqhql"
---

## 검색이 가능한지 알아보기
gatsby는 graphql 기반이기 때문에, graphql 쿼리로 검색이 가능한지 먼저 알아보겠습니다.
개발 서버를 띄우면 터미널에 아래의 설명이 나옵니다.
> View GraphiQL, an in-browser IDE, to explore your site's data and schema
> http://localhost:8000/___graphql

위의 주소로 들어가면 gatsby 블로그의 데이터와 스키마를 전부 볼 수 있고, 궁금한 쿼리도 직접 쳐볼 수 있습니다.
![](/media/190810-1.png)

왼쪽 사이드는 가능한 쿼리 목록이 있고, 오른쪽 사이드는 스키마를 볼 수 있습니다.
gatsby 블로그는 미리 정의된 쿼리로 포스팅 목록 페이지, 포스팅 페이지, 카테고리 페이지 등을 정적으로 만들어주는 것입니다.

그래서 만약 단어로 포스팅 목록을 가져오는 쿼리를 알 수 있다면, gatsby blog를 커스터마이징 할 수 있다는 희망이 있는 것이죠.

일단 연습도 해볼겸 전체 목록을 가져오는 쿼리를 보겠습니다.
![](/media/190810-2.png)

제가 검색한건 모든 markdown 을 가져오되, 필터조건은 template이 post이고, draft는 true가 아닌 것입니다.
그리고 쿼리 결과는 포스팅의 title만 가져오도록 했습니다.

저는 이전 실무에서 graphql, relay 을 한 적이 있어서 쿼리에 익숙하지만, 익숙하지도 않고 처음에 어떤 쿼리를 해봐야할지 모르겠다면
gatsby 소스코드에서 이미 정의된 쿼리를 검색해서 해보면 됩니다.
위의 쿼리도 `/src/templates/index-template.js` 에 있던 쿼리를 가져온 것입니다.

그럼 이제 특정 단어가 들어간 포스팅 목록을 가져오는 쿼리를 작성해볼까요? 실제 쿼리가 잘 됐는지 확인하기 위해 이번에는 결과에 포스팅 목록까지 가져오겠습니다.

넹!! 필터조건 하나 추가했는데 잘 나오는군요?
![](/media/190810-3.png)

쿼리는 아래와 같습니다. 포함하고 싶은 단어를 정규 표현식으로 필터링했습니다.
```graphql
query MyQuery {
  allMarkdownRemark(filter: {frontmatter: {draft: {ne: true}, template: {eq: "post"}}, rawMarkdownBody: {regex: "/react native/i"}}) {
    edges {
      node {
        frontmatter {
          title
        }
        rawMarkdownBody
      }
    }
  }
}
```

네! 쿼리를 찾으니 왠지 블로그에 검색기능을 추가할 수 있을 것만 같네요!
그런데 여기까지 하자마자 제가 잘 못 생각했다는 것을 깨달았습니다.

### 결론
gatsby blog는 정적이므로 검색이 가능하게 하려면 검색 할 것만 같은 단어를 미리 다 정의한 후에
검색 결과 페이지를 정적으로 생성해놔야합니다.
네.. 제 머리로는 할 수 없다는 것으로 잠정 결론 내리고 search 대신에 tag목록을 활용해서 포스팅을 쉽게 찾을 수 있도록 전략을 바꾸겠습니다 ㅠㅠ

### 관련 포스팅
[gatsby 블로그에 태그목록 추가하기](https://yeri-kim.github.io/posts/blog-search/)

