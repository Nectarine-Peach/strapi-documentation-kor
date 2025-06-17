---
title: 플러그인 개발
description: Strapi 플러그인 개발에 대한 일반적인 소개
displayed_sidebar: cmsSidebar
pagination_prev: cms/plugins-development/developing-plugins
pagination_next: cms/plugins-development/create-a-plugin
tags:
- 관리자 패널 API
- 소개
- 플러그인 API
- 플러그인 개발
- 서버 API
---

# Strapi 플러그인 개발

Strapi는 내장 플러그인이나 <ExternalLink to="https://market.strapi.io" text="마켓플레이스"/>에서 사용할 수 있는 서드파티 플러그인과 정확히 동일하게 작동하는 플러그인 개발을 허용합니다. 플러그인을 생성한 후에는:

- 특정 Strapi 프로젝트에서만 작동하는 로컬 플러그인으로 사용하거나,
- 커뮤니티와 공유하기 위해 <ExternalLink to="https://market.strapi.io/submit-plugin" text="마켓플레이스에 제출"/>할 수 있습니다.

👉 Strapi 플러그인 개발을 시작하려면:

1. Plugin SDK를 사용하여 [플러그인을 생성](/cms/plugins-development/create-a-plugin)합니다.
2. [플러그인의 구조](/cms/plugins-development/plugin-structure)에 대해 자세히 알아봅니다.
3. 플러그인에 기능을 추가하기 위한 [플러그인 API](#plugin-apis)의 개요를 확인합니다.
4. 사용 사례에 따른 고급 [가이드](#guides)를 읽어봅니다.

:::note
v4 호환 버전과 구별하기 위해 Strapi 5 플러그인을 다른 주요 버전 번호로 릴리즈하도록 하세요.
:::

## 플러그인 API

Strapi는 플러그인이 Strapi의 일부 기능에 연결할 수 있도록 다음과 같은 프로그래밍 API를 제공합니다:

<CustomDocCardsWrapper>
<CustomDocCard emoji="" title="관리자 패널 API" description="관리자 패널 API를 사용하여 플러그인이 Strapi의 관리자 패널과 상호작용하도록 합니다." link="/cms/plugins-development/admin-panel-api" />
<CustomDocCard emoji="" title="서버 API" description="서버 API를 사용하여 플러그인이 Strapi의 백엔드 서버와 상호작용하도록 합니다." link="/cms/plugins-development/server-api" />
</CustomDocCardsWrapper>

:::strapi 커스텀 필드 플러그인
플러그인은 Strapi에 [커스텀 필드](/cms/features/custom-fields)를 추가하는 데도 사용할 수 있습니다.
:::

## 가이드

<CustomDocCard small emoji="💁" title="Strapi 플러그인에서 데이터를 저장하고 접근하는 방법" description="" link="/cms/plugins-development/guides/store-and-access-data" />
<CustomDocCard small emoji="💁" title="플러그인으로 백엔드 서버에서 관리자 패널로 데이터를 전달하는 방법" description="" link="/cms/plugins-development/guides/pass-data-from-server-to-admin" />

<br />

:::strapi 추가 리소스
<ExternalLink to="https://contributor.strapi.io/" text="기여자 문서"/>에는 Strapi 플러그인 개발 시 유용한 추가 정보가 포함될 수도 있습니다.
:::
