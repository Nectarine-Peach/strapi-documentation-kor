---
title: 이메일
displayed_sidebar: cmsSidebar
toc_max_heading_level: 5
description: 서버 또는 외부 제공업체를 통해 이메일을 발송하세요.
tags:
- admin panel
- controllers 
- email
- lifecycle hooks
- services
- features
---

# 이메일

이메일 기능을 사용하면 Strapi 애플리케이션에서 서버 또는 외부 제공업체를 통해 이메일을 발송할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">이메일 > "send" 권한이 있어야 백엔드 서버를 통해 이메일 발송 가능</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본적으로 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

## 설정

이메일 기능의 대부분의 설정 옵션은 Strapi 프로젝트의 코드에서 처리됩니다. 이메일 기능은 관리자 패널에서 별도의 설정이 불가능하지만, 관리자가 이메일 발송이 정상적으로 동작하는지 테스트할 수 있습니다.

### 관리자 패널 설정

**기능 설정 경로:** <Icon name="gear-six" /> 설정 > 이메일 기능 > 설정

<ThemedImage
  alt="이메일 설정"
  sources={{
    light: '/img/assets/settings/settings-email.png',
    dark: '/img/assets/settings/settings-email_DARK.png',
  }}
/>

설정 화면에서는 "테스트 이메일 발송" 항목의 이메일 주소만 수정할 수 있습니다. **테스트 이메일 발송** 버튼을 누르면 테스트 메일이 전송됩니다.

이 페이지는 현재 역할에 "이메일 설정 페이지 접근" 권한이 있을 때만 표시됩니다([RBAC 기능](/cms/features/rbac) 문서 참고):

<ThemedImage
  alt="이메일 설정"
  sources={{
    light: '/img/assets/settings/settings-email-config-role.png',
    dark: '/img/assets/settings/settings-email-config-role_DARK.png',
  }}
/>

### 코드 기반 설정

