---
title: Webpack 커스터마이징
description: Strapi 관리자 패널의 Webpack 설정을 커스터마이징하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: Webpack 커스터마이징
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# Webpack 커스터마이징

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 내부적으로 Webpack을 사용하여 번들링합니다. 프로젝트에 맞게 Webpack 설정을 확장하거나 오버라이드할 수 있습니다.

## Webpack 설정 확장하기

Webpack 설정을 확장하려면 `/src/admin/app.js` 또는 `/src/admin/app.ts` 파일에서 `config` 객체의 `webpack` 속성을 사용하세요.

예시:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    webpack: (config, webpack) => {
      // 여기서 config를 수정할 수 있습니다.
      config.resolve.alias['@custom'] = '/my/custom/path';
      return config;
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
    webpack: (config, webpack) => {
      // 여기서 config를 수정할 수 있습니다.
      config.resolve.alias['@custom'] = '/my/custom/path';
      return config;
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::tip 참고

- `config`는 기존 Webpack 설정 객체입니다. 필요에 따라 자유롭게 수정할 수 있습니다.
- 더 자세한 Webpack 설정 방법은 [Webpack 공식 문서](https://webpack.js.org/configuration/)를 참고하세요.

::: 