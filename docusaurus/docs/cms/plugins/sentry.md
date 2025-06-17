---
title: Sentry 플러그인
displayed_sidebar: cmsSidebar
description: Strapi 애플리케이션에서 오류를 추적하세요.
tags:
- 환경
- 글로벌 Sentry 서비스
- Sentry 
---

# Sentry 플러그인

이 플러그인을 사용하면 Sentry를 사용하여 Strapi 애플리케이션에서 오류를 추적할 수 있습니다.

<IdentityCard isPlugin>
  <IdentityCardItem icon="navigation-arrow" title="위치">서버 코드를 통해서만 사용 및 구성 가능</IdentityCardItem>
  <IdentityCardItem icon="package" title="패키지 이름">`@strapi/plugin-sentry`</IdentityCardItem>
  <IdentityCardItem icon="plus-square" title="추가 리소스"><ExternalLink to="https://market.strapi.io/plugins/@strapi-plugin-sentry" text="Strapi 마켓플레이스 페이지"/> <ExternalLink to="https://sentry.io/" text="Sentry 페이지"/></IdentityCardItem>
</IdentityCard>

Sentry 플러그인을 사용하면 다음을 수행할 수 있습니다:

* Strapi 애플리케이션 시작 시 Sentry 인스턴스 초기화
* Strapi 애플리케이션 오류를 이벤트로 Sentry에 전송
* 디버깅을 지원하기 위해 Sentry 이벤트에 추가 메타데이터 포함
* Strapi 서버에서 사용 가능한 글로벌 Sentry 서비스 노출

## 설치

다음과 같이 Strapi 애플리케이션에 종속성을 추가하여 Sentry 플러그인을 설치합니다:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn add @strapi/plugin-sentry
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm install @strapi/plugin-sentry
```

</TabItem>

</Tabs>

## 구성

Sentry 플러그인을 구성하려면 `/config/plugins` 파일을 생성하거나 편집하세요. 다음 속성을 사용할 수 있습니다:

| 속성 | 타입 | 기본값 | 설명 |
| -------- | ---- | ------------- |------------ |
| `dsn` | string | `null` | Sentry <ExternalLink to="https://docs.sentry.io/product/sentry-basics/dsn-explainer/" text="데이터 소스 이름"/>. |
| `sendMetadata` | boolean | `true` | 플러그인이 Sentry로 전송되는 이벤트에 추가 정보(예: OS, 브라우저 등)를 첨부할지 여부. |
| `init` | object | `{}` | 초기화 중 Sentry에 직접 전달되는 설정 객체(사용 가능한 옵션은 공식 <ExternalLink to="https://docs.sentry.io/platforms/node/configuration/options/" text="Sentry 문서"/> 참고). |

다음은 기본 구성 예시입니다:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"

module.exports = ({ env }) => ({
  // ...
  sentry: {
    enabled: true,
    config: {
      dsn: env('SENTRY_DSN'),
      sendMetadata: true,
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
  sentry: {
    enabled: true,
    config: {
      dsn: env('SENTRY_DSN'),
      sendMetadata: true,
    },
  },
  // ...
});
```

</TabItem>

</Tabs>

### 비프로덕션 환경에서 비활성화

`sentry.enabled`가 true인 상태에서 `dsn` 속성이 nil 값(`null` 또는 `undefined`)으로 설정되면, Sentry 플러그인은 실행 중인 Strapi 인스턴스에서 사용할 수 있지만 서비스가 실제로 오류를 Sentry에 전송하지는 않습니다. 이를 통해 추가 확인 없이 모든 환경에서 실행되는 코드를 작성할 수 있지만, 프로덕션에서만 오류를 Sentry에 전송할 수 있습니다.

nil `dsn` 설정 속성으로 Strapi를 시작하면 플러그인은 다음 경고를 출력합니다:<br/>`info: @strapi/plugin-sentry is disabled because no Sentry DSN was provided`

환경에 따라 `dsn` 설정 속성을 설정하기 위해 [`env` 유틸리티](/cms/configurations/guides/access-cast-environment-variables)를 사용하여 이를 활용할 수 있습니다.

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"
module.exports = ({ env }) => ({
  // …
  sentry: {
    enabled: true,
    config: {
      // 프로덕션에서만 `dsn` 속성 설정
      dsn: env('NODE_ENV') === 'production' ? env('SENTRY_DSN') : null,
    },
  },
  // …
});
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/plugins.ts"
export default ({ env }) => ({
  // …
  sentry: {
    enabled: true,
    config: {
      // 프로덕션에서만 `dsn` 속성 설정
      dsn: env('NODE_ENV') === 'production' ? env('SENTRY_DSN') : null,
    },
  },
  // …
});
```

</TabItem>

</Tabs>

### 플러그인 완전 비활성화

다른 모든 Strapi 플러그인과 마찬가지로, 플러그인 구성 파일에서 이 플러그인을 비활성화할 수도 있습니다. 이렇게 하면 `strapi.plugins('sentry')`가 `undefined`를 반환합니다:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"
module.exports = ({ env }) => ({
  // …
  sentry: {
    enabled: false,
  },
  // …
});
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/plugins.ts"
export default ({ env }) => ({
  // …
  sentry: {
    enabled: false,
  },
  // …
});
```

</TabItem>
</Tabs>

## 사용법

플러그인을 설치하고 구성한 후, Strapi 애플리케이션에서 다음과 같이 Sentry 서비스에 접근할 수 있습니다:

```js
const sentryService = strapi.plugin('sentry').service('sentry');
```

이 서비스는 다음 메서드를 노출합니다:

| 메서드 | 설명 | 매개변수 |
| ------ | ----------- | ---------- |
| `sendError()` | 수동으로 Sentry에 오류를 전송합니다. | <ul><li><code>error</code>: 전송할 오류.</li><li><code>configureScope</code>: 선택사항. 오류 이벤트를 사용자 지정할 수 있습니다.</li></ul> 자세한 내용은 공식 <ExternalLink to="https://docs.sentry.io/platforms/node/enriching-events/scopes/#configuring-the-scope" text="Sentry 문서"/>를 참조하세요. |
| `getInstance()` | Sentry 인스턴스에 직접 접근하는 데 사용됩니다. | - |

`sendError()` 메서드는 다음과 같이 사용할 수 있습니다:

```js
try {
  // 여기에 코드를 작성하세요
} catch (error) {
  // 간단한 오류 전송
  strapi
    .plugin('sentry')
    .service('sentry')
    .sendError(error);

  // 또는 사용자 지정 Sentry 스코프로 오류 전송
  strapi
    .plugin('sentry')
    .service('sentry')
    .sendError(error, (scope, sentryInstance) => {
      // 여기에서 스코프를 사용자 지정하세요
      scope.setTag('my_custom_tag', 'Tag value');
    });
  throw error;
}
```

`getInstance()` 메서드는 다음과 같이 접근할 수 있습니다:

```js
const sentryInstance = strapi
  .plugin('sentry')
  .service('sentry')
  .getInstance();
```