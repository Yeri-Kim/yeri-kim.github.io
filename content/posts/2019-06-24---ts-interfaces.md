---
title: TypeScript - Interfaces
date: "2019-06-24T13:42:32.169Z"
template: "post"
draft: false
slug: "/posts/ts-interfaces/"
category: "TypeScript"
description: "TypeScript interface 공부 차례!"
tags:
  - "interface"
  - "typescript"
---

[TypeScript - Interfaces](https://www.typescriptlang.org/docs/handbook/interfaces.html)를 공부한 내용입니다.

#### Our First Interface
interface 작동 방식을 보자. 일단 아래의 예제는 printLabel 함수가 하나의 argument를 받는데, 그 인자 타입은 객체이며, label라는 프로퍼티는 string 형이여야 한다.
```ts
function printLabel(labeledObj: { label: string }) {
    console.log(labeledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```
아래도 같은 코드인데 이번엔 interface로 type을 checking하는 방식이다. 위도 아래도 labeledObj에 프로퍼티가 더 있지만, type 선언한 것만 체크하고 만다.
아무래도 아래 코드가 더 깔끔하고 interface를 재사용할 수도 있으니 interface를 사용하는 것이 좋겠다.
```ts
interface LabeledValue {
    label: string;
}

function printLabel(labeledObj: LabeledValue) {
    console.log(labeledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

#### Optional Properties
type checking을 하고 싶긴 한데, 프로퍼티가 있을 수도 있고, 없을 수도 있는 optional한 경우에는 ? 를 붙여주면 된다. 아래의 경우 인자 config에는, color, width라는 프로퍼티가 있을 수도, 없을 수도 있다.
```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
    let newSquare = {color: "white", area: 100};
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}

let mySquare = createSquare({color: "black"});
```

#### Readonly properties
최초에 객체 literal로 값을 할당하고, 프로퍼티를 수정하고 싶지 않다면 readonly 를 붙여주자. react props에 써도 될까? 아직 모르겠다.
일단 유지보수 하고 있는 소스코드에서는 readonly 사용한 것을 본적이 없다. 조만간 알게 되겠지 ㅎㅎ
```ts
interface Point {
    readonly x: number;
    readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```
배열을 수정하고 싶지 않을 때는 ReadonlyArray를 사용하면 된다.
```ts
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```
ReadonlyArray 로 생성한 배열을 다시 배열에 할당하는 것도 안되는데,  type assertion 으로 아래와 같이 할당하면 된다.
```ts
a = ro as number[];
```

어제 공부한 type assertion인데, 어색해서 그런지 뭐더라? 했다. 다시 내용을 가져왔다.

> type assertion이란, any type으로 선언된 변수이긴 한데, 사용할 시점에 확실히 타입을 알았을 때 사용하는 것이다. 아래는 as 방식을 사용했다.
```ts
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

#### readonly vs const
const도 변하면 안 되는 값을 선언할 때 사용하는데 언제 뭐를 써야 할까? 간단하다. 변수는 const를 사용하면 되고, property는 readonly를 사용하면 된다.

#### Excess Property Checks
말그대로 초과 프로퍼티 체크이다. 넣으면 안 될 프로퍼티를 넣었을 때 에러가 난다. 그런데 아래 코드에서 에러가 나는 것을 보고 1분 당황했다.
분명히 interface 첫 예제에서는 type을 선언한 프로퍼티만 검사했기 때문에, 다른 프로퍼티가 있어도 에러가 없었다.
```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: "red", width: 100 });
```
알고보니 객체 리터럴을 직접 전달해줄때만 Excess Property Checks 체크를 한다. 이해할 수 없는 동작원리지만, 컴파일할 때 체크하므로 그러려니 해본다.

#### Function Types
이해필요, wip..

#### Indexable Types
공부 더 필요...


#### Class Types
interface는 C#이나 Java에서 class를 정의할 때 자주 쓰는데, TypeScript에서도 비슷하게 가능하다.

```ts
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date): void;
}

class Clock implements ClockInterface {
    currentTime: Date = new Date();
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```
이 아래부터 다시 공부 필요 wip

한번에 이해가 안되네..

#### Extending Interfaces
class 상속하듯이 interface도 상속할 수 있다.
```ts
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```

여러개 상속도 가능하다.
```
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

#### Hybrid Types
여기도 다시다시..