이메일 기능을 사용하려면 `config/plugins.js|ts` 파일에 제공업체(provider)와 제공업체 설정을 추가해야 합니다. 자세한 설치 및 설정 방법은 [제공업체](#providers) 항목을 참고하세요.

<ExternalLink to="https://www.npmjs.com/package/sendmail" text="Sendmail"/>은 Strapi 이메일 기능의 기본 제공업체입니다. 로컬 개발 환경에서는 기본 설정으로 동작하지만, 운영 환경에서는 추가 설정이 필요하거나 다른 제공업체로 변경해야 합니다.

#### 이메일 설정 옵션

플러그인 설정은 `config/plugins.js` 또는 `config/plugins.ts` 파일에 정의합니다. 각 제공업체별 설치 및 설정 방법은 [제공업체](#providers) 항목을 참고하세요.

| 옵션                    | 타입            | 설명                                                                                                                                            | 기본값  | 비고    |
|-------------------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| `provider`              | `string`        | 사용할 이메일 제공업체                                                                                                                          | `sendmail` | 필수 |
| `providerOptions`       | `object`        | 이메일 제공업체 옵션                                                                                                                           | `{}`    | 선택 |
| `providerOptions.apiKey`| `string`        | 이메일 제공업체의 API 키                                                                                                                        | `''`    | 선택 |
| `settings`              | `object`        | 이메일 설정                                                                                                                                    | `{}`    | 선택 |
| `settings.defaultFrom`  | `string`        | 기본 발신자 이메일 주소                                                                                                                        | `''`    | 선택 |
| `settings.defaultReplyTo`| `string`       | 기본 회신 이메일 주소                                                                                                                          | `''`    | 선택 |
| `ratelimit`             | `object`        | 이메일 발송 속도 제한 설정                                                                                                                     | `{}`    | 선택 |
| `ratelimit.enabled`     | `boolean`       | 속도 제한 활성화 여부                                                                                                                          | `true`  | 선택 |
| `ratelimit.interval`    | `string`        | 속도 제한 간격(분 단위)                                                                                                                        | `5`     | 선택 |
| `ratelimit.max`         | `number`        | 간격 내 허용 요청 최대 수                                                                                                                      | `5`     | 선택 |
| `ratelimit.delayAfter`  | `number`        | 속도 제한 적용 전 허용 요청 수                                                                                                                 | `1`     | 선택 |
| `ratelimit.timeWait`    | `number`        | 요청 응답 전 대기 시간(밀리초)                                                                                                                 | `1`     | 선택 |
| `ratelimit.prefixKey`   | `string`        | 속도 제한 키의 접두사                                                                                                                         | `${userEmail}` | 선택 |
| `ratelimit.whitelist`   | `array(string)` | 속도 제한에서 제외할 IP 주소 배열                                                                                                              | `[]`    | 선택 |
| `ratelimit.store`       | `object`        | 속도 제한 저장소 위치(자세한 내용은 <ExternalLink text="koa2-ratelimit documentation" to="https://www.npmjs.com/package/koa2-ratelimit"/> 참고) | `MemoryStore` | 선택 |

#### 제공업체(Providers)

이메일 기능은 추가 제공업체를 설치 및 설정하여 확장할 수 있습니다.

제공업체를 통해 기본 기능을 확장할 수 있으며, 예를 들어 Amazon SES 등으로 이메일을 발송할 수 있습니다.

Strapi에서 공식적으로 관리하는 제공업체는 [마켓플레이스](/cms/plugins/installing-plugins-via-marketplace)에서 확인할 수 있으며, 커뮤니티 제공업체도 <ExternalLink to="https://www.npmjs.com/" text="npm"/>에서 찾을 수 있습니다.

제공업체는 [비공개](#private-providers)로 설정하여 자산 URL에 서명을 적용할 수도 있습니다.

##### 제공업체 설치

새 제공업체는 `npm` 또는 `yarn`으로 `@strapi/provider-<plugin>-<provider> --save` 형식으로 설치할 수 있습니다.

예시: Sendgrid 제공업체 설치

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="Yarn">

```bash
yarn add @strapi/provider-email-sendgrid
```

</TabItem>

<TabItem value="npm" label="NPM">

```bash
npm install @strapi/provider-email-sendgrid --save
```

</TabItem>

</Tabs>

##### 제공업체 설정

설치한 제공업체는 [ `/config/plugins` 파일](/cms/configurations/plugins)에서 활성화 및 설정합니다. 파일이 없다면 새로 생성해야 합니다.

각 제공업체별로 설정 항목이 다르니, 마켓플레이스 또는 <ExternalLink to="https://www.npmjs.com/" text="npm"/>에서 제공업체별 문서를 참고하세요.

Sendgrid 제공업체 예시 설정:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"

module.exports = ({ env }) => ({
  // ...
  email: {
    config: {
      provider: 'sendgrid', // 커뮤니티 제공업체는 전체 패키지명 사용(예: provider: 'strapi-provider-email-mandrill')
      providerOptions: {
        apiKey: env('SENDGRID_API_KEY'),
      },
      settings: {
        defaultFrom: 'juliasedefdjian@strapi.io',
        defaultReplyTo: 'juliasedefdjian@strapi.io',
        testAddress: 'juliasedefdjian@strapi.io',
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
  email: {
    config: {
      provider: 'sendgrid', // 커뮤니티 제공업체는 전체 패키지명 사용(예: provider: 'strapi-provider-email-mandrill')
      providerOptions: {
        apiKey: env('SENDGRID_API_KEY'),
      },
      settings: {
        defaultFrom: 'juliasedefdjian@strapi.io',
        defaultReplyTo: 'juliasedefdjian@strapi.io',
        testAddress: 'juliasedefdjian@strapi.io',
      },
    },
  },
  // ...
});
```

</TabItem>

</Tabs>

:::note

* 환경별로 다른 제공업체를 사용하려면 `/config/env/${yourEnvironment}/plugins.js|ts`에 올바른 설정을 지정하세요([환경](/cms/configurations/environment) 참고).
* 한 번에 하나의 이메일 제공업체만 활성화됩니다. 이메일 제공업체 설정이 적용되지 않는다면 plugins.js|ts 파일의 위치를 확인하세요.
* Strapi 설치 시 생성되는 두 개의 이메일 템플릿으로 새 제공업체를 테스트할 때, 템플릿의 _발신자 이메일_이 기본값(`no-reply@strapi.io`)으로 되어 있으니, 제공업체에 맞게 수정해야 테스트가 정상 동작합니다([템플릿 로컬 설정](/cms/features/users-permissions#templating-emails) 참고).

:::

###### 환경별 설정

제공업체 설정 시 `NODE_ENV` 환경 변수에 따라 설정을 변경하거나, 환경별 자격증명을 사용할 수 있습니다.

`/config/env/{env}/plugins.js|ts` 파일에 환경별 설정을 추가하면, 기본 설정을 덮어씁니다.

##### 제공업체 직접 구현

직접 제공업체를 구현하려면 <ExternalLink to="https://docs.npmjs.com/creating-node-js-modules" text="Node.js 모듈 생성"/> 가이드를 참고하세요.

이메일 기능용 제공업체 인터페이스 예시:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js
module.exports = {
  init: (providerOptions = {}, settings = {}) => {
    return {
      send: async options => {},
    };
  },
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts
export {
  init: (providerOptions = {}, settings = {}) => {
    return {
      send: async options => {},
    };
  },
};
```

</TabItem>

</Tabs>

send 함수에서는 다음에 접근할 수 있습니다:

* `providerOptions`: plugins.js|ts에 작성한 제공업체 설정
* `settings`: plugins.js|ts에 작성한 이메일 설정
* `options`: 이메일 서비스에서 send 함수 호출 시 전달하는 옵션

<ExternalLink to="https://github.com/strapi/strapi/tree/master/packages/providers" text="Strapi 공식 제공업체 예시"/>를 참고하세요.

직접 구현한 제공업체는 <ExternalLink to="https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages" text="npm에 배포"/>하거나, [로컬에서 사용](#local-providers)할 수 있습니다.

###### 로컬 제공업체

npm에 배포하지 않고 직접 만든 제공업체를 사용하려면 다음 단계를 따르세요:

1. 애플리케이션에 `providers` 폴더 생성
2. 제공업체 구현(예: `/providers/strapi-provider-<plugin>-<provider>`)
3. `package.json`의 dependencies에 로컬 경로로 연결

```json
{
  ...
  "dependencies": {
    ...
    "strapi-provider-<plugin>-<provider>": "file:providers/strapi-provider-<plugin>-<provider>",
    ...
  }
}
```

4. `/config/plugins.js|ts` 파일에서 [제공업체 설정](#configuring-providers)
5. yarn 또는 npm install 실행

###### 비공개 제공업체

비공개 제공업체로 설정하면, 콘텐츠 매니저에 표시되는 모든 자산 URL에 서명이 적용되어 보안 접근이 가능합니다.

비공개 제공업체를 활성화하려면 `isPrivate()` 메서드를 구현하고 true를 반환해야 합니다.

백엔드에서는 제공업체의 `getSignedUrl(file)` 메서드를 사용해 각 자산에 서명된 URL을 생성합니다. 이 URL에는 암호화된 서명이 포함되어, 제한된 시간 및 조건에서만 접근이 허용됩니다(제공업체별로 다름).

보안을 위해 content API에서는 서명된 URL을 제공하지 않습니다. API를 사용하는 개발자는 직접 URL에 서명을 적용해야 합니다.

## 사용법

이메일 기능은 Strapi의 글로벌 API를 사용하므로, 백엔드 서버의 [컨트롤러 또는 서비스](/cms/backend-customization/services)에서, 또는 관리자 패널의 이벤트(예: [라이프사이클 훅](#lifecycle-hook))에서 호출할 수 있습니다.

### 컨트롤러 또는 서비스에서 이메일 발송 {#controller-service}

이메일 기능의 `email` [서비스](/cms/backend-customization/services)에는 이메일을 발송하는 두 가지 함수가 있습니다:

* `send()`: 이메일 내용을 직접 포함하여 발송
* `sendTemplatedEmail()`: 콘텐츠 매니저의 데이터를 활용해 템플릿 기반 이메일 발송

#### `send()` 함수 사용

사용자 액션에 따라 이메일을 발송하려면, [컨트롤러](/cms/backend-customization/controllers) 또는 [서비스](/cms/backend-customization/services)에 `send()` 함수를 추가하세요. send 함수의 속성은 다음과 같습니다:

| 속성      | 타입     | 형식        | 설명                                           |
|-----------|----------|-------------|------------------------------------------------|
| `from`    | `string` | 이메일 주소 | If not specified, uses `defaultFrom` in `plugins.js`. |
| `to`      | `string` | 이메일 주소 | Required                                              |
| `cc`      | `string` | 이메일 주소 | Optional                                              |
| `bcc`     | `string` | 이메일 주소 | Optional                                              |
| `replyTo` | `string` | 이메일 주소 | Optional                                              |
| `subject` | `string` | -             | Required                                              |
| `text`    | `string` | -             | Either `text` or `html` is required.                  |
| `html`    | `string` | HTML          | Either `text` or `html` is required.                  |

The following code example can be used in a controller or a service:

```js title="/src/api/my-api-name/controllers/my-api-name.ts|js (or /src/api/my-api-name/services/my-api-name.ts|js)"
await strapi.plugins['email'].services.email.send({
  to: 'valid email address',
  from: 'your verified email address', //e.g. single sender verification in SendGrid
  cc: 'valid email address',
  bcc: 'valid email address',
  replyTo: 'valid email address',
  subject: 'The Strapi Email feature worked successfully',
  text: 'Hello world!',
  html: 'Hello world!',
}),
```

#### `sendTemplatedEmail()` 함수 사용

The `sendTemplatedEmail()` function is used to compose emails from a template. The function compiles the email from the available properties and then sends the email.

To use the `sendTemplatedEmail()` function, define the `emailTemplate` object and add the function to a controller or service. The function calls the `emailTemplate` object, and can optionally call the `emailOptions` and `data` objects:

| Parameter       | Description                                                                                                                                | Type     | Default |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|----------|---------|
| `emailOptions` <br/> Optional | Contains email addressing properties: `to`, `from`, `replyTo`, `cc`, and `bcc`                                                             | `object` | { }      |
| `emailTemplate` | Contains email content properties: `subject`, `text`, and `html` using <ExternalLink to="https://lodash.com/docs/4.17.15#template" text="Lodash string templates"/> | `object` | { }      |
| `data`  <br/> Optional          | Contains the data used to compile the templates                                                                                            | `object` | { }      |

The following code example can be used in a controller or a service:

```js title="/src/api/my-api-name/controllers/my-api-name.js (or ./src/api/my-api-name/services/my-api-name.js)"
const emailTemplate = {
  subject: 'Welcome <%= user.firstname %>',
  text: `Welcome to mywebsite.fr!
    Your account is now linked with: <%= user.email %>.`,
  html: `<h1>Welcome to mywebsite.fr!</h1>
    <p>Your account is now linked with: <%= user.email %>.<p>`,
};

await strapi.plugins['email'].services.email.sendTemplatedEmail(
  {
    to: user.email,
    // from: is not specified, the defaultFrom is used.
  },
    emailTemplate,
  {
    user: _.pick(user, ['username', 'email', 'firstname', 'lastname']),
  }
);
```

### Sending emails from a lifecycle hook {#lifecycle-hook}

 To trigger an email based on administrator actions in the admin panel use [lifecycle hooks](/cms/backend-customization/models#lifecycle-hooks) and the [`send()` function](#using-the-send-function). 

 The following example illustrates how to send an email each time a new content entry is added in the Content Manager use the `afterCreate` lifecycle hook:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/src/api/my-api-name/content-types/my-content-type-name/lifecycles.js"

module.exports = {
    async afterCreate(event) {    // Connected to "Save" button in admin panel
        const { result } = event;

        try{
            await strapi.plugin('email').service('email').send({ // you could also do: await strapi.service('plugin:email.email').send({
              to: 'valid email address',
              from: 'your verified email address', // e.g. single sender verification in SendGrid
              cc: 'valid email address',
              bcc: 'valid email address',
              replyTo: 'valid email address',
              subject: 'The Strapi Email feature worked successfully',
              text: '${fieldName}', // Replace with a valid field ID
              html: 'Hello world!', 
                
            })
        } catch(err) {
            console.log(err);
        }
    }
}
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/src/api/my-api-name/content-types/my-content-type-name/lifecycles.ts"

export default {
  async afterCreate(event) {    // Connected to "Save" button in admin panel
    const { result } = event;

    try{
      await strapi.plugins['email'].services.email.send({
        to: 'valid email address',
        from: 'your verified email address', // e.g. single sender verification in SendGrid
        cc: 'valid email address',
        bcc: 'valid email address',
        replyTo: 'valid email address',
        subject: 'The Strapi Email feature worked successfully',
        text: '${fieldName}', // Replace with a valid field ID
        html: 'Hello world!', 
      })
    } catch(err) {
      console.log(err);
    }
  }
}

```

</TabItem>

</Tabs>
