---
title: 커스터마이징
description: Strapi 5의 다양한 커스터마이징 가능성을 알아보세요
displayed_sidebar: cmsSidebar
pagination_next: cms/backend-customization
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
- 백엔드 커스터마이징
- 백엔드 서버
- 개념
- Content-type Builder
- Content Manager
- 소개
---

# 커스터마이징

Strapi는 2가지 주요 컴포넌트로 구성되어 있습니다:

- Strapi의 백엔드 부분은 **서버**로, 요청을 받아 응답을 반환하며, Content-Type Builder와 Content Manager를 통해 구축하고 저장한 데이터를 노출합니다. 백엔드 서버에 대한 자세한 설명은 [백엔드 커스터마이징 소개](/cms/backend-customization)에서 확인할 수 있습니다. 백엔드 서버의 대부분은 커스터마이징이 가능합니다.

- Strapi의 프론트엔드, 즉 사용자 인터페이스 부분은 **관리자 패널**이라고 부릅니다. 관리자 패널은 콘텐츠 구조를 만들고, 콘텐츠를 관리하며, 내장 또는 서드파티 플러그인을 통해 다양한 작업을 수행할 수 있는 그래픽 사용자 인터페이스(GUI)입니다. 관리자 패널의 일부도 커스터마이징할 수 있습니다.

전체적인 관점에서 Strapi는 일반적인 환경에서 다음과 같이 통합됩니다: Strapi는 백엔드 서버와 관리자 패널 두 부분으로 구성되며, 데이터베이스(데이터 저장) 및 외부 프론트엔드 애플리케이션(데이터 표시)과 상호작용합니다. Strapi의 두 부분 모두 어느 정도까지 커스터마이징이 가능합니다.

<MermaidWithFallback
    chartFile="/diagrams/customization.mmd"
    fallbackImage="/img/assets/diagrams/customization.png"
    fallbackImageDark="/img/assets/diagrams/customization_DARK.png"
    alt="커스터마이징 다이어그램"
/>

<br />

아래 카드 중 하나를 클릭하면 커스터마이징 가능 항목에 대해 더 자세히 알아볼 수 있습니다:

<CustomDocCardsWrapper>
<CustomDocCard emoji="" title="백엔드 커스터마이징" description="백엔드 서버(라우트, 정책, 미들웨어, 컨트롤러, 서비스, 모델 등) 커스터마이징." link="/cms/backend-customization" />
<CustomDocCard emoji="" title="관리자 패널 커스터마이징" description="관리자 패널(로고, 테마, 메뉴, 번역 등) 커스터마이징." link="/cms/admin-panel-customization" />
</CustomDocCardsWrapper>

:::info
데이터베이스 또는 외부 프론트엔드 애플리케이션 커스터마이징은 본 문서 범위에 포함되지 않습니다.
- Strapi에서 지원하는 데이터베이스에 대한 자세한 내용은 설치 문서([지원 데이터베이스](/cms/installation/cli#preparing-the-installation))와 [데이터베이스 구성](/cms/configurations/database) 문서를 참고하세요.
- 외부 프론트엔드 애플리케이션이 Strapi와 상호작용하는 방법은 Strapi의 <ExternalLink to="https://strapi.io/integrations" text="통합 페이지"/>에서 확인할 수 있습니다.
:::
