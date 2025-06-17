---
title: 로고 커스터마이징
description: Strapi 관리자 패널의 로고를 변경하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 로고 커스터마이징
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 로고 커스터마이징

Strapi의 [관리자 패널](/cms/admin-panel-customization)에서 기본적으로 표시되는 로고를 프로젝트에 맞게 변경할 수 있습니다.

## 로고 변경하기

관리자 패널의 로고를 변경하려면, `/src/admin/app.js` 또는 `/src/admin/app.ts` 파일에서 `config` 객체의 `head` 속성을 사용하세요.

예시:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    head: {
      favicon: '/path/to/your/favicon.ico',
      title: '내 프로젝트',
      // 로고 이미지를 커스터마이징하려면 아래와 같이 설정
      logo: '/path/to/your/logo.png',
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
      favicon: '/path/to/your/favicon.ico',
      title: '내 프로젝트',
      // 로고 이미지를 커스터마이징하려면 아래와 같이 설정
      logo: '/path/to/your/logo.png',
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::tip 참고

- 로고 이미지는 프로젝트의 `public` 폴더(예: `/public/logo.png`)에 위치해야 하며, 경로는 `/logo.png`와 같이 지정합니다.
- favicon 역시 동일하게 `public` 폴더에 위치시키고 경로를 지정하세요.

::: 