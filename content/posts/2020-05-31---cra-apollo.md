---
title: "create-react-app에 apollo 붙이기"
date: "2020-05-31T10:43:32.169Z"
template: "post"
draft: false
slug: "/posts/cra-apollo/"
category: "Graphql"
description: "기존 Create React App에 Graphql client - Apollo를 붙여봅니다."
tags:
  - "apllol"
  - "graphql"
---

회사 어드민 페이지는 CRA에 firebase 로 간단하게 구현되어있는데 이 번에 제대로 백앤드도 붙이면서 Graphql로 넘어가기로 하였습니다.
Graphql client는 relay와 apollo가 있는데 relay는 실무에서 1년정도 쓰다보니 꽤 익숙한 반면, apollo는 처음이라 apollo를 붙여보기로 했습니다.

apollo가 훨씬 쉽고 자료가 더 많다고 하는데, 
학생들 중에서도 graphql을 쓰는 경우가 많아지다 보니 apollo 질문에도 대응할 수 있으면 좋겠다 싶어서 선택했습니다.
간단한 어드민 페이지에서만 apollo를 쓰고, 추후 더 큰 프로젝트는 relay를 사용할 예정입니다.
나중에 relay와 apollo의 차이점도 작성해보도록 하겠습니다.

### apollo 설치
이미 restful api를 호출하고 있는 create react app에 설치하는 것이고 전부 graphql로 바꿀 예정입니다.
graphql query가 나올 때마다 하나씩 수정해도 됩니다. 기존에 되어있던 restful api는 굳이 삭제하지 않아도 됩니다.

```
npm install apollo-boost @apollo/react-hooks graphql
```

### apollo provide로 감싸주기
index.js에서 아래와 같이 설정합니다. 모든 컴포넌트에서 사용할 예정이므로 모든 컴포넌트의 가장 상위 폴더에서 Provider로 감싸줍니다.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import ApolloClient from "apollo-boost";
import { ApolloProvider } from "@apollo/react-hooks";
import Routes from "./Routes";

const client = new ApolloClient({
  uri: "https://staging.api.***.com/graphql",
});

ReactDOM.render(
  <ApolloProvider client={client}>
    <Routes />
  </ApolloProvider>,
  document.getElementById("root")
);
```

### 테스트!

아직 query가 없어서 테스트로 만든 query를 호출해보았습니다.

```jsx
import React from "react";
import { useQuery } from "@apollo/react-hooks";
import { gql } from "apollo-boost";

const PINT_TEST = gql`
  query {
    ping {
      pong
    }
  }
`;

const ApolloTest = () => {
  const { loading, error, data } = useQuery(PINT_TEST);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error :(</p>;

  return <p>{data.ping.pong}</p>;
};

export default ApolloTest;
```

graphql 화면은 아래와 같고요, 이렇게 ping query가 있습니다.

![](/media/200531-graphql.png)

화면에 아주 잘 뜨는군요! relay할 때는 1-2주일 밤새면서 엉엉 울었던 기억이 있는데 apollo는 10분만에 끝났습니다 ㅎㅎ
apollo가 쉬워서 그런 것도 있지만 relay 경험이 있어서 graphql에 익숙해서 그런거겠죠?

제가 정말 좋아하는 임백준님의 한 문장 쓰고 갑니다.
> 실력은 고통의 총합이다

여러분! 오늘도 화이팅입니다. 지금 겪고 있는 삽질이 곧 조만간 나의 실력이 될거예요.

### Reference
* https://www.apollographql.com/docs/
