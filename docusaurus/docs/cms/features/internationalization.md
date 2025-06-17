---
title: 국제화(i18n)
description: 콘텐츠 관리자가 콘텐츠를 번역할 수 있도록 해주는 국제화(i18n) 기능 사용법을 알아보세요.
toc_max_heading_level: 5
tags:
- admin panel
- internationalization
- i18n
- translation
- locale
- features
---

# 국제화(i18n)

국제화(i18n) 기능을 사용하면 다양한 언어(로케일)로 콘텐츠를 관리할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">없음</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본적으로 비활성화되어 있음</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<Guideflow lightId="zkj5431ser" darkId="vkm9nn0t3p"/>

## 설정

국제화 기능을 사용하려면 먼저 <Icon name="gear-six" /> *설정*에서 활성화해야 하며, <Icon name="layout" /> _콘텐츠 타입 빌더_에서 각 콘텐츠 타입에 대해 별도로 활성화해야 합니다.

### 콘텐츠 타입 빌더

**기능 설정 경로:** <Icon name="layout" /> _콘텐츠 타입 빌더_

콘텐츠 매니저에서 국제화 기능을 사용하려면, 각 콘텐츠 타입 및/또는 필드에서 국제화 옵션을 활성화해야 합니다.

1. 이미 생성된 콘텐츠 타입/필드를 수정하거나 새로 생성합니다.
2. **고급 설정** 탭으로 이동합니다.
3. 콘텐츠 타입 수준에서는 "국제화" 옵션을, 필드 수준에서는 "이 필드에 로컬라이제이션 활성화" 옵션을 체크합니다.

<ThemedImage
  alt="콘텐츠 타입 빌더의 고급 설정"
  sources={{
    light: '/img/assets/content-type-builder/advanced-settings.png',
    dark: '/img/assets/content-type-builder/advanced-settings_DARK.png',
  }}
/>

### 설정

**기능 설정 경로:** <Icon name="gear-six" /> *설정 > 글로벌 설정 > 국제화*

*국제화* 인터페이스에는 Strapi 애플리케이션에서 사용 가능한 모든 로케일이 테이블로 표시됩니다. 기본적으로 영어(English) 로케일만 설정되어 있으며, 기본 로케일로 지정되어 있습니다.

각 로케일에 대해 테이블에는 로케일의 ISO 코드, (선택) 표시 이름, 기본 로케일 여부가 표시됩니다. 테이블에서 관리자는 다음 작업을 할 수 있습니다:

- <Icon name="pencil-simple" /> 편집 버튼 클릭 시 로케일 편집
- <Icon name="trash" /> 삭제 버튼 클릭 시 로케일 삭제

#### 새 로케일 추가

관리자는 원하는 만큼 로케일을 추가 및 관리할 수 있습니다. 단, 전체 Strapi 애플리케이션에는 기본 로케일이 하나만 존재할 수 있습니다.

:::note
사용자 지정 로케일은 생성할 수 없습니다. Strapi에서 미리 정의한 <ExternalLink to="https://github.com/strapi/strapi/blob/main/packages/plugins/i18n/server/src/constants/iso-locales.json" text="500개 이상의 로케일 목록"/> 중에서만 선택할 수 있습니다.
:::

1. **새 로케일 추가** 버튼을 클릭합니다.
2. 로케일 추가 창에서 *로케일* 드롭다운 목록에서 원하는 로케일을 선택합니다. 이 목록에는 Strapi에서 지원하는 모든 로케일의 ISO 코드가 알파벳순으로 표시됩니다.
3. (선택) *로케일 표시 이름* 텍스트박스에 새 로케일의 표시 이름을 입력합니다.
4. (선택) 고급 설정 탭에서 *기본 로케일로 설정*을 체크하면 해당 로케일이 전체 Strapi 애플리케이션의 기본 로케일이 됩니다.
5. **저장** 버튼을 클릭하여 새 로케일 추가를 완료합니다.

<ThemedImage
  alt="i18n으로 새 로케일 추가"
  sources={{
    light: '/img/assets/settings/new-locale-i18n.png',
    dark: '/img/assets/settings/new-locale-i18n_DARK.png',
  }}
/>

### 코드 기반 설정

기본 로케일을 환경별로 지정하려면 `STRAPI_PLUGIN_I18N_INIT_LOCALE_CODE` [환경 변수](/cms/configurations/environment#strapi)를 설정할 수 있습니다. 값은 <ExternalLink to="https://github.com/strapi/strapi/blob/main/packages/plugins/i18n/server/src/constants/iso-locales.json" text="Strapi에서 미리 정의한 500개 이상의 ISO 코드"/> 중 하나여야 합니다.

## 사용법

**기능 사용 경로:** <Icon name="feather" /> 콘텐츠 매니저, 콘텐츠 타입의 편집 화면

[콘텐츠 매니저](/cms/features/content-manager)에서, 콘텐츠 타입에 국제화 기능이 활성화되어 있으면, 편집 화면 오른쪽 상단에 로케일 드롭다운이 추가되어 로케일을 전환할 수 있습니다.

국제화 기능을 사용하면 다이나믹 존 및 컴포넌트도 로케일별로 다르게 구성할 수 있습니다. 즉, 로케일에 따라 다이나믹 존의 구조가 달라질 수 있고, 반복 가능한 컴포넌트도 로케일별로 다르게 입력 및 정렬할 수 있습니다.

:::caution
콘텐츠는 한 번에 한 로케일만 관리할 수 있습니다. 여러 로케일의 콘텐츠를 동시에 수정하거나 발행할 수 없습니다(예: **발행** 버튼을 클릭하면 현재 작업 중인 로케일의 콘텐츠만 발행됨).
:::

다른 로케일로 콘텐츠를 번역하려면:

1. 편집 화면 오른쪽 상단의 로케일 드롭다운을 클릭합니다.
2. 번역할 로케일을 선택합니다.
3. 해당 로케일의 콘텐츠 타입 필드를 입력하여 번역합니다.

:::tip
오른쪽 상단의 <Icon name="download-simple" /> *다른 로케일에서 채우기* 버튼을 클릭하면, 선택한 로케일의 값을 현재 로케일의 비관계형 필드에 자동으로 채울 수 있습니다. 다른 로케일의 정확한 내용을 기억하지 못할 때 유용합니다.
:::

<ThemedImage
  alt="i18n으로 로케일 관리"
  sources={{
    light: '/img/assets/content-manager/locale-i18n.png',
    dark: '/img/assets/content-manager/locale-i18n_DARK.png',
  }}
/>

### API와 함께 사용하기

로케일별 콘텐츠는 [Strapi의 콘텐츠 API](/cms/api/content-api)에서 제공하는 다양한 프론트엔드 API를 통해 요청, 생성, 수정, 삭제할 수 있습니다:

<CustomDocCardsWrapper>
<CustomDocCard icon="cube" title="REST API" description="REST API에서 locale 파라미터 사용법을 알아보세요." link="/cms/api/rest/locale"/>
<CustomDocCard icon="cube" title="GraphQL API" description="GraphQL API에서 locale 파라미터 사용법을 알아보세요." link="/cms/api/graphql#locale"/>
</CustomDocCardsWrapper>

Strapi 백엔드 서버에서는 Document Service API를 통해서도 로케일별 콘텐츠를 다룰 수 있습니다:

<CustomDocCardsWrapper>
<CustomDocCard icon="cube" title="Document Service API" description="Document Service API에서 locale 파라미터 사용법을 알아보세요." link="/cms/api/document-service/locale"/>
</CustomDocCardsWrapper>
