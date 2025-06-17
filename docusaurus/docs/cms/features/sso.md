---
title: 싱글 사인온(SSO)
description: SSO(싱글 사인온) 기능을 사용하여 외부 인증 제공업체를 통한 인증을 관리하는 방법을 알아보세요.
toc_max_heading_level: 6
tags:
- admin panel
- SSO
- single sign-on
- features
---

# 싱글 사인온(SSO)
<EnterpriseBadge /> <SsoBadge />

싱글 사인온(SSO) 기능을 사용하면 Strapi 애플리케이션의 관리자가 외부 인증 제공업체(예: Microsoft Azure Active Directory)를 통해 인증할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">CMS 엔터프라이즈 플랜 또는 CMS Growth 플랜의 SSO 애드온</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">Roles > Settings - Single Sign-On에서 읽기 및 수정 권한 필요</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본적으로 비활성화됨</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<ThemedImage
alt="로그인 페이지"
sources={{
    light: '/img/assets/getting-started/login-page-sso.png',
    dark: '/img/assets/getting-started/login-page_DARK.png',
  }}
/>

## 설정

SSO의 일반 설정은 관리자 패널에서 제공되며, 추가 SSO 제공업체는 Strapi 프로젝트의 코드를 통해 설정할 수 있습니다.

### 관리자 패널 설정

**기능 설정 경로:** <Icon name="gear-six" /> *글로벌 설정 > 싱글 사인온*

1. *싱글 사인온* 인터페이스에서 다음 설정을 지정합니다:

| 설정명      | 설명      |
| ----------- | ---------------------|
| 자동 등록 | **True**로 설정하면 SSO 로그인 시 기존 Strapi 관리자 계정이 없을 경우 자동으로 새 관리자 계정이 생성됩니다. **False**로 설정하면 SSO로 로그인하기 전에 관리자 계정을 미리 생성해야 합니다. |
| 기본 역할      | SSO 로그인 시 자동 등록되는 관리자에게 기본으로 할당할 역할을 드롭다운에서 선택합니다.           |
| 로컬 인증 차단 | 드롭다운에서 로컬 인증(아이디/비밀번호 로그인)을 차단할 역할을 선택합니다([Users & Permissions 기능](/cms/features/users-permissions#advanced-settings) 참고).<br />로컬 인증이 차단된 사용자는 반드시 SSO로만 로그인해야 하며, 비밀번호 변경/재설정이 불가합니다. |

2. **저장** 버튼을 클릭합니다.

:::danger
_슈퍼 관리자(Super Admin)_ 역할을 로컬 인증 차단 목록에 추가하지 마세요. 추가할 경우 관리자 패널에 완전히 접근할 수 없게 될 수 있습니다. 곧 수정 예정입니다.

만약 슈퍼 관리자가 로그인할 수 없는 경우, SSO 기능을 임시로 비활성화한 뒤 아이디/비밀번호로 로그인하여 슈퍼 관리자 역할을 차단 목록에서 제거한 후 SSO를 다시 활성화해야 합니다.
:::

<ThemedImage
  alt="SSO 설정"
  sources={{
    light: '/img/assets/settings/settings-sso.png',
    dark: '/img/assets/settings/settings-sso_DARK.png',
  }}
/>

### 코드 기반 설정

SSO 설정은 [ `/config/admin` 파일](/cms/configurations/admin-panel)에 저장됩니다. 다양한 인증 및 회원가입 방식을 추가로 설정하려면 아래 가이드를 참고하세요:

<CustomDocCardsWrapper>
<CustomDocCard icon="sign-in" title="SSO 제공업체 설정 방법" description="Strapi 프로젝트의 코드를 통해 SSO 제공업체를 설정하는 방법을 알아보세요." link="/cms/configurations/guides/configure-sso"/>
</CustomDocCardsWrapper>

## 사용법

특정 제공업체를 통해 관리자 패널에 접근하려면 일반 관리자 계정 로그인 대신 다음 절차를 따르세요:

1. Strapi 애플리케이션의 관리자 패널 URL로 이동합니다.
2. 로그인 폼 하단에 표시된 제공업체 로고 중 원하는 제공업체를 클릭합니다. 제공업체가 보이지 않으면 <Icon name="dots-three-outline" /> 버튼을 클릭해 전체 목록을 확인할 수 있습니다.
3. 선택한 제공업체의 로그인 페이지로 리다이렉트되어 인증을 진행합니다.
