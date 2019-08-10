---
title: TypeScript 공부 시작 - Basic Type
date: "2019-06-19T11:41:32.169Z"
template: "post"
draft: false
slug: "/posts/start-typescript/"
category: "TypeScript"
description: "더 이상 거부할 수 없는 typescript.. 제대로 한 번 공부해보기로 했습니다. 첫 시간에서는 10여개의 ts data type을 알아봅니다."
tags:
  - "typescript"
---

JavaScript를 워낙 오래 써서 그런지 type이 없는 언어에 별로 불편함을 느낀적이 없었습니다.
3-4년 전부터 하도 타입스크립트~타입스크립트~ 해서 시작해볼까도 했지만, 필요성을 못 느껴서 딱히 관심을 두지 않았습니다.

그러다가 새로운 회사에서 타입스크립트로 만든 리액트 프로젝트를 유지보수해야할 일이 있어서 급히 공부를 시작했습니다.
막상 써보니 개발 생산성이 훨씬 높아졌습니다. props, state를 변경하거나 추가할 때마다, 강제로 에러를 내뿜으니 도저히 실수를 할 수가 없는 환경이었습니다.
그래서 타입스크립트를 제대로 배워보기로 했습니다. 그 동안에는 유지보수를 할 정도로만 알고 있었는데 이제 제대로 배워서 타입스크립트를 도입해보려고 합니다.


### TypeScript란?
타입스크립트는 프로그래밍 언어입니다. 언어이긴 언어인데, 자바스크립트에 type을 주입한 확장된 자바스크립트이며,
타입스크립트로 작성된 코드는 결국 컴파일과정을 거쳐 자바스크립트 코드로 변환되어 브라우저에 적용됩니다.

