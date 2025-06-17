---
title: 관리자 패널 구성
sidebar_label: 관리자 패널
displayed_sidebar: cmsSidebar
toc_max_heading_level: 2
description: Strapi의 관리자 패널은 구성을 위한 단일 진입점 파일을 제공합니다.
tags:
- 관리자 패널
- API 토큰
- 인증
- 기본 구성
- 구성
- 최소 구성
- 비밀번호
---

# 관리자 패널 구성

`/config/admin` 파일은 Strapi 애플리케이션의 [관리자 패널](/cms/features/admin-panel) 구성을 정의하는 데 사용됩니다.

이 페이지는 `/config/admin` 파일에서 찾을 수 있는 모든 구성 매개변수와 값들을 주제별로 그룹화하여 참조할 수 있도록 작성되었습니다. 각 기능의 작동 방식에 대한 추가 정보는 각 하위 섹션의 소개에서 제공되는 링크를 참조하세요.

## 관리자 패널 동작

관리자 패널 동작은 다음 매개변수로 구성할 수 있습니다:

| 매개변수                         | 설명                                                                                                                                                                                        | 타입          | 기본값                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `autoOpen`                        | 시작 시 관리자 패널 자동 열기를 활성화하거나 비활성화합니다.                                                                                                                                                 | boolean       | `true`                                                                                                                              |
| `watchIgnoreFiles`                | 개발 중에 감시하지 않을 커스텀 파일을 추가합니다.<br/><br/> 자세한 내용은 <ExternalLink to="https://github.com/paulmillr/chokidar#path-filtering" text="여기" />를 참조하세요 (`ignored` 속성).                                        | array(string) | `[]`                                                                                                                                |
| `serveAdminPanel`                 | false인 경우 관리자 패널이 서비스되지 않습니다.<br/><br/>참고: `index.html`은 여전히 서비스됩니다                                            | boolean       | `true`                                                                                                                              |

:::note config/admin vs. src/admin/app 구성
관리자 패널의 일부 UI 요소는 `src/admin/app` 파일에서 구성해야 합니다:

**튜토리얼 비디오**  
튜토리얼 비디오가 포함된 정보 박스를 비활성화하려면 `config.tutorials` 키를 `false`로 설정하세요.

**릴리즈 알림**  
새로운 Strapi 릴리즈에 대한 알림을 비활성화하려면 `config.notifications.releases` 키를 `false`로 설정하세요.

```js title="/src/admin/app.js"
const config = {
  // … 다른 커스터마이제이션 옵션들
  tutorials: false,
  notifications: { releases: false },
};

export default {
  config,
};
```

:::

## 관리자 패널 서버 

기본적으로 Strapi의 관리자 패널은 `http://localhost:1337/admin`을 통해 노출됩니다. 보안상의 이유로 호스트, 포트, 경로를 업데이트할 수 있습니다.


관리자 패널의 서버 구성은 다음 매개변수로 구성할 수 있습니다:

| 매개변수                         | 설명                                                                                                                                                                                        | 타입          | 기본값                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `url`                             | 관리자 패널에 접근하는 경로입니다. URL이 상대적이면 서버 URL과 연결됩니다.<br/><br/>예시: `/dashboard`는 관리자 패널을 `http://localhost:1337/dashboard`에서 접근 가능하게 만듭니다.                                                                                | string        | `/admin`                                                                                                                            |
| `host`                            | 관리자 패널 서버의 호스트입니다. | string        | `localhost`                                                                                                                         |
| `port`                            | 관리자 패널 서버의 포트입니다. | string        | `8000`                                                                                                                              |

:::note
`url` 옵션에 경로를 추가해도 애플리케이션에 접두사가 붙지 않습니다. 이를 위해서는 Nginx와 같은 프록시 서버를 사용하세요([선택적 소프트웨어 배포 가이드](/cms/deployment#additional-resources) 참고).
:::

### 관리자 패널의 경로만 업데이트

관리자 패널을 다른 경로에서 접근 가능하게 하려면, 예를 들어 `http://localhost:1337/dashboard`에서, `url` 속성을 정의하거나 업데이트하세요:

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  // … 다른 구성 속성들
  url: "/dashboard",
});
```

기본적으로 백엔드 서버와 관리자 패널 서버가 같은 호스트와 포트에서 실행되므로, 백엔드 [서버 구성](/cms/configurations/server) 파일에서 `host`와 `port` 속성 값을 그대로 두었다면 `config/admin` 파일만 업데이트하면 됩니다.

### 관리자 패널의 호스트와 포트 업데이트

관리자 패널 서버와 백엔드 서버가 같은 서버에 호스팅되지 않는 경우, 관리자 패널의 호스트와 포트를 업데이트해야 합니다. 예를 들어, 관리자 패널을 `my-host.com:3000`에서 호스팅하려면:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  host: "my-host.com",
  port: 3000,
  // 추가로 기본 /admin 대신 다른 경로를 정의할 수도 있습니다 👇
  // url: '/dashboard' 
});
```

