---
title: 플러그인 구조
description: Strapi 플러그인의 구조에 대해 자세히 알아보세요
displayed_sidebar: cmsSidebar
tags:
- 관리자 패널
- 커맨드 라인 인터페이스 (CLI)
- 백엔드 서버
- 플러그인
- 플러그인 개발
- 플러그인 구조
- 서버 전용 플러그인
---

import InteractivePluginStructure from '@site/src/components/PluginStructure.js'

# 플러그인 구조

[Plugin SDK로 플러그인을 생성](/cms/plugins-development/create-a-plugin)할 때, Strapi는 `/src/plugins/my-plugin` 폴더에 다음과 같은 보일러플레이트 구조를 생성합니다:

<InteractivePluginStructure />

Strapi 플러그인은 2개 부분으로 나뉘며, 각각 다른 폴더에 있고 다른 API를 제공합니다:

| 플러그인 부분 | 설명 | 폴더       | API |
|-------------|-------------|--------------|-----|
| 관리자 패널 | [관리자 패널](/cms/intro)에서 보이는 것들을 포함합니다 (컴포넌트, 네비게이션, 설정 등) | `admin/` |[관리자 패널 API](/cms/plugins-development/admin-panel-api)|
| 백엔드 서버 | [백엔드 서버](/cms/backend-customization)와 관련된 것들을 포함합니다 (콘텐츠 타입, 컨트롤러, 미들웨어 등) |`server/` |[서버 API](/cms/plugins-development/server-api)|

<br />

:::note 특정 사용 사례에 대한 다른 부분의 유용성에 대한 참고사항
- **서버 전용 플러그인**: 애플리케이션의 API를 향상시키기 위해 서버 부분만 사용하는 플러그인을 만들 수 있습니다. 예를 들어, 이 플러그인은 특정 사용 사례에 유용한 자체적인 표시되거나 숨겨진 콘텐츠 타입, 컨트롤러 액션, 라우트를 가질 수 있습니다. 이런 시나리오에서는 플러그인이 관리자 패널에 인터페이스를 가질 필요가 없습니다.

- **관리자 패널 플러그인 vs. 애플리케이션별 커스터마이징**: 관리자 패널에 일부 컴포넌트를 주입하기 위한 플러그인을 만들 수 있습니다. 하지만 `/src/admin/index.js` 파일을 생성하고 `bootstrap` 라이프사이클 함수를 호출하여 컴포넌트를 주입하는 방법으로도 이를 달성할 수 있습니다. 이 경우 플러그인을 만들지 여부는 코드를 재사용하고 배포할 계획이 있는지 또는 고유한 Strapi 애플리케이션에만 유용한지에 따라 결정됩니다.
:::

<br/>

:::strapi 다음에 읽을 내용은?
Strapi 플러그인 개발 여정의 다음 단계는 Strapi 플러그인 API 중 하나를 사용해야 합니다.

플러그인 API 사용 방법을 이해하는 데 도움이 되는 2가지 유형의 리소스가 있습니다:

- [관리자 패널 API](/cms/plugins-development/admin-panel-api)와 [서버 API](/cms/plugins-development/server-api)에 대한 참조 문서는 Strapi 플러그인으로 할 수 있는 것들에 대한 개요를 제공합니다.
- [가이드](/cms/plugins-development/developing-plugins#guides)는 특정 사용 사례 기반 예제들을 다룹니다.
:::
