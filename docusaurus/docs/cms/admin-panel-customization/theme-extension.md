---
title: 테마 확장
description: Strapi 관리자 패널의 테마를 확장하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 테마 확장
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

# 테마 확장

Strapi의 [관리자 패널](/cms/admin-panel-customization)은 라이트/다크 모드를 지원하며, 두 모드 모두 커스텀 테마 설정을 통해 확장할 수 있습니다.

테마를 확장하려면 다음 중 하나를 사용하세요:

- 라이트 모드 확장: `config.theme.light` 키
- 다크 모드 확장: `config.theme.dark` 키

:::strapi Strapi 디자인 시스템
기본 <ExternalLink to="https://github.com/strapi/design-system/tree/main/packages/design-system/src/themes" text="Strapi 테마"/>는 다양한 테마 관련 키(그림자, 색상 등)를 정의하며, `config.theme.light` 및 `config.theme.dark` 키를 통해 업데이트할 수 있습니다. <ExternalLink to="https://design-system.strapi.io/" text="Strapi 디자인 시스템"/>은 완전히 커스터마이징 가능하며, <ExternalLink to="https://design-system-git-main-strapijs.vercel.app" text="StoryBook"/> 문서도 참고할 수 있습니다.
:::