</TabItem>
<TabItem value="ts" label="TypeScript">

```js title="/config/admin.ts"
export default ({ env }) => ({
  host: "my-host.com",
  port: 3000,
  // 추가로 기본 /admin 대신 다른 경로를 정의할 수도 있습니다 👇
  // url: '/dashboard'
});
```

</TabItem>
</Tabs>

### 다른 서버에 배포 {#deploy-on-different-servers}

Strapi의 백엔드 서버와 관리자 패널 서버를 다른 서버에 배포하지 않는 한, 기본적으로:
- 백엔드 서버와 관리자 패널 서버는 모두 같은 호스트와 포트에서 실행됩니다(`http://localhost:1337/`)
- 관리자 패널은 `/admin` 경로에서 접근 가능하고 백엔드 서버는 `/api` 경로에서 접근 가능합니다

관리자 패널과 백엔드를 완전히 다른 서버에 배포하려면, 서버(`/config/server`)와 관리자 패널(`/config/admin-panel`) 구성을 모두 설정해야 합니다.

다음 예시 설정을 통해 API가 다른 도메인에서 실행되는 동안 한 도메인에서 관리자 패널을 서비스할 수 있습니다:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/config/server.js"
module.exports = ({ env }) => ({
  host: env("HOST", "0.0.0.0"),
  port: env.int("PORT", 1337),
  url: "http://yourbackend.com",
});
```

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  /**
   * 참고: 관리자 패널이 도메인의 루트에서 접근 가능합니다
   * (예: http://yourfrontend.com/)
   */ 
  url: "/",
  serveAdminPanel: false, // http://yourbackend.com은 정적 관리자 파일을 서비스하지 않습니다
});
```

</TabItem>
<TabItem value="ts" label="TypeScript">

```js title="/config/server.ts"
export default ({ env }) => ({
  host: env("HOST", "0.0.0.0"),
  port: env.int("PORT", 1337),
  url: "http://yourbackend.com",
});
```

```js title="/config/admin.ts"
export default ({ env }) => ({
  /**
   * 참고: 관리자 패널이 도메인의 루트에서 접근 가능합니다
   * (예: http://yourfrontend.com/)
   */ 
  url: "/",
  serveAdminPanel: false, // http://yourbackend.com은 정적 관리자 파일을 서비스하지 않습니다
});
```

</TabItem>
</Tabs>

이 구성으로:
- 관리자 패널은 `http://yourfrontend.com`에서 접근 가능합니다
- 패널의 모든 API 요청은 `http://yourbackend.com`으로 전송됩니다
- `serveAdminPanel: false`로 인해 백엔드 서버는 정적 관리자 파일을 서비스하지 않습니다

## API 토큰

[API 토큰](/cms/features/api-tokens) 기능은 다음 매개변수로 구성할 수 있습니다:

| 매개변수                         | 설명                                                                                                                                                                                        | 타입          | 기본값                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `apiToken.salt`                   | API 토큰 생성에 사용되는 솔트                                                                                                                            | string        | 랜덤 문자열                                                                                                                       |
| `apiToken.secrets.encryptionKey`   | 관리자 패널에서 API 토큰 가시성을 설정하는 데 사용되는 암호화 키 | string | 랜덤 문자열 |

## 감사 로그

[감사 로그](/cms/features/audit-logs) 기능은 다음 매개변수로 구성할 수 있습니다:

| 매개변수                         | 설명                                                                                                                                                                                        | 타입          | 기본값                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `auditLogs.enabled`               | 감사 로그 기능을 활성화하거나 비활성화합니다                                                                                                                         | boolean       | `true`                                                                                                                              |
| `auditLogs.retentionDays`         | 감사 로그가 보관되는 기간(일 단위)입니다.<br /><br />_자체 호스팅 vs. Strapi Cloud 고객에 대한 동작이 다릅니다. 표 아래 참고사항을 참조하세요._               | integer       | 90                                                                                                                                  |

:::note 자체 호스팅 vs. Strapi Cloud 사용자의 보관 일수
Strapi Cloud 고객의 경우, `config/admin.js|ts` 구성 파일에서 _더 작은_ `retentionDays` 값이 정의되지 않는 한, 라이선스 정보에 저장된 `auditLogs.retentionDays` 값이 사용됩니다.
:::

## Authentication

The authentication system, including [SSO configuration](/cms/configurations/guides/configure-sso), can be configured with the following parameters:

