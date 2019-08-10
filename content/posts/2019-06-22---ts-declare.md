---
title: TypeScript - 변수선언
date: "2019-06-22T11:41:32.169Z"
template: "post"
draft: false
slug: "/posts/ts-declare/"
category: "TypeScript"
description: "TypeScript의 변수 선언에 대해 공부합니다. 이 부분은 var와 ES6의 let/const의 명확한 차이, Scope, Closure, Hoisting, ES6의 Destructuring 개념을 잘 알고 있다면, JavaScript와 다를 것이 거의 없으므로 그냥 넘어가도 좋습니다."
tags:
  - "typescript"
---

이번에 공부할 내용은 [TypeScript - 변수선언](https://www.typescriptlang.org/docs/handbook/variable-declarations.html)이다.
이 부분은 var와 ES6의 let/const의 명확한 차이, Scope, Closure, Hoisting, ES6의 Destructuring 개념을 잘 알고 있다면, JavaScript와 다를 것이 거의 없으므로 그냥 넘어가도 좋다.

혹시 잘 모른다면 아래를 참고해서 충분히 공부하자.

- [exploringjs - 변수와 스콥](https://exploringjs.com/es6/ch_variables.html)
- [JavaScript.info - 변수](https://javascript.info/variables)
- [JavaScript.info - 클로저](https://javascript.info/closure)
- [exploringjs - destructuring](https://exploringjs.com/es6/ch_destructuring.html)
- [JavaScript.info - destructuring 과제들](https://javascript.info/destructuring-assignment)

#### let 선언과 block scoping
JavaScript와 결과값이 다른 코드가 있어서 가져왔다.

```ts
function foo() {
    return a;
}

foo();

let a;
```
let 키워드는 hoisting이 되지 않으므로 위의 코드가 .js 로 저장됐다면 runtime 시에 `Uncaught ReferenceError: Cannot access 'a' before initialization` 에러가 뜬다.
그러나 .ts로 저장하고 컴파일된 코드를 보면 `let a;`가 `var a;`로 되어있다. 그래서 위의 코드는 에러가 나지 않으며, `변수 a`가 hoisting 되어 `foo()`는 정상적으로 호출이 되고 `undefined`를 return한다.

