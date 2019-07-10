---
title: 함수는 first class object (1급 객체) 이다.
date: "2019-07-09T13:42:32.169Z"
template: "post"
draft: true
slug: "/posts/1th-class/"
category: "JavaScript"
---

함수가 first class object 인게 뭐가 그리 중요할까? 어쨋든 2-3년 쯤에 한참 유행했던 면접문제인듯하다.
꽤 오래된 JavaScript Pattern 책을 들춰보다가 잘 정리되어있길래 블로그에 작성하기로 했다.

이것은 자바스크립트를 독특한 언어로 만드는 것중 하나의 요인이다.
자바스크립트는 클래스가 없고, 함수는 first class object로 다양한 작업에 사용된다.
ES6에 소개된 Class는 사실상 진짜 Class가 아니라 함수이며,
마치 다른 언어의 Class처럼 사용할 수 있게 문법적으로 추가된 것 뿐이다. 이런 것을 Syntax Sugar라고 부르는 것 같다.

first class object(일급객체)는 다음과 같은 특징을 가지는 객체이다.
- 변수나 데이터 구조 안에 담을 수 있다.
- 인자로 전달할 수 있다.
- return 값으로 사용할 수 있다.
- 런타임에 생성할 수 있다.
- 할당에 사용된 이름과 관계없이 고유하게 식별할 수 있다.

앞에 3개는 이해하기가 쉬운데, 뒤의 2개는 정확히 감이 오지 않는다.
capturing과 memoization 얘기까지 나오던데 다시 공부해서 올려야 겠다...

참고로 first class object는 JavaScript에만 있는 개념은 아니다.
[first class function](https://en.wikipedia.org/wiki/First-class_function) 참고!