| Parameter                         | Description                                                                                                                                                                                        | Type          | Default                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `auth`                            | Authentication configuration                                                                                                                                                                       | object        | -                                                                                                                                   |
| `auth.secret`                     | Secret used to encode JWT tokens                                                                                                                                                                   | string        | `undefined`                                                                                                                         |
| `auth.domain`                     | Domain used within the cookie for SSO authentication <EnterpriseBadge /> <SsoBadge />)                                                                                                                             | string        | `undefined`                                                                                                                         |
| `auth.providers`                  | List of authentication providers used for SSO                                                                                           | array(object) | -                                                                                                                                   |
| `auth.options`                    | Options object passed to jsonwebtoken                                                                                                                | object        | -                                                                                                                                   |
| `auth.options.expiresIn`          | JWT expire time used in jsonwebtoken                                                                                                                 | object        | `30d`                                                                                                                               |
| `auth.events`                     | Record of all the events subscribers registered for the authentication                                                                                                                             | object        | `{}`                                                                                                                                |
| `auth.events.onConnectionSuccess` | Function called when an admin user log in successfully to the administration panel                                                                                                                 | function      | `undefined`                                                                                                                         |
| `auth.events.onConnectionError`   | Function called when an admin user fails to log in to the administration panel                                                                                                                     | function      | `undefined`                                                                                                                         |

## Feature flags

The feature flags can be configured with the following parameters:

| Parameter                         | Description                                                                                                                                                                                        | Type          | Default                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `flags`                           | Settings to turn certain features or elements of the admin on or off                                                                                                                               | object        | {}                                                                                                                                  |
| `flags.nps`                       | Enable/Disable the Net Promoter Score popup                                                                                                                                                        | boolean       | `true`                                                                                                                              |
| `flags.promoteEE`                 | Enable/Disable the promotion of Strapi Enterprise features                                                                                                                                         | boolean       | `true`                                                                                                                              |

## Forgot password

The forgot password functionality, including [email templating](/cms/features/users-permissions#templating-emails), can be configured with the following parameters:

| Parameter                         | Description                                                                                                                                                                                        | Type          | Default                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `forgotPassword`                  | Settings to customize the forgot password email                                                        | object        | {}                                                                                                                                  |
| `forgotPassword.emailTemplate`    | Email template as defined in email plugin                                                                                         | object        | Default template |
| `forgotPassword.from`             | Sender mail address                                                                                                                                                                                | string        | Default value defined in <br />your provider configuration                             |
| `forgotPassword.replyTo`          | Default address or addresses the receiver is asked to reply to                                                                                                                                     | string        | Default value defined in <br />your provider configuration                             |

## Rate limiting

The rate limiting for the admin panel's authentication endpoints can be configured with the following parameters. Additional configuration options come from the <ExternalLink text="koa2-ratelimit" to="https://www.npmjs.com/package/koa2-ratelimit"/> package:

| Parameter                         | Description                                                                                                                                                                                        | Type          | Default                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `rateLimit`                       | Settings to customize the rate limiting of the admin panel's authentication endpoints | object        | {}                                                                                                                                  |
| `rateLimit.enabled`               | Enable or disable the rate limiter                                                                                                                                                                 | boolean       | `true`                                                                                                                              |
| `rateLimit.interval`              | Time window for requests to be considered as part of the same rate limiting bucket                                                                                                                 | object        | `{ min: 5 }`                                                                                                                        |
| `rateLimit.max`                   | Maximum number of requests allowed in the time window                                                                                                                                              | integer       | `5`                                                                                                                                 |
| `rateLimit.delayAfter`            | Number of requests allowed before delaying responses                                                                                                                                               | integer       | `1`                                                                                                                                 |
| `rateLimit.timeWait`              | Time to wait before responding to a request (in milliseconds)                                                                                                                                      | integer       | `3000`                                                                                                                              |
| `rateLimit.prefixKey`             | Prefix for the rate limiting key                                                                                                                                                                   | string        | `${userEmail}:${ctx.request.path}:${ctx.request.ip}`                                                                                |
| `rateLimit.whitelist`             | Array of IP addresses to whitelist from rate limiting                                                                                                                                              | array(string) | `[]`                                                                                                                                |
| `rateLimit.store`                 | Rate limiting storage location (Memory, Sequelize, or Redis). For more information see the koa2-ratelimit documentation               | object        | `MemoryStore`                                                                                                                       |

## Transfer tokens

Transfer tokens for the [Data transfer](/cms/data-management/transfer) feature can be configured with the following parameters:

| Parameter                         | Description                                                                                                                                                                                        | Type          | Default                                                                                                                             |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `transfer.token.salt`             | Salt used to generate Transfer tokens.<br/><br/>If no transfer token salt is defined, transfer features will be disabled.               | string        | a random string                                                                                                                       |

:::note Retention days for self-hosted vs. Strapi Cloud users
For Strapi Cloud customers, the `auditLogs.retentionDays` value stored in the license information is used, unless a _smaller_ `retentionDays` value is defined in the `config/admin.js|ts` configuration file.
:::

## Configuration examples

The `/config/admin` file should at least include a minimal configuration with required parameters for [authentication](#authentication) and [API tokens](#api-tokens). Additional parameters can be included for a full configuration.

:::note
[Environmental configurations](/cms/configurations/environment.md) (i.e. using the `env()` helper) do not need to contain all the values so long as they exist in the default `/config/server`.
:::

<Tabs>
<TabItem value="minimal config" label="Minimal configuration">

The default configuration created with any new project should at least include the following:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  apiToken: {
    salt: env('API_TOKEN_SALT', 'someRandomLongString'),
  },
  auditLogs: { // only accessible with an Enterprise plan
    enabled: env.bool('AUDIT_LOGS_ENABLED', true),
  },
  auth: {
    secret: env('ADMIN_JWT_SECRET', 'someSecretKey'),
  },
  transfer: { 
    token: { 
      salt: env('TRANSFER_TOKEN_SALT', 'anotherRandomLongString'),
    } 
  },
});