### TypeScript Handbook
TypeScript 좋은 강의 없나 보다가 egghead.io에서 하나 들었는데 너무 쉽고 짧아서 typescript docs를 찾았습니다.
하루에 하나씩 [TypeScript HandBook](https://www.typescriptlang.org/docs/handbook/basic-types.html)을 보려고 합니다.
오늘 차례는 Basic Type입니다.

아래의 내용은 [TypeScript Handbook - Basic Type](https://www.typescriptlang.org/docs/handbook/basic-types.html) 를 정리한 내용입니다.

### Basic Type
JavaScript 에서 사용하는 type하고 거의 같다고 보면 된다.

#### Boolean
```ts
let isDone: boolean = false;
```

#### Number
JavaScript와 마찬가지로 TypeScript 도 floating point 값이다.
TypeScript는 16진수, 10진수 뿐 아니라 ES6에 소개된 2진수 8진수도 지원한다.

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

#### String
", ' 둘다 되고 JavaScript의 String type과 같다.
```ts
let color: string = "blue";
color = 'red';

let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.

I'll be ${ age + 1 } years old next month.`;


```


#### Array
TypeScript에서 배열은 두 가지 방법으로 작성할 수 있다.

- 배열의 요소의 type 을 작성하고 []을 쓰는 방법
```ts
let list: number[] = [1, 2, 3];
```

- generic array type 인 Array<elemType> 을 쓰는 방법
```ts
let list: Array<number> = [1, 2, 3];
```

#### Tuple
파이썬에서 tuple을 쓴 적은 있지만, JavaScript에서는 쓴 적이 없었다. TypeScript Tuple을 보고 설마 es6에서 추가되었나 검색했는데 그렇지 않다.
TypeScript에서만 쓸 수 있는 type이다.

TypeScript의 tuple은 요소 타입을 먼저 선언해주기 때문에, 선언한 타입의 순서에 맞게 Tuple 요소를 추가해줘야 한다.

```ts
// tuple type 선언
let x: [string, number];
// 값 초기화
x = ["hello", 10]; // OK
// type 순서 달라서 초기화 에러
x = [10, "hello"]; // Error
```

아래의 값 초기화에 어떤 에러가 나오나 봤는데
```ts
x = ["hello", 333, "hi"];
```

이런 에러가 떴다.

Type '[string, number, string]' is not assignable to type '[string, number]'.
Types of property 'length' are incompatible.
Type '3' is not assignable to type '2'.



그리고 바로 다음 내용이 인덱스 넘어선 요소에 접근하려고 하면, union type이 사용된다는데 union type은 심화 내용이라고 한다.
그런데 나는 아래처럼 되지 않고 `Type '"world"' is not assignable to type 'undefined'.` 라는 에러가 났다.

```ts
x[3] = "world"; // OK, 'string' can be assigned to 'string | number'
console.log(x[5].toString()); // OK, 'string' and 'number' both have 'toString'
x[6] = true; // Error, 'boolean' isn't 'string | number'
```
다음 챕터를 더 공부하고 다시 봐야겠다...

#### Enum
Enum 또한 JavaScript에는 없는 타입이다. 그래도 백엔드에서 enum 타입으로 보낸 값을 여러 번 받아쓴적이 있어 친숙하다.

```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

enum의 시작 인덱스는 원래 0인데, 다음과 같이 지정해서 사용할 수도 있다. 그럼 1부터 시작한다. 참고로 아예 각각의 값에 numbering을 해도 된다.
```ts
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

enum의 좋은 점 중 하나는 enum 값을 숫자로 접근할 수 있다는 것이다.

```ts
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName); // Displays 'Green' as its value is 2 above
```



#### Any
Any 는 어떤 타입을 사용해야할지 확실히 모를 때 사용한다.
업무내용을 전부 파악하지 못한상태에서 유지보수를 맡았을 때, Any를 몇 개 쓰다가.. 왠지 구멍을 심어놓은 것 같아서 다시 코드를 전부 분석하고 확정 타입으로 많이 되돌려 놓았다.
TypeScript를 제대로 공부하지 않았던 시절임에도 Any는 왠지 쓰면 안 될 것 같았다.

웬만하면 user input 이나 3rd party에서 받는 값에서나 Any를 쓰도록 하자.

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

TypeScript는 컴파일시에 에러를 알리기 때문에, 아래처럼 확실하지 않는 값은 any 타입으로 편하게 쓰면 된다.
물론 아래의 코드는 runtime시에 에러가 난다. (notSure.ifItExists가 함수가 아니니깐)
```ts
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

배열 요소의 type이 확실하지 않을 때도 편하게 쓰자 ㅎㅎ
```ts
let list: any[] = [1, true, "free"];
list[1] = 100;
```


#### Void
void는 any랑 반대의 느낌인데, 즉, 아무 type도 없다는 뜻이다. return 하는게 없을 때 사용하면 된다.
```ts
function warnUser(): void {
    console.log("This is my warning message");
}
```


void type으로 선언하는 것은 `undefined` 나 `null`만 할당할 수 있기 때문에 별로 유용하지 않다.(굳이 type 안 정해줘도 된다는 소리)
```ts
let unusable: void = undefined;
```

#### Null, Undefined
TypeScript에서 Null, Undefined 도 하나의 type이다.
void와 같이 굳이 type 정해주지 않아도 된다.
```ts
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

#### Never
Never 타입은 실무에서 아직 써본적이 없다. 그런데 지금 보니 쓸만한 곳이 많았을 것 같다. 한 번 적용해봐야겠다.

never 타입은 절대 일어날 일이 없는 값의 타입이라고 한다. 예를들어 exception을 던지거나 return하지 않는 함수표현의 return type이다.

```ts
// error를 던지는 함수. never type이다.
function error(message: string): never {
    throw new Error(message);
}

// Inferred return type is never
function fail() {
    return error("Something failed");
}

// 이것 또한 끝(return)에 도달하지 않는 never type의 함수
function infiniteLoop(): never {
    while (true) {
    }
}
```

#### Object
별것 없다. 그런데 declare라는 키워드는 처음 봤는데? 다음 챕터에 나오겠지..
```ts
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error

```

#### Type assertions

type assertion이란, any라는 type으로 선언된 변수이긴 한데, 사용할 시점에 확실히 타입을 알았을 때 사용하는 것이다. 두 가지 방법이 있다고 한다.

-  "angle-bracket" 방식
```ts
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```
- as 방식
```ts
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

둘 중에 뭘 써도 상관 없다고 하는데 JSX에서는 as만 허용된다고 하니 앞으로 as만 애용해야겠다.
