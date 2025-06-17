---
title: 테마 커스터마이징
description: Strapi 관리자 패널의 테마(색상, 다크 모드 등)를 커스터마이징하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 테마 커스터마이징
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 테마 커스터마이징

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 기본적으로 라이트/다크 모드를 지원하며, 색상 팔레트 등 테마를 커스터마이징할 수 있습니다.

## 테마 옵션 설정하기

관리자 패널의 테마를 변경하려면 `/src/admin/app.js` 또는 `/src/admin/app.ts` 파일에서 `config` 객체의 `theme` 속성을 사용하세요.

예시:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    theme: {
      light: {
        colors: {
          primary100: '#f6ecfc',
          primary200: '#e0c1f4',
          primary500: '#ac73e6',
          primary600: '#9736e8',
          primary700: '#8312d1',
        },
      },
      dark: {
        colors: {
          primary100: '#f6ecfc',
          primary200: '#e0c1f4',
          primary500: '#ac73e6',
          primary600: '#9736e8',
          primary700: '#8312d1',
        },
      },
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
    theme: {
      light: {
        colors: {
          primary100: '#f6ecfc',
          primary200: '#e0c1f4',
          primary500: '#ac73e6',
          primary600: '#9736e8',
          primary700: '#8312d1',
        },
      },
      dark: {
        colors: {
          primary100: '#f6ecfc',
          primary200: '#e0c1f4',
          primary500: '#ac73e6',
          primary600: '#9736e8',
          primary700: '#8312d1',
        },
      },
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::tip 참고

- `light`와 `dark` 객체 내에서 색상 팔레트를 자유롭게 지정할 수 있습니다.
- 더 많은 테마 옵션은 [Strapi 공식 테마 문서](https://docs.strapi.io/dev-docs/admin-panel-customization#theme-customization)에서 확인하세요.

::: 