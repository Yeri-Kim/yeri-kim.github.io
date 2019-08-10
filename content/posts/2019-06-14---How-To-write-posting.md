---
title: Gatsby 블로그 글 작성하기
date: "2019-06-14T23:41:32.169Z"
template: "post"
draft: false
slug: "/posts/how-to-wirte-gatsby/"
category: "gatsby"
description: "Gatsby 블로그는 어떻게 글을 쓰면 되는지, 어떻게 올릴 수 있는지 알아봅시다."
tags:
  - "gatsby"
---

정적 블로그는 네이버처럼 글쓰는 에디터가 따로 있고, 저장을 하면 글 내용이 어딘가에 보관되는 그런 방식이 아닙니다.
직접 markdown 파일에 내용을 작성하고 파일을 그대로 서버에 올립니다.

소스코드 중에 블로그 글을 작성해야 하는 위치는 아래와 같습니다. `root/content` 폴더를 봐주세요.

```
content
  |
   - pages : about, contact 페이지 메뉴
   - posts : 실제 쓰는 포스팅들
```


posts 디렉토리 밑에 블로그 글을 하나씩 생성하면 됩니다. 파일 확장자는 꼭 `.md`여야 합니다.

파일명은 "`날짜---파일이름.md`" 으로 하시면 됩니다. 파일명에 대한 강력한 규칙이 따로 있는 것 같지는 않습니다.
대신에 각 파일의 내용은 중요합니다.

### 포스팅 작성하기
파일 내용을 한 번 보겠습니다.

```
---
title: Gatsby 블로그 글 작성하기
date: "2019-06-14T23:41:32.169Z"
template: "post"
draft: false
slug: "/posts/how-to-wirte-gatsby/"
category: "gatsby"
description: "Gatsby 블로그는 어떻게 글을 쓰면 되는지, 어떻게 올릴 수 있는지 알아봅시다."
---
```
- title: 블로그 글 목록에서 보여지는 제목입니다.
- date: 포스팅에 보여지는 날짜입니다. 실제 배포된 날짜랑 다르고, 여기에 적혀진 날짜가 그대로 블로그 쓴 날짜로 올라옵니다.
- template: 블로그 글 디자인(템플릿)을 사용하겠다는 의미입니다.
- slug: 해당 포스팅 url로 사용할 주소를 작성합니다. posts는 꼭 붙여주세요.
- category: 해당 포스팅을 넣을 카테고리를 작성합니다. 앞으로 몇 개의 카테고리를 정해서 그 중에 하나 정해서 작성하면 됩니다. `/category/gatsby/` path로 접속 가능합니다.
제 블로그에서 특정 카테고리가 모아있는 주소는 `http://yeri-kim.github.io/category/gatsby/` 입니다.


그리고 이렇게 작성한 설정 내용 밑에서부터 글을 작성하면 됩니다. markdown 표기법에 익숙해져야 합니다.
[여기에서](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) 참고하고 공부해보세요.

지금 이 글의 원래 모습은 아래와 같습니다.
```
---
title: Gatsby 블로그 글 작성하기
date: "2019-06-14T23:41:32.169Z"
template: "post"
draft: false
slug: "/posts/how-to-wirte-gatsby/"
category: "gatsby"
description: "Gatsby 블로그는 어떻게 글을 쓰면 되는지, 어떻게 올릴 수 있는지 알아봅시다."
---

정적 블로그는 네이버처럼 글쓰는 에디터가 따로 있고, 저장을 하면 글 내용이 어딘가에 보관되는 그런 방식이 아닙니다.
직접 markdown 파일에 내용을 작성하고 파일을 그대로 서버에 올립니다.

소스코드 중에 블로그 글을 작성해야 하는 위치는 아래와 같습니다. `root/content` 폴더를 봐주세요.

posts 디렉토리 밑에 블로그 글을 하나씩 생성하면 됩니다. 파일 확장자는 꼭 `.md`여야 합니다.

파일명은 "`날짜---파일이름.md`" 으로 하시면 됩니다. 파일명에 대한 강력한 규칙이 따로 있는 것 같지는 않습니다.
대신에 각 파일의 내용은 중요합니다.

### 포스팅 작성하기
파일 내용을 한 번 보겠습니다.

- title: 블로그 글 목록에서 보여지는 제목입니다.
- date: 포스팅에 보여지는 날짜입니다. 실제 배포된 날짜랑 다르고, 여기에 적혀진 날짜가 그대로 블로그 쓴 날짜로 올라옵니다.
- template: 블로그 글 디자인(템플릿)을 사용하겠다는 의미입니다.
- slug: 해당 포스팅 url로 사용할 주소를 작성합니다. posts는 꼭 붙여주세요.
- category: 해당 포스팅을 넣을 카테고리를 작성합니다. 앞으로 몇 개의 카테고리를 정해서 그 중에 하나 정해서 작성하면 됩니다. `/category/gatsby/` path로 접속 가능합니다.
제 블로그에서 특정 카테고리가 모아있는 주소는 `http://yeri-kim.github.io/category/gatsby/` 입니다.


그리고 이렇게 작성한 설정 내용 밑에서부터 글을 작성하면 됩니다. markdown 표기법에 익숙해져야 합니다.
[여기에서](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) 참고하고 공부해보세요.
```

### 포스팅 로컬환경에서 확인하기
내가 쓴 글이 잘 나오는지 확인해야겠죠?
```
# yarn을 사용하는 경우
yarn develop
```
```
# npm을 사용하는 경우
npm run develop
```
```
# gatsby 커맨드를 사용하셔도 됩니다.
gatsby develop
```

그리고 `localhost:8000` 에서 확인해봅시다.
포스팅 글인 `md` 파일을 수정하면 실시간 reload가 되어 수정하는 중에 화면을 확인하기가 쉽습니다.

### 포스팅 올리기

글을 잘 작성했다면 바로 배포를 해도 좋습니다. `yarn deploy` 로 배포를 하면 자동으로 소스코드가 master 브랜치에 올라갑니다.
그런데 배포를 할 때 build 과정을 거치게 되는데 저희가 볼 필요가 없는 파일들이 아주 많이 생성됩니다.

![](/media/190614-3.png)
master 브랜치에는 이렇게 빌드된 많은 파일들을 올리고, develop이라는 브랜치를 새로 만들어서 여기에는 블로그 글만 올리겠습니다.

브랜치를 생성합니다.
```
git branch develop
```

develop 브랜치로 넘어갑니다.
```
git checkout develop
```

현재 수정된 파일을 확인합니다.
```
git status
```
저는 두 개의 파일이 추가되었고, 두 개의 파일이 수정되었네요.
![](/media/190614-4.png)

수정된 소스코드를 git에 올리도록 하겠습니다. 브랜치는 develop으로 하겠습니다.
```
git add .
git commit -m "개츠비 블로그 작성 법 포스팅"
git push origin develop
```
여기까지 수정/추가된 파일을 git에 올린 것입니다. 이제는 블로그 사이트에 배포하도록 하겠습니다. 아래에서 본인이 사용하는 package manager로 해주세요.

```
yarn deploy
```
```
npm deploy
```
네! 이제 포스팅이 올라갔는지 확인해주세요.
