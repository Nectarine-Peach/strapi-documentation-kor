---
title: 로고
description: Strapi 관리자 패널에 표시되는 로고를 커스터마이징하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 로고
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 로고

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 로그인 화면과 메인 네비게이션에 브랜드 로고를 표시합니다. 이 이미지를 교체하면 인터페이스를 브랜드에 맞게 맞출 수 있습니다. 본 문서에서는 관리자 패널 설정 파일을 통해 두 개의 로고 파일을 오버라이드하는 방법을 안내합니다. UI에서 직접 업로드하고 싶다면 [로고 커스터마이징](/cms/features/admin-panel#customizing-the-logo) 문서를 참고하세요.

Strapi 관리자 패널은 2개의 위치에 각각 다른 키로 로고를 표시합니다:

| UI 내 위치 | 설정에서 변경할 키 |
| ---------------------- | --------------------------- |
| 로그인 페이지 | `config.auth.logo` |
| 메인 네비게이션 | `config.menu.logo` |

:::note
관리자 패널에서 직접 업로드한 로고가 설정 파일에서 지정한 로고보다 우선 적용됩니다.
:::

### 관리자 패널 내 로고 위치

<!--TODO: update screenshot #2 -->

`config.auth.logo`로 지정한 로고는 로그인 화면에만 표시됩니다:

![Auth 로고 위치](/img/assets/development/config-auth-logo.png)

`config.menu.logo`로 지정한 로고는 관리자 패널 좌측 상단 메인 네비게이션에 표시됩니다:

![메뉴 로고 위치](/img/assets/development/config-menu-logo.png)

### 로고 변경 방법

로고 이미지를 `/src/admin/extensions` 폴더에 두고, `src/admin/app`에서 import한 뒤 아래 예시처럼 해당 키를 업데이트하세요:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/admin/app.js"
import AuthLogo from "./extensions/my-auth-logo.png";
import MenuLogo from "./extensions/my-menu-logo.png";

export default {
  config: {
    // … other configuration properties 
    auth: { // Replace the Strapi logo in auth (login) views
      logo: AuthLogo,
    },
    menu: { // Replace the Strapi logo in the main navigation
      logo: MenuLogo,
    },
    // … other configuration properties 

  bootstrap() {},
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```jsx title="/src/admin/app.ts"
import AuthLogo from "./extensions/my-auth-logo.png";
import MenuLogo from "./extensions/my-menu-logo.png";

export default {
  config: {
    // … other configuration properties 
    auth: { // Replace the Strapi logo in auth (login) views
      logo: AuthLogo,
    },
    menu: { // Replace the Strapi logo in the main navigation
      logo: MenuLogo,
    },
    // … other configuration properties 

  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::note
There is no size limit for image files set through the configuration files.
:::
