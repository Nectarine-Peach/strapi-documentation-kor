---
title: API 토큰
description: API 토큰을 사용하여 엔드유저 인증을 관리하는 방법을 알아보세요.
sidebar_position: 2
toc_max_heading_level: 5
tags:
  - api tokens
  - admin panel
  - authentication
  - users & permissions
  - features
---

# API 토큰

API 토큰을 사용하면 사용자가 REST 및 GraphQL API 쿼리를 인증할 수 있습니다([API 소개](/cms/api/content-api) 참고).

<IdentityCard>
  <IdentityCardItem icon="layout" title="플랜">
    무료 기능
  </IdentityCardItem>
  
  <IdentityCardItem icon="user" title="역할 및 권한">
    최소 "API 토큰 설정 페이지 접근" 권한 필요 (Roles > Settings - API tokens)
  </IdentityCardItem>
  
  <IdentityCardItem icon="toggle-right" title="활성화">
    기본적으로 사용 가능
  </IdentityCardItem>
  
  <IdentityCardItem icon="desktop" title="환경">
    개발 및 운영 환경 모두에서 사용 가능
  </IdentityCardItem>
</IdentityCard>

<ThemedImage
alt="API tokens"
sources={{
    light: '/img/assets/settings/settings_pregen-api-tokens.png',
    dark: '/img/assets/settings/settings_pregen-api-tokens_DARK.png',
  }}
/>

## 설정

API 토큰의 대부분의 설정 옵션은 관리자 패널에서 제공되며, Strapi 프로젝트의 코드를 통해 토큰 생성 방식을 변경할 수도 있습니다.

### 관리자 패널 설정

**기능 설정 경로:** <Icon name="gear-six" /> _설정 > 글로벌 설정 > API 토큰_

_API 토큰_ 인터페이스에는 생성된 모든 API 토큰의 목록이 테이블로 표시됩니다. 각 토큰의 이름, 설명, 생성일, 마지막 사용일이 표시됩니다.

여기서 다음 작업이 가능합니다:

- <Icon name="pencil-simple" />를 클릭하여 API 토큰의 이름, 설명, 유형, 기간을 수정하거나 [토큰 재생성](#regenerating-an-api-token) 가능
- <Icon name="trash" />를 클릭하여 API 토큰 삭제

:::note
Strapi는 기본적으로 2개의 API 토큰(전체 접근, 읽기 전용)을 미리 생성합니다. 토큰은 암호화가 설정되지 않은 경우 한 번만 볼 수 있으므로, 암호화 키를 설정한 후 [재생성](#regenerating-an-api-token)하여 영구적으로 볼 수 있도록 하는 것이 좋습니다.
:::

#### 새 API 토큰 생성

1. **새 API 토큰 생성** 버튼을 클릭합니다.
2. API 토큰 편집 화면에서 다음을 설정합니다:
   | 설정명 | 설명 |
   | -------------- | ------------------------------------------------------------------------ |
   | 이름 | API 토큰의 이름을 입력합니다. |
   | 설명 | (선택) API 토큰에 대한 설명을 입력합니다. |
   | 토큰 기간 | 토큰 기간을 선택합니다: _7일_, _30일_, _90일_, _무제한_. |
   | 토큰 유형 | 토큰 유형을 선택합니다: _읽기 전용_, _전체 접근_, _사용자 지정_. |
3. (선택) _사용자 지정_ 유형의 경우, 콘텐츠 타입별로 API 엔드포인트 권한을 체크박스로 직접 설정할 수 있습니다.
4. **저장** 버튼을 클릭합니다. 새 API 토큰이 인터페이스 상단에 표시되며, 복사 버튼 <Icon name="copy" />도 함께 나타납니다.

<ThemedImage
alt="Custom API token"
sources={{
    light: '/img/assets/settings/settings_api-token-custom.png',
    dark: '/img/assets/settings/settings_api-token-custom_DARK.png',
  }}
/>

:::info 토큰 보기
Strapi 프로젝트에 암호화 키(`admin.secrets.encryptionKey`)가 설정되어 있으면, 새로 생성되거나 재생성된 API 토큰을 언제든지 관리자 패널에서 볼 수 있습니다.

암호화 키가 없으면, 토큰은 생성 또는 재생성 직후 **한 번만** 볼 수 있습니다.
:::

#### API 토큰 재생성

1. API 토큰의 편집 버튼을 클릭합니다.
2. **재생성** 버튼을 클릭합니다.
3. 대화 상자에서 **재생성** 버튼을 눌러 확인합니다.
4. 새로 표시된 API 토큰을 복사합니다.

### 코드 기반 설정

새 API 토큰은 salt를 사용하여 생성됩니다. 이 salt는 Strapi에서 자동으로 생성되어 환경 변수(`.env` 파일)의 `API_TOKEN_SALT`에 저장됩니다.

salt는 다음과 같이 커스터마이즈할 수 있습니다:

- `/config/admin` 파일의 `apiToken.salt` 값을 수정
- 또는 프로젝트의 `.env` 파일에 `API_TOKEN_SALT` 환경 변수 추가

:::caution
salt를 변경하면 기존의 모든 API 토큰이 무효화됩니다.
:::

#### 관리자 패널에서 API 토큰을 항상 볼 수 있도록 설정

API 토큰을 관리자 패널에서 항상 볼 수 있도록 하려면, `/config/admin` 파일의 `apiToken.secrets.encryptionKey`에 암호화 키를 추가해야 합니다:

<Tabs groupId="js-ts">
<TabItem label="JavaScript" value="js">

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  // 기타 설정
  apiToken: {
    secrets: {
      encryptionKey: env('ENCRYPTION_KEY'),
    },
  }
});
```

</TabItem>

<TabItem label="TypeScript" value="ts">

```js title="/config/admin.ts"
export default ({ env }) => ({
  // 기타 설정
  apiToken: {
    secrets: {
      encryptionKey: env('ENCRYPTION_KEY'),
    },
  }
});
```

</TabItem>
</Tabs>

이 키는 토큰 값을 암호화/복호화하는 데 사용됩니다. 이 키가 없으면 토큰은 계속 사용할 수 있지만, 최초 표시 이후에는 볼 수 없습니다. 새 Strapi 프로젝트는 이 키가 자동으로 생성됩니다.

## 사용법

API 토큰을 사용하면 [REST API](/cms/api/rest) 또는 [GraphQL API](/cms/api/graphql) 엔드포인트에서 인증된 사용자로 요청을 실행할 수 있습니다.

API 토큰은 사용자 계정을 별도로 관리하거나 Users & Permissions 플러그인 설정을 변경하지 않고도, 사람이나 애플리케이션에 접근 권한을 부여할 때 유용합니다.

Strapi의 REST API에 요청할 때는, API 토큰을 요청의 `Authorization` 헤더에 다음과 같은 형식으로 추가해야 합니다: `bearer your-api-token`.

:::note
읽기 전용 API 토큰은 `find` 및 `findOne` 기능만 접근할 수 있습니다.
:::
