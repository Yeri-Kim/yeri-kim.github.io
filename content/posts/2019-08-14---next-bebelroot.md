---
title: "Next.js에서 babel-plugin-root-import 사용하기"
date: "2019-08-15T14:43:32.169Z"
template: "post"
draft: true
slug: "/posts/next-bebelroot/"
category: "Next.js"
description: "TypeScript기반인 Next.js에서 babel-plugin-root-import를 사용할 수 있도록 설정합니다."
tags:
  - "next"
  - "typescript"
  - "babel"
---

한 동안 react에서 절대경로를 사용하기 위해 NODE_PATH 환경변수를 사용했는데,
어느 순간부터 터미널에 deprecated 됐다고 떠서 방법을 바꾸기로 하였습니다.
(그리고 2기 wecoder 분이 Next.js로 프로젝트를 했는데 NODE PATH 로 절대경로를 설정할 수 없다고 했던 것 같습니다)

Next.js는 babel plugin을 쉽게 추가할 수 있으므로,
여러 프로젝트에서 만나보았던 [babel-plugin-root-import](https://github.com/entwicklerstube/babel-plugin-root-import)
을 사용하려고 합니다.
해당 플러그인을 사용하면 `~` 물결표시를 절대경로로 사용할 수 있습니다.

## babel-plugin-root-import 설치
```
yarn add babel-plugin-root-import --dev
```

## 절대경로 사용
CRA는 src를 root path로 사용하는데, Next.js는 프로젝트 루트가 곧 소스코드의 root path입니다.


```
- .next
- config
    |- google-login.tsx
-- pages
    |- index.tsx
    |- login.tsx
(생략..)
```

login.tsx 에서 google-login.tsx의 `GOOGLE_CLIENT_ID`를 가져오기 위해 아래와 같이 바꿔주었습니다.
```js
// babel-plugin-root-import 사용 전
import { GOOGLE_CLIENT_ID } from '../config/google-login';

// babel-plugin-root-import 사용 후
import { GOOGLE_CLIENT_ID } from '~/config/google-login';
```

위와 같이 바꾸면 TypeScript(확장자가 .tsx)을 쓸 경우,
`can't not find module` 이라는 에러가 뜹니다.
`~`를 인지하지 못하기에 절대 경로를 잡아주지 못합니다.

## tsconfig.json 수정
기존에 있던 세팅에 `baseUrl`와 `paths`를 추가하여 `~`가 곧 루트임을 설정해줍니다.
```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "~/*": ["./*"]
    },
    (생략..)
  }
}
```

만약 일반 react 프로젝트라면 paths 설정에서 `"~/*": ["src/*"]` 로 바꿔주어 src를 root로 설정하면 됩니다.
