---
title: "cors  크롬 띄우기"
date: "2019-08-14T10:43:32.169Z"
template: "post"
draft: false
slug: "/posts/chrome-cors/"
category: "JavaScript"
description: "cors extension이 도저히 먹질 않아서 찾아보게 되었습니다."
tags:
  - "cors"
---


```jsx
open -n -a /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --args --user-data-dir="/tmp/chrome_dev_test" --disable-web-security
```
