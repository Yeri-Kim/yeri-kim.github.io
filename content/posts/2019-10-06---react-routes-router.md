---
title: 커리어 스킬 1부. 소프트웨어 개발자로 입문하기
date: "2019-10-04T04:46:32.169Z"
template: "post"
draft: true
slug: "/posts/career-skills-1/"
category: "잡담"
description: "완벽한 개발자 인생 로드맵 - 커리어 스킬(존 손메즈) 리뷰"
tags:
  - "wecode"
  - "위코드"
---

이것도 정리 https://medium.com/@jenniferdobak/react-router-vs-switch-components-2af3a9fc72e

Router

Router component is what makes the connection between browser location and the react application. It doesn't render anything visible on your page. It just exposes API to interact with location changes of the browser via React context. So any component down the tree can use this API to change their behavior based on location changes in the browser or change the browser location into what they want.

Router is the abstract for all other specific router components. Practically, we use a specific implementation of it like BrowserRouter, MemoryRouter, and HashRouter which use different methods to manage browser history. Also, Router is usually a top-level component in the component tree and use only one time in the entire application. All other react-router components should be descendants of Router as they can't function without the API which Router provides.

Route

Route is much simple to explain. It just renders some UI when a location matches the route’s path. So an application can have many Routes based on its layout complexity in different levels of the component tree. Also, Route has some additional props to configure how the match should happen. Route internally use API provided by Router to access the location and decide whether to render the given component or not.
