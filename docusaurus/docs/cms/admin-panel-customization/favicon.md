---
title: 파비콘
description: Strapi 관리자 패널에 표시되는 파비콘을 교체하는 방법을 안내합니다.
displayed_sidebar: cmsSidebar
sidebar_label: 파비콘
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 파비콘

Strapi의 [관리자 패널](/cms/admin-panel-customization)에는 브랜드를 나타내는 여러 이미지가 표시됩니다. 대표적으로 [로고](/cms/admin-panel-customization/logos)와 파비콘이 있습니다. 이 이미지를 교체하면 인터페이스와 애플리케이션을 브랜드 아이덴티티에 맞게 맞출 수 있습니다.

파비콘을 교체하려면:

1. `/src/admin/extensions/` 폴더가 없다면 새로 만듭니다.
2. 파비콘 이미지를 `/src/admin/extensions/` 폴더에 업로드합니다.
3. Strapi 애플리케이션 루트의 기존 **favicon.png|ico** 파일을 원하는 커스텀 `favicon.png|ico` 파일로 교체합니다.
4. `/src/admin/app.[tsx|js]` 파일을 아래와 같이 수정합니다:

   ```js title="./src/admin/app.js"
   import favicon from "./extensions/favicon.png";

   export default {
     config: {
       // 커스텀 아이콘으로 파비콘 교체
       head: {
         favicon: favicon,
       },
     },
   };
   ```

5. 터미널에서 `yarn build && yarn develop` 명령어로 앱을 다시 빌드하고 실행한 뒤, 변경된 파비콘이 적용됐는지 확인합니다.

:::tip
동일한 방법으로 로그인 로고(`AuthLogo`)와 메뉴 로고(`MenuLogo`)도 교체할 수 있습니다. ([로고 커스터마이징 문서](/cms/admin-panel-customization/logos) 참고)
:::

:::caution
파비콘이 캐시될 수 있으니, 웹 브라우저와 Cloudflare CDN 등 도메인 관리 도구의 캐시를 반드시 삭제하세요.
:::

