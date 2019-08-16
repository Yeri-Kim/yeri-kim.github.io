---
title: "Next.js에서 TypeScript 사용하기"
date: "2019-08-15T10:43:32.169Z"
template: "post"
draft: true
slug: "/posts/next-typescript/"
category: "Next.js"
tags:
  - "next"
  - "typescript"
---


Next.js에서 TypeScript을 사용하는 법을 알아봅니다.
[Next 공식 블로그](https://nextjs.org/blog/next-9)를 참고했습니다.
여기에서 말하기로는 Next.js(9버전)을 설치하고 확장자를 js에서 tsx로 바꾸면 터미널에 알아서 지시가 나올거라고 하네요.

## Next.js 설치
```
npx create-next-app my-app
```

## js -> tsx 로 바꿔보기
초기 세팅 구조를 보니 아래와 같이 js 파일이 두 개라 확장자를 js에서 tsx로 바꿨습니다.

```
- /components/nav.js
- /pages/index.js
```

그리고 실행
```
yarn dev
```

네! 아래와 같이 뭔가를 설치하라고 안내가 잘 나옵니다. 하라는대로 합니다.
> Please install typescript, @types/react, and @types/node by running:

## typescript 설치
```
yarn add --dev typescript @types/react @types/node
```

설치 후 다시 실행
```
yarn dev
```

오!! 잘 되나봅니다. `tsconfig.json` 파일도 생성되었고, 실행했더니 아래와 같이 에러도 잘 뜨네요.
![](/media/190814-1.png)

이제 에러나는 부분을 TypeScript에 맞게 수정해봅시다.

links 의 type 정의가 어디되어있는 걸까요? link에는 key라는 프로퍼티가 없는데 추가했다고 에러가 나고 있습니다.
뒤의 map은 없애주고 render에서 map 돌릴 때 key로 사용하려고 했던 key 프로퍼티대신에
일단 unique해보이는 label 프로퍼티를 key로 사용하였습니다.

```jsx
const links = [
  { href: 'https://zeit.co/now', label: 'ZEIT' },
  { href: 'https://github.com/zeit/next.js', label: 'GitHub' }
].map(link => {
  link.key = `nav-link-${link.href}-${link.label}`
  return link
})
```

넹 에러가 없이 화면이 잘 나옵니다.

끝! 이제 TypeScript와 함께 열심히 개발 시작
