---
title: 업그레이드
description: Strapi 5의 업그레이드 프로세스에 대해 자세히 알아보세요
displayed_sidebar: cmsSidebar
pagination_prev: cms/plugins-development/developing-plugins
pagination_next: cms/upgrade-tool
tags:
- 마이그레이션
- 업그레이드
- 업그레이드 도구
- Strapi 버전 
---

import InstallCommand from '/docs/snippets/install-npm-yarn.md'
import BuildCommand from '/docs/snippets/build-npm-yarn.md'
import DevelopCommand from '/docs/snippets/develop-npm-yarn.md'

# 업그레이드

Strapi는 새로운 버전을 통해 코드 개선 사항을 주기적으로 릴리즈합니다. 새로운 Strapi 버전은 터미널과 관리자 패널 모두에서 알림이 표시되며, <ExternalLink to="https://github.com/strapi/strapi/releases" text="GitHub 릴리즈 노트"/>에서 각 새 버전의 새로운 기능을 확인할 수 있습니다.

Strapi 코어 팀에서 릴리즈한 최신 Strapi 버전 번호는 <ExternalLink to="https://www.npmjs.com/package/@strapi/strapi" text="npm"/> 또는 <ExternalLink to="https://github.com/strapi/strapi/releases" text="GitHub"/>에서 찾을 수 있습니다.

새로운 버전의 Strapi가 릴리즈되면 업그레이드를 원할 수 있으며, 이 페이지는 업그레이드에 대한 정보의 진입점 역할을 합니다.

<details>
<summary>현재 Strapi 버전 번호를 어떻게 찾을 수 있나요?</summary>

Strapi 애플리케이션의 현재 버전 번호는 다음 방법으로 찾을 수 있습니다:

- 관리자 패널에서 _설정 > 전역 설정 > 개요_로 이동하여 세부 정보 섹션에 표시된 Strapi 버전 번호를 확인:

  <ThemedImage
    alt="관리자 패널에서 Strapi 버전 번호 찾기"
    sources={{
      light: '/img/assets/migration/strapi-version-number.png',
      dark: '/img/assets/migration/strapi-version-number_DARK.png'
    }}
  />

- 또는 Strapi 프로젝트가 있는 폴더에서 터미널에서 `yarn strapi version` 또는 `npm run strapi version`을 실행하여 확인.

</details>

사용 사례에 따라 다음 2개 카드 중 하나를 클릭하세요:

<CustomDocCard emoji="4️⃣" title="Strapi v4를 실행 중이며 Strapi 5로 업그레이드하고 싶습니다." description="Strapi의 최신 주요 버전인 Strapi 5로 업그레이드하는 데 필요한 모든 정보." link="/cms/migration/v4-to-v5/introduction-and-faq" />
<CustomDocCard emoji="5️⃣" title="이미 Strapi 5를 실행 중이며 최신 버전으로 업그레이드하고 싶습니다." description="Strapi v4에서 Strapi 5로 또는 기존 Strapi 5.x.x 버전에서 더 최신 버전으로 업그레이드하는 자동 업그레이드 도구 사용에 대한 모든 정보." link="/cms/upgrade-tool" />
