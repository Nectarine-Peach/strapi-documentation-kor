---
title: 관리자 패널 번들러
description: Strapi 5에서 Vite와 webpack 번들러 설정 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 번들러
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
- webpack
- Vite
---

import FeedbackCallout from '/docs/snippets/backend-customization-feedback-cta.md'

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 Strapi 애플리케이션의 모든 기능과 설치된 플러그인을 포함하는 React 기반 싱글 페이지 애플리케이션입니다. Strapi 5에서는 2가지 번들러([Vite](#vite)(기본값), [webpack](#webpack))를 사용할 수 있으며, 둘 다 필요에 맞게 설정할 수 있습니다.

:::info
문서에서는 편의상 `strapi develop` 명령어를 언급하지만, 실제로는 사용하는 패키지 매니저에 따라 `yarn develop` 또는 `npm run develop`을 실행하게 됩니다.
:::

## Vite

Strapi 5에서는 <ExternalLink to="https://vitejs.dev/" text="Vite"/>가 관리자 패널 빌드의 기본 번들러입니다. 따라서 `strapi develop` 명령어를 실행하면 기본적으로 Vite가 사용됩니다.

Vite 설정을 확장하려면 `/src/admin/vite.config` 파일 내에서 설정을 확장하는 함수를 정의하세요:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/vite.config.js"
const { mergeConfig } = require("vite");

module.exports = (config) => {
  // 중요: 수정된 config를 반드시 반환해야 합니다.
  return mergeConfig(config, {
    resolve: {
      alias: {
        "@": "/src",
      },
    },
  });
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```ts title="/src/admin/vite.config.ts"
import { mergeConfig } from "vite";

export default (config) => {
  // 중요: 수정된 config를 반드시 반환해야 합니다.
  return mergeConfig(config, {
    resolve: {
      alias: {
        "@": "/src",
      },
    },
  });
};
```

</TabItem>
</Tabs>

## Webpack

Strapi 5의 기본 번들러는 Vite입니다. <ExternalLink to="https://webpack.js.org/" text="webpack"/>을 번들러로 사용하려면 `strapi develop` 명령어에 옵션을 추가해야 합니다:

```bash
strapi develop --bundler=webpack
```

:::prerequisites
webpack을 커스터마이징하기 전에 기본 `webpack.config.example.js` 파일의 이름을 `webpack.config.`로 변경하세요.
:::

webpack v5 설정을 확장하려면 `/src/admin/webpack.config.` 파일 내에서 설정을 확장하는 함수를 정의하세요:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/webpack.config.js"
module.exports = (config, webpack) => {
  // 참고: 위에서 webpack을 제공하므로 별도로 require할 필요 없음

  // webpack config 커스터마이징
  config.plugins.push(new webpack.IgnorePlugin(/\/__tests__\//));

  // 중요: 수정된 config를 반환해야 합니다.
  return config;
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```ts title="/src/admin/webpack.config.ts"
export default (config, webpack) => {
  // 참고: 위에서 webpack을 제공하므로 별도로 require할 필요 없음

  // webpack config 커스터마이징
  config.plugins.push(new webpack.IgnorePlugin(/\/__tests__\//));

  // 중요: 수정된 config를 반환해야 합니다.
  return config;
};
```

</TabItem>
</Tabs>

