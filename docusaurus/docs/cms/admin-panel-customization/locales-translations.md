---
title: 로케일 & 번역
description: Strapi 관리자 패널에서 로케일을 추가하고 번역을 확장하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 로케일 & 번역
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 로케일 & 번역

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 항상 영어 번역을 기본으로 제공하지만, 추가 언어를 표시할 수 있습니다. 또한 인터페이스에 표시되는 모든 텍스트를 오버라이드할 수도 있습니다.
이 문서에서는 프로젝트 코드베이스에서 직접 로케일을 정의하고, Strapi 또는 플러그인 번역을 확장하는 방법을 안내합니다.

## 로케일 정의하기

관리자 패널에서 사용할 수 있는 로케일 목록을 업데이트하려면 `config.locales` 배열을 사용하세요:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/admin/app.js"
export default {
  config: {
    locales: ["ru", "zh"],
  },
  bootstrap() {},
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```jsx title="/src/admin/app.ts"
export default {
  config: {
    locales: ["ru", "zh"],
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::note 참고

- `en` 로케일은 빌드에서 제거할 수 없습니다. 이는 기본값이자, 번역이 없는 경우 fallback으로 사용됩니다.
- 사용 가능한 전체 로케일 목록은 <ExternalLink to="https://github.com/strapi/strapi/blob/v4.0.0/packages/plugins/i18n/server/constants/iso-locales.json" text="Strapi의 Github 저장소"/>에서 확인할 수 있습니다.

:::

## 번역 확장하기

번역 키/값 쌍은 `@strapi/admin/admin/src/translations/[언어명].json` 파일에 선언되어 있습니다.

이 키들은 `config.translations` 키를 통해 확장할 수 있습니다:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    locales: ["fr"],
    translations: {
      fr: {
        "Auth.form.email.label": "test",
        Users: "Utilisateurs",
        City: "CITY (FRENCH)",
        // Content Manager 테이블의 라벨 커스터마이징
        Id: "ID french",
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
    locales: ["fr"],
    translations: {
      fr: {
        "Auth.form.email.label": "test",
        Users: "Utilisateurs",
        City: "CITY (FRENCH)",
        // Content Manager 테이블의 라벨 커스터마이징
        Id: "ID french",
      },
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

플러그인 번역 키/값 쌍은 각 플러그인의 `/admin/src/translations/[언어명].json` 파일에 별도로 선언되어 있습니다. 이 키/값 쌍도 `config.translations` 키에서 플러그인 이름을 접두사로 붙여 확장할 수 있습니다(예: `[plugin name].[key]: 'value'`). 아래 예시를 참고하세요:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/admin/app.js"
export default {
  config: {
    locales: ["fr"],
    translations: {
      fr: {
        "Auth.form.email.label": "test",
        // 플러그인 번역 키/값 쌍 확장 예시 (플러그인 이름 접두사 사용)
        // 아래는 "content-type-builder" 플러그인의 "plugin.name" 키를 번역
        "content-type-builder.plugin.name": "Constructeur de Type-Contenu",
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
    locales: ["fr"],
    translations: {
      fr: {
        "Auth.form.email.label": "test",
        // 플러그인 번역 키/값 쌍 확장 예시 (플러그인 이름 접두사 사용)
        // 아래는 "content-type-builder" 플러그인의 "plugin.name" 키를 번역
        "content-type-builder.plugin.name": "Constructeur de Type-Contenu",
      },
    },
  },
  bootstrap() {},
};
```

</TabItem>
</Tabs>

추가 번역 파일이 필요하다면 `/src/admin/extensions/translations` 폴더에 파일을 추가하세요.
