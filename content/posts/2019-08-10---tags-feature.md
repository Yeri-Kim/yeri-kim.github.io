---
title: "gatsby 블로그에 태그목록 추가하기"
date: "2019-08-10T06:43:32.169Z"
template: "post"
draft: false
slug: "/posts/blog-tags/"
category: "gatsby"
description: "gatsby로 만든 블로그에 태그목록을 추가하여 포스팅을 쉽게 찾을 수 있도록 하겠습니다."
tags:
  - "gatsby"
  - "graqhql"
  - "css-module"
  - "sass"
  - "css"
---

원래 gatsby 블로그에 검색기능을 추가하려고 했지만, 당장 구현할 수 없음을 깨닫고 홈 화면에 태그 목록을 추가해보려고 합니다.
[검색기능을 추가하려고 시도했던 포스팅](https://yeri-kim.github.io/posts/blog-search/)을 참고해주세요.

## 태그 관련 컴포넌트
태그 관련한 기능은 제가 선택한 gatsby theme에 잘 구현되어 있습니다.
gatsby theme 코드 분석은 gatsby를 공부하기에도 좋고, react를 공부하기에도 좋습니다!
태그 관련 파일 위치를 한 번 볼까요?

1. **특정 태그의 포스팅 목록 페이지**:  `/src/templates/tags-template.js`
2. **태그 목록 페이지**:  `/src/templates/tags-list-template.js`
3. **포스팅의 하단 태그 목록**: `/src/components/Post/Tags/Tags.js`

왼쪽 사이드 메뉴 하단에 태그목록을 보여주는 컴포넌트를 직접 개발하려고 했는데,
이미 컴포넌트가 잘 만들어져있기에 갖다 쓰겠습니다 :)

## 왼쪽 사이드메뉴 하단에 태그 추가하기
Tags 컴포넌트를 쓰면 딱일 것 같네요!
홈페이지의 왼쪽 사이드 영역 컴포넌트를 찾아서 Tags 컴포넌트를 추가해보겠습니다.

구조가 참 잘 짜여있나봐요! 바로 찾았습니다.
- `/src/components/Sidebar/Sidebar.js`
<div>
<img style="display: inline-block;" src="/media/190810-4.png" width="200">
<img style="display: inline-block;" src="/media/190810-5.png" width="300">
</div>

Sidebar의 Copyright 바로 밑에 Tags 컴포넌트를 추가하면 될 것 같습니다.

### Tags 컴포넌트
Tags 컴포넌트를 바로 갖다 쓰기 전에 필요한 props와 데이터 구조를 파악해야합니다.

```jsx
// @flow
import React from 'react';
import { Link } from 'gatsby';
import styles from './Tags.module.scss';

type Props = {
  tags: string[],
  tagSlugs: string[]
};

const Tags = ({ tags, tagSlugs }: Props) => (
  <div className={styles['tags']}>
    <ul className={styles['tags__list']}>
      {tagSlugs && tagSlugs.map((slug, i) => (
        <li className={styles['tags__list-item']} key={tags[i]}>
          <Link to={slug} className={styles['tags__list-item-link']}>
            {tags[i]}
          </Link>
        </li>
      ))}
    </ul>
  </div>
);

export default Tags;

```

tag명으로 구성된 배열인 `tags`와 tag url로 구성된 배열인 `tagSlugs`가 필요해보입니다.

### Sidebar 컴포넌트
Sidebar 컴포넌트에 Tags 컴포넌트를 추가하고, 데이터를 잘 정제하여 보내줍니다.
tag 목록을 가져오기 위해 useTagsList() 을 사용하였습니다.

tag 목록을 가져오는 쿼리가 궁금하다면 `/src/hooks/use-tags-list.js` 에서 보시고, http://localhost:8000/__graphql 에서 직접 쿼리해보세요!
```jsx
// @flow
import React from 'react';
import Author from './Author';
import Contacts from './Contacts';
import Copyright from './Copyright';
import Menu from './Menu';
import styles from './Sidebar.module.scss';
import { useSiteMetadata, useTagsList } from '../../hooks';
import Tags from '../Post/Tags';

type Props = {
  isIndex?: boolean
};

const Sidebar = ({ isIndex }: Props) => {
  const { author, copyright, menu } = useSiteMetadata();
  const tags = useTagsList();

  return (
    <div className={styles['sidebar']}>
      <div className={styles['sidebar__inner']}>
        <Author author={author} isIndex={isIndex} />
        <Menu menu={menu} />
        <Contacts contacts={author.contacts} />
        <Copyright copyright={copyright} />
        <Tags
          tags={tags.map((tag) => tag.fieldValue)}
          tagSlugs={tags.map((tag) => `/tag/${tag.fieldValue}`)}
        />
      </div>
    </div>
  );
};

export default Sidebar;
```

오! 잘 추가가 되었습니다.
<img style="display: inline-block;" src="/media/190810-7.png" width="300">

