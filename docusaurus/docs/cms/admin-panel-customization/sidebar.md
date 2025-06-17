---
title: 사이드바 커스터마이징
description: Strapi 관리자 패널의 사이드바를 커스터마이징하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 사이드바 커스터마이징
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 사이드바 커스터마이징

Strapi의 [관리자 패널](/cms/admin-panel-customization)에서 사이드바의 항목을 프로젝트에 맞게 커스터마이징할 수 있습니다.

## 사이드바 항목 커스터마이징

사이드바에 표시되는 항목을 변경하려면 `/src/admin/app.js` 또는 `/src/admin/app.ts` 파일에서 `config` 객체의 `menu` 속성을 사용하세요.

예시:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    menu: {
      // 예시: Content Manager 메뉴 숨기기
      hide: ['content-manager'],
      // 예시: Custom 메뉴 추가
      custom: [
        {
          label: '내 메뉴',
          icon: 'plug',
          to: '/my-custom-page',
        },
      ],
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
    menu: {
      // 예시: Content Manager 메뉴 숨기기
      hide: ['content-manager'],
      // 예시: Custom 메뉴 추가
      custom: [
        {
          label: '내 메뉴',
          icon: 'plug',
          to: '/my-custom-page',
        },
      ],
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::tip 참고

- `hide` 배열에는 숨기고 싶은 기본 메뉴의 이름을 추가하세요.
- `custom` 배열에는 직접 추가할 메뉴 항목을 객체 형태로 정의할 수 있습니다.
- 아이콘 이름은 Strapi에서 지원하는 아이콘 이름을 사용하세요.

::: 