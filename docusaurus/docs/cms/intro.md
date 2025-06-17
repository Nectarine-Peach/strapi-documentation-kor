---
sidebar_label: '소개'
description: Strapi CMS 문서는 관리자 패널 관련 모든 정보와 Strapi 5 애플리케이션의 설정, 고급 사용법, 커스터마이징, 업데이트에 관한 기술 정보를 담고 있습니다.
displayed_sidebar: cmsSidebar
slug: /cms/intro
pagination_next: cms/quick-start
sidebar_position: 1
tags:
 - 소개
 - 개념
---

# Strapi CMS 공식 문서에 오신 것을 환영합니다!

<!--
<SubtleCallout title="Strapi CMS & Strapi Cloud 문서" emoji="📍">

Strapi에는 각 제품별로 2가지 공식 문서가 있습니다:

- <Icon name="feather" /> **CMS 문서**: 현재 보고 계신 문서로, Strapi 5 프로젝트(설치, 설정, 배포, 관리자 패널에서의 콘텐츠 관리 등)에 관한 모든 정보를 담고 있습니다.
- <Icon name="cloud" /> **[Cloud 문서](/cloud/intro)**: Strapi Cloud에 애플리케이션을 배포하고, Strapi Cloud 프로젝트 및 설정을 관리하는 방법을 안내합니다.

</SubtleCallout>
-->

Strapi CMS 공식 문서는 Strapi 5를 중심으로, 프로젝트의 시작부터 고급 사용법, 커스터마이징, 업데이트, 관리자 패널 및 콘텐츠·사용자 관리까지 전체 여정을 안내합니다.

<ThemedImage
alt="관리자 패널 홈 화면"
sources={{
    light: '/img/assets/getting-started/admin-panel-homepage-2.png',
    dark: '/img/assets/getting-started/admin-panel-homepage-2_DARK.png',
  }}
/>

:::strapi 처음 시작하는 분들을 위한 안내
**Strapi**가 완전히 처음이라면 <Annotation>**💡 알고 계셨나요?**<br />이 프로젝트의 원래 목적은 Boot**strap** your **API**를 돕는 것이었습니다. 그래서 Strapi라는 이름이 탄생했고, 지금의 Strapi가 만들어졌습니다.<br /><br />현재 Strapi는 개발자가 선호하는 도구와 프레임워크를 자유롭게 선택할 수 있고, 에디터가 애플리케이션의 관리자 패널을 통해 콘텐츠를 관리·배포할 수 있는 **오픈소스 헤드리스 CMS**입니다.<br /><br />플러그인 시스템 기반으로, Strapi는 관리자 패널과 API 모두 확장 가능하며, 모든 부분을 원하는 대로 커스터마이징할 수 있습니다. 또한 내장된 사용자 시스템을 통해 관리자의 권한과 최종 사용자의 접근을 세밀하게 제어할 수 있습니다.<br /></Annotation> 아래 순서로 시작해보세요:

1. [빠른 시작](/cms/quick-start) 가이드를 참고하세요.
2. 관리자 패널과 Strapi CMS의 두 핵심 기능(콘텐츠 매니저, 콘텐츠 타입 빌더)에 대해 알아보세요.
3. 각 기능별로 별도 문서가 준비되어 있으니, Draft & Publish, 다국어(i18n), 정적 미리보기 등 관심 있는 기능을 살펴보세요.
:::

Strapi CMS 공식 문서의 목차는 여러분의 사용 여정에 맞춰 7개 주요 섹션으로 구성되어 있습니다. 각 카드를 클릭하면 해당 섹션의 소개 또는 가장 많이 읽힌 페이지로 이동합니다.

<CustomDocCardsWrapper>

<CustomDocCard icon="rocket" title="시작하기" description="Strapi 설치 및 배포, 관리자 패널 사용법. 초보자에게 추천!" link="/cms/installation" />

<CustomDocCard icon="backpack" title="기능" description="Strapi의 다양한 기능과 설정·사용법을 알아보세요." link="/cms/features/api-tokens" />

<CustomDocCard icon="cube" title="API" description="REST, GraphQL, Strapi의 저수준 API로 콘텐츠를 쿼리하세요." link="/cms/api/content-api" />

<CustomDocCard icon="gear-fine" title="설정" description="프로젝트의 기본 및 추가 설정 방법을 안내합니다." link="/cms/configurations" />

<CustomDocCard icon="laptop" title="개발" description="Strapi 서버와 관리자 패널 커스터마이징, 고급 옵션 안내." link="/cms/customization" />

<CustomDocCard icon="puzzle-piece" title="플러그인" description="내장 플러그인 사용 또는 직접 플러그인 개발 방법." link="/cms/plugins/installing-plugins-via-marketplace" />

<CustomDocCard icon="escalator-up" title="업그레이드" description="애플리케이션을 최신 Strapi 버전으로 업그레이드하세요." link="/cms/migration/v4-to-v5/introduction-and-faq" />

</CustomDocCardsWrapper>

:::tip 문서를 100% 활용하는 팁
- 찾고자 하는 내용이 명확하다면, 검색창이나 목차를 활용하세요.
- 프로젝트 코드 구조를 보며 학습하고 싶다면, [프로젝트 구조](/cms/project-structure) 페이지를 참고하세요.
- 데모가 필요하다면 <ExternalLink to="https://youtu.be/zd0_S_FPzKg" text="영상 데모"/>를 시청하거나, <ExternalLink to="https://strapi.io/demo" text="라이브 데모"/>를 신청할 수 있습니다.
- AI 어시스턴트도 활용해보세요: **Ask AI** 버튼을 눌러 자연어로 질문하면, 실시간 답변과 추천 자료를 확인할 수 있습니다.
- Strapi Docs를 AI 모델과 연동하고 싶다면 **Copy Markdown** 버튼이나 [`llms.txt`](/llms.txt), [`llms-full.txt`](/llms-full.txt) 페이지를 참고하세요.
:::

:::strapi 초보 개발자를 위한 정보
CMS 문서의 일부(예: API, 설정, 개발)는 주로 개발자를 대상으로 하며, JavaScript 생태계에 대한 사전 지식을 전제로 합니다.

Strapi와 함께 JavaScript 웹 개발을 처음 접하신다면, <ExternalLink to="https://developer.mozilla.org/ko/docs/Learn/Getting_started_with_the_web/JavaScript_basics" text="JavaScript" />와 <ExternalLink to="https://docs.npmjs.com/about-npm" text="npm" />에 대해 먼저 학습해보시길 권장합니다. 프로젝트에 따라 <ExternalLink text="TypeScript" to="https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html" />도 미리 익혀두면 CMS 문서의 기술적 내용을 이해하는 데 도움이 됩니다.
:::
