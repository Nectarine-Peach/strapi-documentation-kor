---
title: 타이틀 커스터마이징
description: Strapi 관리자 패널의 타이틀(브라우저 탭 제목 등)을 변경하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 타이틀 커스터마이징
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 타이틀 커스터마이징

Strapi의 [관리자 패널](/cms/admin-panel-customization)에서 브라우저 탭에 표시되는 타이틀을 프로젝트에 맞게 변경할 수 있습니다.

## 타이틀 변경하기

관리자 패널의 타이틀을 변경하려면 `/src/admin/app.js` 또는 `/src/admin/app.ts` 파일에서 `config` 객체의 `head` 속성을 사용하세요.

예시:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    head: {
      title: '내 프로젝트',
    },
  },
  bootstrap() {},
};
```

</TabItem>
<TabItem value="ts" label="TypeScript">

```js title="/src/admin/app.ts"
export default {
  config: {
    head: {
      title: '내 프로젝트',
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::tip 참고

- `title` 속성은 브라우저 탭에 표시되는 텍스트를 의미합니다.
- favicon, 로고 등도 동일한 방식으로 `head` 속성에서 지정할 수 있습니다.

::: 