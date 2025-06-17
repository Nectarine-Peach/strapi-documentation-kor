---
title: 사용자 및 권한(Users & Permissions)
description: 엔드유저 관리를 위한 Users & Permissions 및 API 토큰 기능 사용법을 알아보세요.
toc_max_heading_level: 5
tags:
- admin panel
- users & permissions
- api tokens
- features
---

# 사용자 및 권한(Users & Permissions)

Users & Permissions 기능을 사용하면 Strapi 프로젝트의 엔드유저<Annotation>💡 **엔드유저란?** <br/> 엔드유저는 Strapi 애플리케이션으로 생성·관리된 콘텐츠를 웹사이트, 모바일 앱, 기타 디바이스 등에서 소비하는 사용자입니다. 관리자는 아니며, 관리자 패널 접근 권한이 없습니다.</Annotation>를 관리할 수 있습니다. 이 기능은 JSON Web Token(JWT) 기반의 인증 프로세스와, 사용자 그룹별 권한을 관리할 수 있는 ACL(Access Control List) 전략을 제공합니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">Roles > Plugins - Users & Permissions에서 CRUD 권한 필요</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본적으로 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

## 관리자 패널 설정

Users & Permissions 기능은 관리자 패널 설정과 코드 기반 설정 모두에서 관리할 수 있습니다.

### 역할

Users & Permissions 기능을 사용하면 엔드유저 역할을 생성 및 관리하고, 각 역할별로 접근 권한을 설정할 수 있습니다.

#### 새 역할 생성

**기능 경로:** <Icon name="gear-six" /> *Users & Permissions 플러그인 > 역할*

*역할* 화면 우측 상단의 **새 역할 추가** 버튼을 클릭하면 엔드유저 역할을 새로 만들 수 있습니다.

