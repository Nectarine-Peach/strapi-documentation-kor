---
title: 관리자 패널
description: 관리자 패널 사용법을 알아보세요.
toc_max_heading_level: 5
tags:
- 관리자 패널
- 프로필
- 라이트 모드
- 다크 모드
---

# 관리자 패널

관리자 패널은 Strapi 애플리케이션의 백오피스입니다. 관리자 패널에서 콘텐츠 타입을 관리하고 실제 콘텐츠를 작성할 수 있을 뿐만 아니라, 관리자 및 최종 사용자 모두를 관리할 수 있습니다.

<ThemedImage
alt="관리자 패널 홈 화면"
sources={{
    light: '/img/assets/admin-homepage/admin-panel-homepage-with-tour.png',
    dark: '/img/assets/admin-homepage/admin-panel-homepage-with-tour_DARK.png',
  }}
/>

:::tip
[위젯을 직접 만들어](/cms/admin-panel-customization/homepage) 관리자 패널의 홈페이지를 커스터마이징할 수 있습니다.
:::

## 개요

:::prerequisites
관리자 패널을 사용할 때 다음과 같은 요소들이 인터페이스와 사용 경험에 영향을 줄 수 있습니다.

- **개발, 스테이징, 프로덕션 환경** <br/> 콘텐츠 구조와 애플리케이션 설정 상태는 개발 환경에서 배포 후 프로덕션 또는 스테이징 환경으로 전환됩니다. 일부 기능은 개발 환경에서만 사용할 수 있습니다. 각 기능의 사용 가능 여부는 Identity Card에서 확인하세요.

- **라이선스 및 요금제** <br/> 일부 기능의 사용 가능 여부 또는 제한은 무료 Community Edition, <ExternalLink to="https://strapi.io/pricing-self-hosted" text="Growth plan"/> 또는 <ExternalLink to="https://strapi.io/pricing-self-hosted" text="Enterprise plan"/>에 따라 다릅니다. 문서 내 <GrowthBadge /> 및 <EnterpriseBadge /> 뱃지를 참고하세요.

- **역할 및 권한** <br/> 일부 기능과 콘텐츠는 세부적으로 정의할 수 있는 권한 시스템에 의해 제어됩니다. 본인의 역할과 권한에 따라 모든 기능과 옵션에 접근하지 못할 수 있습니다. 자세한 내용은 [RBAC 기능 문서](/cms/features/rbac)를 참고하세요.