그런데 문제가 몇 개 있어 보입니다.
1. 그 동안 포스팅에 category만 추가했지, tag는 몇 개 추가한게 없어서 태그 목록이 많지 않습니다. => 해결: 포스팅 하나하나 수작업으로 추가한다.
2. 태그 대/소문자를 구분하므로 통일시켜야겠네요. => 해결: 쿼리로 해보려고 했으나 실패. 수작업으로 대/소문자 통일한다.
3. 사이드바에서 태그들이 커보여서 css를 조금 수정하겠습니다. => 해결: 아래 내용 참고하여 수정!

참고로 frontmatter에서 태그는 아래와 같이 추가합니다.
```
---
title: "gatsby 블로그에 태그목록 추가하기"
date: "2019-08-10T06:43:32.169Z"
template: "post"
draft: false
slug: "/posts/blog-tags/"
category: "gatsby"
description: "gatsby로 만든 블로그에 태그목록을 추가하여 포스팅을 쉽게 찾을 수 있도록 하겠습니다."
tags:
  - "gatsby"
  - "graqhql"
  - "tag"
---
```

### css-module
이제는 css를 수정하겠습니다. sidebar 내부에 있는 tag만 작게 보일 수 있도록 스타일을 추가할 것입니다.
(포스팅 페이지 하단에 있는 tag 스타일은 영향이 가지 않도록)

gatsby에서 css를 수정하려면 sass와 css-module을 알고 있어야합니다.

[css-module](https://www.gatsbyjs.org/docs/css-modules/) 는 css를 모듈화 해주는 것입니다.
예를 들어, cra에서 컴포넌트별로 css파일을 분리해서 작성하지만 빌드하면 하나의 css 파일로 합쳐집니다.
그래서 같은 클래스명을 사용하면 충돌나기 십상이라 클래스 작명에 신경써야 하고 css selector을 주의하여 써야합니다.

css module을 사용하면 css가 해당 component에서만 적용되도록 css 명이 바뀌어서 scope가 한정될 수 있도록 해줍니다.

그렇기 때문에 지금과 같이 .sidebar 내에 있는 .tags인 경우에만 스타일을 바꾸고 싶을 때,
평소처럼 sidebar selector를 .tags 위에 추가해준다고 해서 적용되지 않습니다.
```scss
//예를 들어 이렇게
.sidebar {
  .tags {
    //태그 박스 작게
  }
}
```

### css 수정
css-module 쓰는 건 알겠고, 이럴 경우 css를 sidebar 컴포넌트 쪽 css파일에 추가해야할까요? 아님 tag 쪽에 추가해야할까요?

무조건 sidebar 밑에 있는 tag에만 스타일을 적용시킬 것이라 혼란이 오겠지만, 저라면 tag 컴포넌트에 있는 css에 추가하겠습니다.
tag component를 수정하다가 스타일도 수정하게 될 경우, 바로 옆에 있는 파일을 수정하면 유지/보수에 편리하기 때문입니다.

하지만 무조건 sidebar 내에서만 tags의 스타일을 변경해줄 것이므로 sidebar쪽에 추가해도 될 것 같다는 생각입니다.
좀 더 공부하고 명확한 기준이 생기면 포스팅을 수정하도록 하겠습니다!

제가 구현한 방법은
Sidebar에서 Tags 컴포넌트를 사용할 때, Sidebar 내부인지 아닌지 알려주는 inSidebar라는 props를 넘겼습니다.
```jsx
<Tags
  tags={tags.map((tag) => tag.fieldValue)}
  tagSlugs={tags.map((tag) => `/tag/${tag.fieldValue}`)}
  inSidebar
/>
```

Tags 컴포넌트에서는 inSidebar 여부에 따라 class를 추가해주었습니다.
classnames의 cx() 메서드를 사용하여 중복 class를 부여할 수 있습니다.
```jsx
// @flow
import React from 'react';
import { Link } from 'gatsby';
import cx from 'classnames';
import styles from './Tags.module.scss';

type Props = {
  tags: string[],
  tagSlugs: string[],
  inSidebar?: boolean
};

const Tags = ({ tags, tagSlugs, inSidebar }: Props) => (
  <div className={styles['tags']}>
    <ul className={styles['tags__list']}>
      {tagSlugs && tagSlugs.map((slug, i) => (
        <li
          className={cx(
            styles['tags__list-item'],
            inSidebar && styles['tags_insidebar__list-item']
          )}
          key={tags[i]}
        >
          <Link
            to={slug}
            className={cx(
              styles['tags__list-item-link'],
              inSidebar && styles['tags_insidebar__list-item-link']
            )}
          >
            {tags[i]}
          </Link>
        </li>
      ))}
    </ul>
  </div>
);

export default Tags;
```
코드가 지저분해진 것은 기분탓일까요?

만약 앞으로 Tags 컴포넌트가 쓰이는 곳이 많아지는데,
Sidebar에서만 다른 스타일을 주고 싶은 것이라면 비효율적인 코드같습니다.
좋은 아이디어 있다면 제보 부탁드립니다.

<img src="/media/190810-8.png" width="200" >

네!! 드디어 원하는대로 바꼈습니다. 이제 저는 모든 포스팅에 tag 추가하러 가야겠습니다.

이만 -










