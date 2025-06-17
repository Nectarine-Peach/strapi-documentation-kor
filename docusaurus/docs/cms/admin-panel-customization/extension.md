---
title: 관리자 패널 확장
description: Strapi 관리자 패널을 확장하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징

---

import HotReloading from '/docs/snippets/hot-reloading-admin-panel.md'

# 관리자 패널 확장

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 Strapi 애플리케이션의 모든 기능과 설치된 플러그인을 포함하는 React 기반 싱글 페이지 애플리케이션입니다. Strapi에서 제공하는 [커스터마이징 옵션](/cms/admin-panel-customization#available-customizations)만으로는 요구사항을 충족할 수 없는 경우, 관리자 패널을 확장해야 합니다.

관리자 패널 확장이란, React 기반 구조를 활용해 프로젝트의 특정 요구에 맞게 인터페이스와 기능을 확장하거나 새로운 컴포넌트, 필드 타입 등을 추가하는 것을 의미합니다.

관리자 패널을 확장해야 하는 경우는 2가지가 있습니다:

- Strapi 플러그인 개발자로서, **어떤 Strapi 애플리케이션에 설치해도 항상 관리자 패널을 확장하는 플러그인**을 개발하고 싶은 경우

  👉 [플러그인용 관리자 패널 API](/cms/plugins-development/admin-panel-api)를 활용하면 됩니다.

- Strapi 개발자로서, **특정 Strapi 애플리케이션 인스턴스만을 위한 고유한 확장**을 개발하고 싶은 경우

  👉 `/src/admin/app` 파일을 직접 수정하고, `/src/admin/extensions` 내의 파일을 import하여 확장할 수 있습니다.

:::strapi 추가 자료
* 기본 리치 텍스트 에디터를 교체하고 싶다면 [관련 문서](/cms/admin-panel-customization/wysiwyg-editor)를 참고하세요.
* <ExternalLink to="https://design-system.strapi.io/?path=/docs/getting-started-welcome--docs" text="Strapi Design System 문서"/>에서도 관리자 패널 개발에 대한 추가 정보를 확인할 수 있습니다.
:::

<HotReloading />