```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/admin.ts"

export default ({ env }) => ({
  apiToken: {
    salt: env('API_TOKEN_SALT', 'someRandomLongString'),
  },
   auditLogs: { // only accessible with an Enterprise plan
    enabled: env.bool('AUDIT_LOGS_ENABLED', true),
  },
  auth: {
    secret: env('ADMIN_JWT_SECRET', 'someSecretKey'),
  },
  transfer: { 
    token: { 
      salt: env('TRANSFER_TOKEN_SALT', 'anotherRandomLongString'),
    } 
  },
});
```

</TabItem>

</Tabs>
</TabItem>

<TabItem value="full config" label="Full configuration">

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/admin.js"

module.exports = ({ env }) => ({
  apiToken: {
    salt: env('API_TOKEN_SALT', 'someRandomLongString'),
    secrets: {
      encryptionKey: env('ENCRYPTION_KEY'),
    },
  },
  auditLogs: { // only accessible with an Enterprise plan
    enabled: env.bool('AUDIT_LOGS_ENABLED', true),
    retentionDays: 120,
  },
  auth: {
    events: {
      onConnectionSuccess(e) {
        console.log(e.user, e.provider);
      },
      onConnectionError(e) {
        console.error(e.error, e.provider);
      },
    },
    options: {
      expiresIn: '7d',
    },
    secret: env('ADMIN_JWT_SECRET', 'someSecretKey'),
  },
  url: env('PUBLIC_ADMIN_URL', '/dashboard'),
  autoOpen: false,
  watchIgnoreFiles: [
    './my-custom-folder', // Folder
    './scripts/someScript.sh', // File
  ],
  host: 'localhost',
  port: 8003,
  serveAdminPanel: env.bool('SERVE_ADMIN', true),
  forgotPassword: {
    from: 'no-reply@example.com',
    replyTo: 'no-reply@example.com',
  },
  rateLimit: {
    interval: { hour: 1, min: 30 },
    timeWait: 3*1000,
    max: 10,
  },
  transfer: { 
    token: { 
      salt: env('TRANSFER_TOKEN_SALT', 'anotherRandomLongString'),
    } 
  },
});

```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/admin.ts"

export default ({ env }) => ({
  apiToken: {
    salt: env('API_TOKEN_SALT', 'someRandomLongString'),
    secrets: {
      encryptionKey: env('ENCRYPTION_KEY'),
    },
  },
  auditLogs: { // only accessible with an Enterprise plan
    enabled: env.bool('AUDIT_LOGS_ENABLED', true),
    retentionDays: 120,
  },
  auth: {
    events: {
      onConnectionSuccess(e) {
        console.log(e.user, e.provider);
      },
      onConnectionError(e) {
        console.error(e.error, e.provider);
      },
    },
    options: {
      expiresIn: '7d',
    },
    secret: env('ADMIN_JWT_SECRET', 'someSecretKey'),
  },
  url: env('PUBLIC_ADMIN_URL', '/dashboard'),
  autoOpen: false,
  watchIgnoreFiles: [
    './my-custom-folder', // Folder
    './scripts/someScript.sh', // File
  ],
  host: 'localhost',
  port: 8003,
  serveAdminPanel: env.bool('SERVE_ADMIN', true),
  forgotPassword: {
    from: 'no-reply@example.com',
    replyTo: 'no-reply@example.com',
  },
  rateLimit: {
    interval: { hour: 1, min: 30 },
    timeWait: 3*1000,
    max: 10,
  },
  transfer: { 
    token: { 
      salt: env('TRANSFER_TOKEN_SALT', 'anotherRandomLongString'),
    } 
  },
});
```

</TabItem>

</Tabs>
</TabItem>
</Tabs>