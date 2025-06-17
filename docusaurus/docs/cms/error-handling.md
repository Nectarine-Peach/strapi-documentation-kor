---
title: 에러 처리
displayed_sidebar: cmsSidebar
description: Strapi의 에러 처리 기능을 통해 애플리케이션에서 에러를 쉽게 송수신할 수 있습니다.
tags:
- ctx
- GraphQL API
- GraphQL 에러
- 정책(policies)
- 미들웨어
- REST API
- REST 에러
- 에러 발생
- strapi-utils
---

# 에러 처리

Strapi는 표준 포맷으로 에러를 기본적으로 처리합니다.

에러 처리는 두 가지 주요 상황에서 사용됩니다:

- [REST](/cms/api/rest) 또는 [GraphQL](/cms/api/graphql) API를 통해 콘텐츠를 쿼리하는 개발자는 요청에 대한 [에러 응답](#receiving-errors)을 받을 수 있습니다.
- Strapi 애플리케이션의 백엔드를 커스터마이징하는 개발자는 컨트롤러나 서비스에서 [에러를 발생](#throwing-errors)시킬 수 있습니다.

## 에러 응답 받기

에러는 응답 객체의 `error` 키에 포함되며, HTTP 상태 코드, 에러 이름, 추가 정보 등이 포함됩니다.

### REST 에러

REST API에서 발생한 에러는 다음과 같은 [응답](/cms/api/rest#requests) 포맷으로 반환됩니다:

```json
{
  "data": null,
  "error": {
    "status": "", // HTTP 상태
    "name": "", // Strapi 에러 이름('ApplicationError' 또는 'ValidationError')
    "message": "", // 사람이 읽을 수 있는 에러 메시지
    "details": {
      // 에러 타입별 상세 정보
    }
  }
}
```

### GraphQL 에러

GraphQL API에서 발생한 에러는 다음과 같은 포맷으로 반환됩니다:

```json
{ "errors": [
    {
      "message": "", // 사람이 읽을 수 있는 에러 메시지
      "extensions": {
        "error": {
          "name": "", // Strapi 에러 이름('ApplicationError' 또는 'ValidationError')
          "message": "", // 사람이 읽을 수 있는 에러 메시지(동일)
          "details": {}, // 에러 타입별 상세 정보
        },
        "code": "" // GraphQL 에러 코드(예: BAD_USER_INPUT)
      }
    }
  ],
  "data": {
    "graphQLQueryName": null
  }
}
```

## 에러 발생시키기

<br/>

### 컨트롤러 및 미들웨어

Strapi에서 커스텀 로직을 개발할 때 에러를 발생시키는 권장 방법은 [컨트롤러](/cms/backend-customization/controllers)나 [미들웨어](/cms/backend-customization/middlewares)에서 적절한 상태와 본문으로 응답하는 것입니다.

이는 컨텍스트(예: `ctx`)에서 에러 함수를 호출하여 처리할 수 있습니다. 사용 가능한 에러 함수는 <ExternalLink to="https://github.com/jshttp/http-errors#list-of-all-constructors" text="http-errors 문서"/>에서 확인할 수 있으며, Strapi에서는 lower camel-case로 사용해야 합니다(예: `badRequest`).

에러 함수는 두 개의 파라미터를 받으며, 각각 [API 쿼리 시 받는 응답](#receiving-errors)의 `error.message`와 `error.details`에 해당합니다:

- 첫 번째 파라미터: 에러 메시지(`message`)
- 두 번째 파라미터: 응답의 `details`로 설정될 객체

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js
// path: ./src/api/[api-name]/controllers/my-controller.js

module.exports = {
  renameDog: async (ctx, next) => {
    const newName = ctx.request.body.name;
    if (!newName) {
      return ctx.badRequest('name is missing', { foo: 'bar' })
    }
    ctx.body = strapi.service('api::dog.dog').rename(newName);
  }
}

// path: ./src/api/[api-name]/middlewares/my-middleware.js

module.exports = async (ctx, next) => {
  const newName = ctx.request.body.name;
  if (!newName) {
    return ctx.badRequest('name is missing', { foo: 'bar' })
  }
  await next();
}
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts
// path: ./src/api/[api-name]/controllers/my-controller.ts

export default {
  renameDog: async (ctx, next) => {
    const newName = ctx.request.body.name;
    if (!newName) {
      return ctx.badRequest('name is missing', { foo: 'bar' })
    }
    ctx.body = strapi.service('api::dog.dog').rename(newName);
  }
}

// path: ./src/api/[api-name]/middlewares/my-middleware.ts

export default async (ctx, next) => {
  const newName = ctx.request.body.name;
  if (!newName) {
    return ctx.badRequest('name is missing', { foo: 'bar' })
  }
  await next();
}
```

</TabItem>

</Tabs>

### 서비스 및 모델 라이프사이클

컨트롤러나 미들웨어보다 더 깊은 레이어에서 작업할 때는, 에러를 발생시키기 위한 전용 에러 클래스가 있습니다. 이 클래스들은 <ExternalLink to="https://nodejs.org/api/errors.html#errors_class_error" text="Node `Error` 클래스"/>를 확장하며, 특정 용도에 맞게 설계되어 있습니다.

이 에러 클래스들은 `@strapi/utils` 패키지에서 import하여 여러 레이어에서 사용할 수 있습니다. 아래 예시는 서비스 레이어를 사용하지만, 서비스와 모델 라이프사이클 외에도 사용할 수 있습니다. 모델 라이프사이클 레이어에서 에러를 발생시킬 때는 `ApplicationError` 클래스를 사용하는 것이 관리자 패널에 올바른 에러 메시지를 표시하는 데 권장됩니다.

:::note
Strapi에서 제공하는 [기본 에러 클래스](#default-error-classes)도 참고하세요.
:::

#### 예시: 서비스에서 에러 발생시키기

아래는 [코어 서비스 확장](/cms/backend-customization/services#extending-core-services)에서 `create` 메서드에 커스텀 검증을 추가하는 예시입니다:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="path: ./src/api/restaurant/services/restaurant.js"

const { errors } = require('@strapi/utils');
const { ApplicationError } = errors;
const { createCoreService } = require('@strapi/strapi').factories;

module.exports = createCoreService('api::restaurant.restaurant', ({ strapi }) =>  ({
  async create(params) {
    let okay = false;

    // 에러를 발생시키면 restaurant 생성이 중단됨
    if (!okay) {
      throw new errors.ApplicationError('Something went wrong', { foo: 'bar' });
    }
  
    const result = await super.create(params);

    return result;
  }
}));
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="path: ./src/api/[api-name]/policies/my-policy.ts"

import { errors } from '@strapi/utils';
import { factories } from '@strapi/strapi';

const { ApplicationError } = errors;

export default factories.createCoreService('api::restaurant.restaurant', ({ strapi }) =>  ({
  async create(params) {
    let okay = false;

    // 에러를 발생시키면 restaurant 생성이 중단됨
    if (!okay) {
      throw new errors.ApplicationError('Something went wrong', { foo: 'bar' });
    }
  
    const result = await super.create(params);

    return result;
  }
}));

```

</TabItem>

</Tabs>

#### 예시: 모델 라이프사이클에서 에러 발생시키기

아래는 [커스텀 모델 라이프사이클](/cms/backend-customization/models#lifecycle-hooks)에서 에러를 발생시켜 요청을 중단하고, 관리자 패널에 올바른 에러 메시지를 반환하는 예시입니다. 일반적으로 `beforeX` 라이프사이클에서만 에러를 발생시키는 것이 좋습니다.

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="path: ./src/api/[api-name]/content-types/[api-name]/lifecycles.js"

const { errors } = require('@strapi/utils');
const { ApplicationError } = errors;

module.exports = {
  beforeCreate(event) {
    let okay = false;

    // 에러를 발생시키면 엔티티 생성이 중단됨
    if (!okay) {
      throw new errors.ApplicationError('Something went wrong', { foo: 'bar' });
    }
  },
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="path: ./src/api/[api-name]/content-types/[api-name]/lifecycles.ts"

import { errors } from '@strapi/utils';
const { ApplicationError } = errors;

export default {
  beforeCreate(event) {
    let okay = false;

    // 에러를 발생시키면 엔티티 생성이 중단됨
    if (!okay) {
      throw new errors.ApplicationError('Something went wrong', { foo: 'bar' });
    }
  },
};
```

</TabItem>

</Tabs>

### 정책(Policy)

[정책(Policy)](/cms/backend-customization/policies)은 컨트롤러 실행 전 동작하는 특수한 미들웨어입니다. 사용자의 액션 허용 여부를 검사하며, 허용되지 않을 경우 `return false`로 일반 에러를 발생시킬 수 있습니다. 또는 Strapi의 `ForbiddenError`, `ApplicationError` 클래스(둘 다 [기본 에러 클래스](#default-error-classes) 참고), 그리고 <ExternalLink to="https://nodejs.org/api/errors.html#errors_class_error" text="Node `Error` 클래스"/>를 확장한 커스텀 에러 메시지를 던질 수도 있습니다.

`PolicyError` 클래스는 `@strapi/utils` 패키지에서 제공되며, 2개의 파라미터를 받습니다:

- 첫 번째 파라미터: 에러 메시지(`message`)
- (선택) 두 번째 파라미터: 응답의 `details`로 설정될 객체(권장: 에러를 발생시킨 정책 이름을 `policy` 키로 명시)

#### 예시: 커스텀 정책에서 PolicyError 발생시키기

아래는 [커스텀 정책](/cms/backend-customization/policies)에서 에러 메시지를 던져 요청을 중단하는 예시입니다.

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="path: ./src/api/[api-name]/policies/my-policy.js"

const { errors } = require('@strapi/utils');
const { PolicyError } = errors;

module.exports = (policyContext, config, { strapi }) => {
  let isAllowed = false;

  if (isAllowed) {
    return true;
  } else {
    throw new errors.PolicyError('You are not allowed to perform this action', {
      policy: 'my-policy',
      myCustomKey: 'myCustomValue',
    });
  }
}

```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="path: ./src/api/[api-name]/policies/my-policy.ts"

import { errors } from '@strapi/utils';
const { PolicyError } = errors;

export default (policyContext, config, { strapi }) => {
  let isAllowed = false;

  if (isAllowed) {
    return true;
  } else {
    throw new errors.PolicyError('You are not allowed to perform this action', {
      policy: 'my-policy',
      myCustomKey: 'myCustomValue',
    });
  }
};
```

</TabItem>

</Tabs>

### 기본 에러 클래스

기본 에러 클래스는 `@strapi/utils` 패키지에서 import하여 사용할 수 있습니다. 모든 기본 에러 클래스는 확장하여 커스텀 에러 클래스를 만들 수 있습니다. 커스텀 에러 클래스는 코드 내에서 에러를 발생시키는 데 사용할 수 있습니다.

<Tabs> 

<TabItem value="Application" label="Application">

`ApplicationError` 클래스는 애플리케이션 에러를 위한 일반적인 에러 클래스이며, 기본값으로 권장됩니다. 이 클래스는 관리자 패널에서 읽고 사용자에게 표시할 수 있는 에러 메시지를 던지도록 설계되었습니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `An application error occured` |
| `details` | `object` | 추가 상세 정보 객체 | `{}` |

```js
throw new errors.ApplicationError('Something went wrong', { foo: 'bar' });
```

</TabItem>

<TabItem value="Pagination" label="Pagination">

`PaginationError` 클래스는 [REST](/cms/api/rest/sort-pagination#pagination), [GraphQL](/cms/api/graphql#pagination), [Document Service](/cms/api/document-service)에서 페이지네이션 정보를 파싱할 때 주로 사용됩니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Invalid pagination` |

```js
throw new errors.PaginationError('Exceeded maximum pageSize limit');
```

</TabItem>

<TabItem value="NotFound" label="NotFound">

`NotFoundError` 클래스는 404 상태 코드를 반환할 때 사용하는 일반 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Entity not found` |

```js
throw new errors.NotFoundError('These are not the droids you are looking for');
```

</TabItem>

<TabItem value="Forbidden" label="Forbidden">

`ForbiddenError` 클래스는 인증 정보가 없거나 올바르지 않을 때 사용하는 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Forbidden access` |

```js
throw new errors.ForbiddenError('Ah ah ah, you didn\'t say the magic word');
```

</TabItem>

<TabItem value="Unauthorized" label="Unauthorized">

`UnauthorizedError` 클래스는 인증은 되었으나 특정 액션에 대한 권한이 없을 때 사용하는 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Unauthorized` |

```js
throw new errors.UnauthorizedError('You shall not pass!');
```

</TabItem>

<TabItem value="NotImplemented" label="NotImplemented">

`NotImplementedError` 클래스는 아직 구현되지 않았거나 설정되지 않은 기능을 요청할 때 사용하는 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `This feature isn't implemented` |

```js
throw new errors.NotImplementedError('This isn\'t implemented', { feature: 'test', implemented: false });
```

</TabItem>

<TabItem value="PayloadTooLarge" label="PayloadTooLarge">

`PayloadTooLargeError` 클래스는 요청 본문이나 첨부 파일이 서버 제한을 초과할 때 사용하는 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Entity too large` |

```js
throw new errors.PayloadTooLargeError('Uh oh, the file too big!');
```

</TabItem>

<TabItem value="Policy" label="Policy">

`PolicyError` 클래스는 [라우트 정책](/cms/backend-customization/policies)에서 사용하도록 설계된 에러입니다. 권장 사항은 `details` 파라미터에 정책 이름을 명시하는 것입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Policy Failed` |
| `details` | `object` | 추가 상세 정보 객체 | `{}` |

```js
throw new errors.PolicyError('Something went wrong', { policy: 'my-policy' });
```

</TabItem>

<TabItem value="Validation" label="Validation">

`ValidationError` 클래스는 입력 데이터 검증 실패 시 사용하는 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `message` | `string` | 에러 메시지 | `Validation error` |
| `details` | `object` | 추가 상세 정보 객체 | `{}` |

```js
throw new errors.ValidationError('Invalid input provided', { field: 'email', value: 'not-an-email' });
```

</TabItem>

<TabItem value="YupValidation" label="YupValidation">

`YupValidationError` 클래스는 <ExternalLink to="https://github.com/jquense/yup" text="Yup"/>을 사용한 검증 실패 시 사용하는 에러 클래스입니다. 파라미터는 다음과 같습니다:

| 파라미터 | 타입 | 설명 | 기본값 |
| --- | --- | --- | --- |
| `errors` | `array` | Yup 검증 에러 배열 | `[]` |
| `message` | `string` | 에러 메시지 | `Validation error` |

```js
throw new errors.YupValidationError(validationErrors, 'Schema validation failed');
```

</TabItem>

</Tabs>
