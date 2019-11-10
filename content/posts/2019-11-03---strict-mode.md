---
title: use strict
date: "2019-11-03T04:46:32.169Z"
template: "post"
draft: false
slug: "/posts/strict-mode/"
category: "JavaScript"
description: "JavaScript에서 strict mode란 무엇일까?"
tags:
  - "java-script"
---

#### JavaScript의 유연함
JavaScript의 장점이자 단점 중 하나는 유연하다는 것이다.
Vanilla JavaScript(= js without any additional libraries)에서 변수를 선언하지 않고 사용해도 에러를 일으키지 않으며, 객체에 중복된 프로퍼티명을 작성해도 문제가 없다.

그래서 TypeScript나 react의 PropTypes이 나오고 큰 인기를 끌었을 것이다. 하지만 이 전에
JavaScript에는 strict mode 라는 것이 있다.

#### strict mode
strict mode는 말그대로 js 파일에서 '엄격한 모드'로 JavaScript 코드를 실행하겠다는 것이다.
잠재적인 문제를 줄이고, 안전한 코드를 작성할 수 있게 해준다.

strict mode를 활성화 하려면, 파일 상단, 또는 함수 바디의 시작부분에서 아래의 표현을 '홑따옴표' 또는 "쌍따옴표"로 작성해주면 된다.
하지만 특정 함수에서만 strict mode를 활성화 시키기 보다는, 코드 전체에 적용하기 위하여 항상 파일 상단에 작성하는 것을 권장한다.

```js
"use strict";
```

#### 에러 examples
strict mode로 변경했을 때 나타나는 에러 몇 개를 소개한다.
- 변수를 선언하지 않고 사용할 때

```js
// "use strict"; 주석을 제거하면 에러 발생!

a = 1;
```

- 8진수 문자열 불가능

```js
// "use strict"; 주석 제거하면 octal literal 에러

let a = 010;
console.log(a); // 10
```

- 중복 parameter명 불가능

```js
// "use strict"; 주석 제거하면 Duplicate parameter name 에러

function test(text, text) {
    console.log(text);
}

test(1, 2);
```
위의 예제들은 원래의 JavaScript에서 전혀 에러없이 돌아가는 코드였다는 소리다.

[위코드](https://wecode.co.kr) pre course의 enemy game을 구현할 때,
많은 wecoder 분들이 변수를 선언하지 않고 바로 사용 했었는데,
앞으로 `"use strict"`를 활성화 해야겠다.

#### strict mode 기본 탑재
`"use strict"`를 작성하지 않고도 기본으로 활성화되는 경우가 있다.

- es6의 class를 사용할 때. 즉, class 내부 코드는 모두 strict mode이다.
- module code일 때. 예를 들어 webpack이 내장되어있는 CRA. (하지만, strict mode는 browser에서 support 하는 것이므로 Node.js 경우는 제외)

#### 마치며
요즘은 CRA, webpack, es6 module 등을 사용하므로 기본 적으로 strict mode가 활성화 되어있다.
하지만 vanilla js 프로젝트를 할 경우, 잊지 말고 "use strict"를 사용하자.
