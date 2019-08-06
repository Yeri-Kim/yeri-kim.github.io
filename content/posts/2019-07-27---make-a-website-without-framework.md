---
title: "framework 없이 웹사이트 만들기"
date: "2019-07-27T13:42:32.169Z"
template: "post"
draft: true
slug: "/posts/make-a-website-without-framework/"
category: "JavaScript"
description: React, Vue, jQuery 등의 framework/library 없이, 그러나 webpack 등의 개발환경 설정으로 웹사이트를 만들어봅니다.
---

webpack은 bundler이다.


## dependency 설치

1. webpack을 사용하기 위해 webpack과, webpack 명령어를 사용하기 위해 webpack-cli를 설치한다.
```
yarn add -D webpack webpack-cli  webpack-dev-server
```
webpack-dev-server 는 개발 모드일 때 live reload와 dev server를 사용할 수 있기 때문이다.


2. ES6 이상을 여러 브라우저에서 사용할 수 있도록 babel을 설치한다. babel을 사용하기 위해서는 아래 3개를 세트로 설치해야한다.
```
yarn add -D @babel/core @babel/preset-env babel-loader
```

3. sass 사용을 위해
```
yarn add -D node-sass
```

entry point default path가 src/index.js 이다.

default output path는 dist/main.js이다.

webpack에게 mode를 알려주는 것이 좋다.
development인지, production인지.



webpack이 js를 bundle한 결과 파일을 html에서 인지하지 못한다.
config에 추가해야한다.

### plugin 역할

html-webpack-plugin
배포용 폴더에 html을 옮겨줘야 하기 때문에 필요하다.

html-loader
HTML file 압축화
한줄로 나온다.

file-loader
이미지 등의 파일을 번들 디렉토리에 보내준다.
default 설정은 이미지 파일 이름이 바뀌는 것인데, 파일 내용에 기반한 hash 값이 파일 이름이 된다.
아마도 이미지가 달라졌는데 이름이 그대로이면 브라우저 캐쉬 때문에 바뀐 파일을 감지하지 못해서 그런것 같지 싶다.
그리고 bundle output directory 바로 밑에 생성된다.
```json
{
    test: /\.(png|jpe?g|gif|svg)$/i,
    use: [
      {
        loader: 'file-loader',
        options: {
          name: '[name].[ext]'
        }
      }
    ]
}
```

css-loader
The css-loader interprets @import and url() like import/require() and will resolve them.


sass-loader
sass -> css

mini-css-extract-plugin
css 압축

#### reference
- https://blog.bitsrc.io/build-a-website-with-modern-tooling-and-no-frameworks-a33e65099f9