버튼을 클릭하면 역할 편집 화면으로 이동하며, 여기서 역할의 이름, 설명, 권한을 설정할 수 있습니다([역할 편집](#editing-a-role) 참고).

<ThemedImage
  alt="엔드유저 역할 인터페이스"
  sources={{
    light: '/img/assets/users-permissions/end-user_roles.png',
    dark: '/img/assets/users-permissions/end-user_roles_DARK.png',
  }}
/>

:::note
새로 가입한 엔드유저에게 기본으로 할당되는 역할은 *Users & Permissions 플러그인*의 *고급 설정*에서 지정할 수 있습니다([고급 설정](#advanced-settings) 참고).
:::

#### 역할 편집

**기능 경로:** <Icon name="gear-six" /> *Users & Permissions 플러그인 > 역할*

*역할* 화면에는 Strapi 애플리케이션의 모든 엔드유저 역할이 표시됩니다.

기본적으로 2가지 엔드유저 역할이 제공됩니다:

- Authenticated: 로그인한 엔드유저만 콘텐츠 접근 가능
- Public: 로그인하지 않아도 콘텐츠 접근 가능

추가 역할을 생성할 수도 있으며([새 역할 생성](#creating-a-new-role)), 모든 역할은 편집 화면에서 수정할 수 있습니다.

1. 편집할 역할의 <Icon name="pencil-simple" /> 버튼을 클릭합니다(또는 새 역할 생성 후 자동 이동).
2. *역할 세부 정보*를 입력합니다:

| 역할 세부 정보 | 설명 |
| ------------- | ---------------------------------------- |
| 이름 | 역할의 이름을 입력합니다. |
| 설명 | 역할에 대한 설명을 입력합니다. (관리자가 역할 권한을 쉽게 이해할 수 있도록) |

3. *권한*을 설정합니다:
    1. 권한 카테고리(예: Application, Content-Manager, Email 등)를 클릭합니다.
    2. 역할에 부여할 작업 및 권한의 체크박스를 선택합니다.
4. **저장** 버튼을 클릭합니다.

:::tip
작업/권한 체크박스를 선택하면, 해당 API의 관련 라우트가 화면 오른쪽에 표시됩니다.
:::

<ThemedImage
  alt="엔드유저 역할 권한 설정"
  sources={{
    light: '/img/assets/users-permissions/end-user_roles-config.png',
    dark: '/img/assets/users-permissions/end-user_roles-config_DARK.png',
  }}
/>

#### 역할 삭제

**기능 경로:** <Icon name="gear-six" /> *Users & Permissions 플러그인 > 역할*

기본 제공되는 2가지 역할은 삭제할 수 없지만, 그 외 역할은 해당 역할이 할당된 엔드유저가 없을 때 삭제할 수 있습니다.

1. 삭제할 역할 오른쪽의 <Icon name="trash" /> 버튼을 클릭합니다.
2. 삭제 창에서 <Icon name="trash" /> **확인** 버튼을 클릭합니다.

### 제공업체(Providers)

**기능 경로:** <Icon name="gear-six" /> *Users & Permissions 플러그인 > 제공업체*

Users & Permissions 기능을 사용하면, 엔드유저가 외부 제공업체를 통해 로그인할 수 있도록 제공업체를 활성화 및 설정할 수 있습니다.

기본적으로 "이메일" 제공업체가 활성화되어 있습니다.

1. 활성화 및 설정할 제공업체의 <Icon name="pencil-simple" /> 버튼을 클릭합니다.
2. 제공업체 편집 창에서 *활성화* 옵션을 **TRUE**로 설정합니다.
3. 제공업체별 설정을 입력합니다. 각 제공업체마다 설정 항목이 다릅니다([Users & Permissions 제공업체 문서](/cms/configurations/users-and-permissions-providers#setting-up-the-provider---examples) 참고).
4. **저장** 버튼을 클릭합니다.

<ThemedImage
  alt="제공업체 인터페이스"
  sources={{
    light: '/img/assets/settings/up_providers.png',
    dark: '/img/assets/settings/up_providers_DARK.png',
  }}
/>

기본 제공업체 외에, Strapi에서 제공하지 않는 외부 제공업체도 직접 추가할 수 있습니다. 아래 카드에서 자세한 설정 방법을 확인하세요:

<CustomDocCardsWrapper>
<CustomDocCard icon="question" title="제공업체 설정 방법" description="Users & Permissions 제공업체의 동작 원리, 로그인 플로우, 예시를 알아보세요." link="/cms/configurations/users-and-permissions-providers" />
<CustomDocCard icon="list-plus" title="커스텀 제공업체 만들기" description="Users & Permissions 기능에 사용할 커스텀 제공업체를 직접 만드는 방법을 알아보세요." link="/cms/configurations/users-and-permissions-providers/new-provider-guide" />
</CustomDocCardsWrapper>

### 이메일 템플릿

**기능 경로:** <Icon name="gear-six" /> *Users & Permissions 플러그인 > 이메일 템플릿*

Users & Permissions 기능은 "이메일 주소 확인"과 "비밀번호 재설정" 두 가지 이메일 템플릿을 사용합니다. 이 템플릿은 다음 상황에서 엔드유저에게 발송됩니다:

- 계정 활성화를 위해 이메일 확인이 필요한 경우
- 비밀번호 재설정이 필요한 경우

두 템플릿 모두 수정할 수 있습니다.

1. 편집할 이메일 템플릿의 <Icon name="pencil-simple" /> 버튼을 클릭합니다.
2. 이메일 템플릿을 설정합니다:

| 설정명 | 설명 |
| ------ | ----------------------------------------------- |
| 발신자 이름 | 이메일 발신자 이름 입력 |
| 발신자 이메일 | 이메일 발신자 주소 입력 |
| 회신 이메일 | (선택) 엔드유저가 회신할 이메일 주소 입력 |
| 제목 | 이메일 제목 입력(변수 사용 가능, [이메일 템플릿화](#templating-emails) 참고) |

3. "메시지" 텍스트박스에서 이메일 본문(HTML, 변수 사용 가능)을 수정합니다.
4. **완료** 버튼을 클릭합니다.

<ThemedImage
  alt="이메일 템플릿 인터페이스"
  sources={{
    light: '/img/assets/settings/up_email-templates.png',
    dark: '/img/assets/settings/up_email-templates_DARK.png',
  }}
/>

### 고급 설정

**기능 경로:** <Icon name="gear-six" /> *Users & Permissions 플러그인 > 고급 설정*

Users & Permissions 기능의 모든 세부 설정은 *고급 설정* 화면에서 관리합니다. 여기서 엔드유저의 기본 역할, 회원가입 및 이메일 인증 활성화, 비밀번호 재설정 페이지 등 다양한 옵션을 설정할 수 있습니다.

1. 원하는 설정을 입력합니다:

| 설정명 | 설명 |
| ------ | --------------------------------------------------------------|
| 인증된 사용자의 기본 역할 | 새 엔드유저에게 기본으로 할당할 역할을 드롭다운에서 선택 |
| 이메일당 1계정 제한 | **TRUE**로 설정 시 동일 이메일로 여러 계정 생성 불가, **FALSE**로 설정 시 여러 계정 허용 |
| 회원가입 허용 | **TRUE**로 설정 시 엔드유저가 직접 회원가입 가능, **FALSE**로 설정 시 회원가입 불가 |
| 비밀번호 재설정 페이지 | 프론트엔드의 비밀번호 재설정 페이지 URL 입력 |
| 이메일 인증 활성화 | **TRUE**로 설정 시 엔드유저 계정 활성화 시 이메일 인증 필요, **FALSE**로 설정 시 인증 불필요 |
| 인증 후 리디렉션 URL | 엔드유저가 계정 인증 후 이동할 페이지의 URL 입력 |

2. **저장** 버튼을 클릭합니다.

<ThemedImage
  alt="고급 설정 인터페이스"
  sources={{
    light: '/img/assets/settings/up_settings.png',
    dark: '/img/assets/settings/up_settings_DARK.png',
  }}
/>

## 코드 기반 설정

대부분의 Users & Permissions 설정은 관리자 패널에서 관리하지만, 일부 고급 설정은 Strapi 프로젝트의 코드를 통해 세밀하게 조정할 수 있습니다.

### JWT 설정

[플러그인 설정 파일](/cms/configurations/plugins)에서 JWT 생성 방식을 설정할 수 있습니다.

Strapi는 <ExternalLink to="https://www.npmjs.com/package/jsonwebtoken" text="jsonwebtoken"/> 패키지를 사용해 JWT를 생성합니다.

사용 가능한 옵션:

- `jwtSecret`: JWT 생성에 사용되는 임의 문자열(보통 `JWT_SECRET` [환경 변수](/cms/configurations/environment#strapi)로 설정)
- `jwt.expiresIn`: 만료 시간(초 또는 문자열, 예: 60, "45m", "10h", "2 days", "7d", "2y"). 숫자는 초 단위, 문자열은 시간 단위(단위 미지정 시 ms)

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"

module.exports = ({ env }) => ({
  // ...
  'users-permissions': {
    config: {
      jwt: {
        expiresIn: '7d',
      },
    },
  },
  // ...
});
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/plugins.ts"

export default ({ env }) => ({
  // ...
  'users-permissions': {
    config: {
      jwt: {
        expiresIn: '7d',
      },
    },
  },
  // ...
});
```

</TabItem>

</Tabs>

:::warning
JWT 만료 시간을 30일 이상으로 설정하는 것은 보안상 권장하지 않습니다.
:::

### 회원가입 설정

User **모델**<Annotation>모델(또는 콘텐츠 타입)은 Strapi에서 콘텐츠 구조를 정의합니다.<br/>User는 모든 Strapi 프로젝트에 기본 제공되는 특수 콘텐츠 타입입니다. 다른 모델과 마찬가지로 필드를 추가하는 등 커스터마이즈할 수 있습니다.<br/>자세한 내용은 [모델](/cms/backend-customization/models) 문서를 참고하세요.</Annotation>에 회원가입 시 추가로 받아야 할 필드를 추가했다면, 해당 필드를 `/config/plugins` 파일의 `config.register.allowedFields`에 명시해야 회원가입 API에서 허용됩니다.

예시: "nickname" 필드를 회원가입 시 허용하려면 아래와 같이 설정합니다.

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"
module.exports = ({ env }) => ({
  // ...
  "users-permissions": {
    config: {
      register: {
        allowedFields: ["nickname"],
      },
    },
  },
  // ...
});
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/plugins.ts"
export default ({ env }) => ({
  // ...
  "users-permissions": {
    config: {
      register: {
        allowedFields: ["nickname"],
      },
    },
  },
  // ...
});
```

</TabItem>

</Tabs>

### 이메일 템플릿화

이 플러그인은 기본적으로 두 가지 템플릿(비밀번호 재설정, 이메일 주소 확인)을 제공합니다. 템플릿은 <ExternalLink to="https://lodash.com/docs/4.17.15#template" text="Lodash의 template() 메서드"/>를 사용해 변수를 치환합니다.

이메일 템플릿은 **플러그인 > Roles & Permissions > 이메일 템플릿** 탭에서 수정할 수 있습니다([이메일 템플릿 설정](#email-templates) 참고).

사용 가능한 변수:

<Tabs>
<TabItem value="reset-password" label="비밀번호 재설정">

<br/>
- `USER` (object)
  - `username`
  - `email`
- `TOKEN`: 비밀번호 재설정용 토큰
- `URL`: 이메일 내 클릭 시 이동할 링크
- `SERVER_URL`: 서버의 절대 URL

</TabItem>

<TabItem value="email-address-confirmation" label="이메일 주소 확인">

<br/>
- `USER` (object)
  - `username`
  - `email`
- `CODE`: 이메일 인증용 코드
- `URL`: 인증 코드 확인용 Strapi 백엔드 URL(기본값 `/auth/email-confirmation`)
- `SERVER_URL`: 서버의 절대 URL

</TabItem>
</Tabs>

### 보안 설정

JWT는 디지털 서명으로 검증 및 신뢰할 수 있습니다. 서명에는 _secret_이 필요합니다. 기본적으로 Strapi는 `/extensions/users-permissions/config/jwt.js`에 secret을 생성·저장합니다.

개발 환경에서는 기본값을 사용해도 되지만, 운영 환경에서는 보안을 위해 환경 변수 `JWT_SECRET`을 직접 지정하는 것이 좋습니다.

기본적으로 `JWT_SECRET` 환경 변수를 지정하면 해당 값이 secret으로 사용됩니다. 다른 환경 변수를 사용하려면 아래와 같이 설정 파일을 수정하세요.

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/extensions/users-permissions/config/jwt.js"

module.exports = {
  jwtSecret: process.env.SOME_ENV_VAR,
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/extensions/users-permissions/config/jwt.ts"

export default {
  jwtSecret: process.env.SOME_ENV_VAR,
};
```

</TabItem>

</Tabs>

#### 커스텀 콜백 검증기 생성 {#creating-a-custom-password-validation}

기본적으로 Strapi SSO는 설정에 명시된 URL과 정확히 일치하는 리디렉션 URL만 허용합니다:

<ThemedImage
  alt="Users & Permissions 설정"
  sources={{
      light: '/img/assets/users-permissions/sso-config-custom-validator.png',
      dark: '/img/assets/users-permissions/sso-config-custom-validator_DARK.png'
    }}
/>

다른 URL도 허용하려면, `users-permissions` 플러그인의 plugins.js에 콜백 `validate` 함수를 추가할 수 있습니다.

```tsx title="/config/plugins.js|ts"
  // ... 기타 플러그인 설정 ...
  // Users & Permissions 설정
  'users-permissions': {
    enabled: true,
    config: {
      callback: {
        validate: (cbUrl, options) => {
          // cbUrl: 인증 제공업체에서 리디렉션 요청한 URL

          // 아래 예시는 특정 도메인만 허용
          if (cbUrl.startsWith('https://myproxy.mysite.com/') || 
              cbUrl.startsWith('https://mysite.com/')) {
            return;
          }

          // 허용하지 않는 경우 반드시 에러를 throw해야 함
          throw new Error('Invalid callback url');
        },
      },
    },
  },
```

## 사용법

Users & Permissions 기능은 관리자 패널에서 엔드유저 계정을 생성하거나, API를 통해 사용할 수 있습니다.

### 관리자 패널 사용법

**기능 경로:** <Icon name="feather" /> 콘텐츠 매니저

Users & Permissions 기능이 설치된 Strapi 애플리케이션에는 "User"를 포함한 3가지 컬렉션 타입이 자동 생성되며, 이 중 "User"만 콘텐츠 매니저에서 직접 관리할 수 있습니다.

<ThemedImage
  alt="콘텐츠 매니저에서 엔드유저 관리"
  sources={{
    light: '/img/assets/users-permissions/end-user_content-manager.png',
    dark: '/img/assets/users-permissions/end-user_content-manager_DARK.png',
  }}
/>

Users & Permissions 플러그인을 사용하는 프론트엔드에서 엔드유저를 등록하면, User 컬렉션 타입에 새 엔트리가 추가됩니다.

1. <Icon name="feather" /> 콘텐츠 매니저에서 User 컬렉션 타입으로 이동합니다.
2. 우측 상단의 **새 엔트리 생성** 버튼을 클릭합니다.
3. 기본 필드를 입력합니다(관리자가 추가한 필드가 있다면 함께 표시됨):

| 필드 | 설명 |
| ---- | ---------------------------- |
| 사용자명 | 엔드유저의 사용자명 입력 |
| 이메일 | 엔드유저의 이메일 입력 |
| 비밀번호 | (선택) 새 비밀번호 입력(눈 아이콘 클릭 시 표시됨) |
| 인증됨 | (선택) **ON**으로 설정 시 계정 인증됨 처리 |
| 차단됨 | (선택) **ON**으로 설정 시 계정 차단(콘텐츠 접근 불가) |
| 역할 | (선택) 새 엔드유저에게 부여할 역할(미입력 시 기본 역할 자동 할당) |

4. **저장** 버튼을 클릭합니다.

:::note
엔드유저가 프론트엔드에서 직접 회원가입할 수 있도록 설정되어 있다면([고급 설정](#advanced-settings) 참고), 새 엔트리가 자동으로 생성되고 입력값이 반영됩니다. 모든 필드는 관리자가 수정할 수 있습니다.
:::

### API 사용법

API 요청 시마다 서버는 `Authorization` 헤더를 확인하여, 요청 사용자가 해당 리소스에 접근할 권한이 있는지 검증합니다.

:::note
역할이 없는 사용자 또는 `/api/auth/local/register` 경로로 가입한 사용자는 기본적으로 authenticated 역할이 부여됩니다.
:::

#### 식별자(Identifier)

`identifier` 파라미터는 이메일 또는 사용자명 모두 사용할 수 있습니다.

<Tabs>

<TabItem value="Axios" title="Axios">

```js
import axios from 'axios';

// API 요청 예시
axios
  .post('http://localhost:1337/api/auth/local', {
    identifier: 'user@strapi.io',
    password: 'strapiPassword',
  })
  .then(response => {
    // 성공 처리
    console.log('Well done!');
    console.log('User profile', response.data.user);
    console.log('User token', response.data.jwt);
  })
  .catch(error => {
    // 에러 처리
    console.log('An error occurred:', error.response);
  });
```

</TabItem>

<TabItem value="Postman" title="Postman">

Postman을 사용할 경우, **body**를 **raw**로 설정하고 **JSON** 형식으로 입력하세요:

```json
{
  "identifier": "user@strapi.io",
  "password": "strapiPassword"
}
```

요청이 성공하면 **user의 JWT**가 `jwt` 키로 반환됩니다:

```json
{
    "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNTc2OTM4MTUwLCJleHAiOjE1Nzk1MzAxNTB9.UgsjjXkAZ-anD257BF7y1hbjuY3ogNceKfTAQtzDEsU",
    "user": {
        "id": 1,
        "username": "user",
        ...
    }
}
```

</TabItem>
</Tabs>

#### 토큰 사용법

`jwt`는 권한이 필요한 API 요청에 사용할 수 있습니다. 사용자로서 API 요청을 하려면, `GET` 요청의 `Authorization` 헤더에 JWT를 추가하세요.

토큰이 없는 요청은 기본적으로 public 역할 권한으로 처리됩니다. 각 역할별 권한은 관리자 대시보드에서 수정할 수 있습니다.

인증 실패 시 401(unauthorized) 에러가 반환됩니다.

`token` 변수는 로그인 또는 회원가입 시 받은 `data.jwt` 값입니다.

```js
import axios from 'axios';

const token = 'YOUR_TOKEN_HERE';

// API 요청 예시
axios
  .get('http://localhost:1337/posts', {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  })
  .then(response => {
    // 성공 처리
    console.log('Data: ', response.data);
  })
  .catch(error => {
    // 에러 처리
    console.log('An error occurred:', error.response);
  });
```

#### 사용자 회원가입

기본 역할이 'registered'인 새 사용자를 데이터베이스에 생성하는 예시:

```js
import axios from 'axios';

// API 요청 예시
// public 사용자가 회원가입할 수 있도록 제한/커스터마이즈하려면 아래 코드를 참고하세요.
axios
  .post('http://localhost:1337/api/auth/local/register', {
    username: 'Strapi user',
    email: 'user@strapi.io',
    password: 'strapiPassword',
  })
  .then(response => {
    // 성공 처리
    console.log('Well done!');
    console.log('User profile', response.data.user);
    console.log('User token', response.data.jwt);
  })
  .catch(error => {
    // 에러 처리
    console.log('An error occurred:', error.response);
  });
```

#### Strapi 컨텍스트의 user 객체

인증에 성공한 요청에서는 `user` 객체가 `ctx.state`에 포함되어 있습니다.

```js
create: async ctx => {
  const { id } = ctx.state.user;

  const depositObj = {
    ...ctx.request.body,
    depositor: id,
  };

  const data = await strapi.services.deposit.add(depositObj);

  // 201 created 반환
  ctx.created(data);
};
```