- **Future flags** <br/> 일부 예정된 Strapi 기능은 아직 모든 사용자에게 공개되지 않았지만, 커뮤니티 사용자가 미리 피드백을 제공할 수 있도록 future flag로 제공됩니다. 이러한 실험적 기능은 해당 future flag를 활성화해야 사용할 수 있습니다. 문서 내 <FeatureFlagBadge /> 뱃지를 참고하고, [Feature flags 문서](/cms/configurations/features#enabling-a-future-flag)를 참고하세요.
:::

<Guideflow lightId="dkd2m1lsgr" darkId="dkd2mjlugr"/>

## 설정

**관리자 패널 설정 경로:** 계정명 또는 이니셜(좌측 하단) > 프로필

신규 관리자인 경우, Strapi 애플리케이션을 본격적으로 사용하기 전에 프로필을 먼저 설정하는 것을 권장합니다. 관리자 프로필에서 사용자 정보(이름, 사용자명, 이메일, 비밀번호)를 수정할 수 있습니다. 또한 Strapi 애플리케이션의 인터페이스 언어와 모드를 선택할 수 있습니다.

<ThemedImage
alt="관리자 패널 홈 화면"
sources={{
    light: '/img/assets/getting-started/user-information-profile.png',
    dark: '/img/assets/getting-started/user-information-profile_DARK.png',
  }}
/>

더 다양한 설정 및 커스터마이징 옵션이 있습니다. 자세한 내용은 아래 페이지를 참고하세요:

<CustomDocCardsWrapper>
  <CustomDocCard icon="panorama" title="코드 기반 설정" description="/config/admin 파일을 통해 Strapi 관리자 패널의 외관, 보안, 기능을 설정하세요." link="/cms/configurations/admin-panel" />
  <CustomDocCard icon="wrench" title="커스터마이징" description="브랜딩 맞춤, WYSIWYG 에디터 교체, 번들러 설정, 기능 확장 등 다양한 커스터마이징 방법을 알아보세요." link="/cms/admin-panel-customization" />
</CustomDocCardsWrapper>

### 프로필 정보(이름, 이메일, 사용자명) 수정

1. *프로필* 섹션으로 이동합니다.
2. 다음 옵션을 입력합니다:

| 프로필 & 경험 | 안내 |
| -------------------- | ------------------------------------------------- |
| 이름 | 텍스트박스에 이름을 입력하세요. |
| 성 | 텍스트박스에 성을 입력하세요. |
| 이메일 | 텍스트박스에 전체 이메일 주소를 입력하세요. |
| 사용자명 | (선택) 텍스트박스에 사용자명을 입력하세요. |

3. **저장** 버튼을 클릭합니다.

### 계정 비밀번호 변경

1. *비밀번호 변경* 섹션으로 이동합니다.
2. 다음 옵션을 입력합니다:

| 비밀번호 변경 | 안내 |
| --------------------- | ------------------------------------------- |
| 현재 비밀번호 | 텍스트박스에 현재 비밀번호를 입력하세요. |
| 새 비밀번호 | 텍스트박스에 새 비밀번호를 입력하세요. |
| 비밀번호 확인 | 텍스트박스에 동일한 새 비밀번호를 입력하세요. |

3. **저장** 버튼을 클릭합니다.

:::tip
비밀번호 입력란 옆의 <Icon name="eye" /> 아이콘을 클릭하면 비밀번호가 표시됩니다.
:::

### 인터페이스 언어 선택

*경험* 섹션에서 *인터페이스 언어* 드롭다운을 사용해 원하는 언어를 선택하세요.

:::note
인터페이스 언어 선택은 본인 계정에만 적용됩니다. 동일 애플리케이션의 다른 사용자는 각자 다른 언어를 사용할 수 있습니다.
:::

### 인터페이스 모드(라이트, 다크) 선택

기본적으로 브라우저의 모드에 따라 인터페이스 모드가 결정됩니다. 하지만 *경험* 섹션에서 *인터페이스 모드* 드롭다운을 통해 라이트 모드 또는 다크 모드를 직접 선택할 수 있습니다.

:::note
인터페이스 모드 선택 역시 본인 계정에만 적용됩니다.
:::

### 로고 커스터마이징

**관리자 패널 설정 경로:** <Icon name="gear-six" /> *설정 > 글로벌 설정 > 개요*

Strapi의 기본 로고는 애플리케이션의 메인 네비게이션과 인증 페이지에 표시됩니다. 이 이미지는 원하는 로고로 변경할 수 있습니다.

1. *메뉴 로고* 또는 *Auth 로고*의 업로드 영역을 클릭합니다.
2. 파일 탐색, 드래그&드롭, URL 입력 등으로 원하는 로고를 업로드합니다. 로고는 750x750px 이하를 권장합니다.
3. 업로드 창에서 **로고 업로드** 버튼을 클릭합니다.
4. 우측 상단의 **저장** 버튼을 클릭합니다.

업로드 후에는 <Icon name="plus" classes="ph-bold"/> 아이콘으로 새 로고로 교체하거나, <Icon name="arrow-clockwise" classes="ph-bold"/> 아이콘으로 기본 Strapi 로고 또는 설정 파일에 지정한 로고로 초기화할 수 있습니다.

:::note
두 로고 모두 [관리자 패널 커스터마이징](/cms/admin-panel-customization/logos) 문서에서 설명한 대로 설정 파일을 통해 프로그래밍 방식으로도 변경할 수 있습니다. 단, 관리자 패널에서 직접 업로드한 로고가 설정 파일의 로고보다 우선 적용됩니다.
:::

<ThemedImage
  alt="커스텀 로고 설정"
  sources={{
    light: '/img/assets/settings/settings_custom-logo.png',
    dark: '/img/assets/settings/settings_custom-logo_DARK.png',
  }}
/>

## 사용법

:::caution
관리자 패널에 접근하려면 Strapi 애플리케이션이 실행되고 있어야 하며, 관리자 패널의 URL(예: `api.example.com/admin`)을 알고 있어야 합니다.
:::

관리자 패널에 접근하는 방법:

1. Strapi 애플리케이션의 관리자 패널 URL로 이동합니다.
2. 로그인 자격 증명을 입력합니다.
3. **로그인** 버튼을 클릭합니다. 관리자 패널의 홈페이지로 리디렉션됩니다.

:::note
SSO 제공업체를 통해 로그인하는 것을 선호하거나 필요한 경우, [Single Sign-On 문서](/cms/features/sso)를 참고하세요.
:::

<ThemedImage
alt="로그인 페이지"
sources={{
    light: '/img/assets/getting-started/login-page-sso.png',
    dark: '/img/assets/getting-started/login-page_DARK.png',
  }}
/>
