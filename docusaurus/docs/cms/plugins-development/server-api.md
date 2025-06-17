---
title: 플러그인용 서버 API
sidebar_label: 서버 API
displayed_sidebar: cmsSidebar
description: Strapi의 플러그인용 서버 API는 Strapi 플러그인이 애플리케이션의 백엔드 부분(즉, 서버)을 커스터마이징할 수 있게 해줍니다.
tags:
- 플러그인 API
- 라이프사이클 함수
- register 함수
- bootstrap 함수
- destroy 함수
- 설정
- 백엔드 커스터마이징
- 라우트
- 컨트롤러
- 서비스
- 정책
- 미들웨어

---

# 플러그인용 서버 API

Strapi 플러그인은 Strapi 애플리케이션의 백엔드와 [프론트엔드](/cms/plugins-development/admin-panel-api) 모두와 상호작용할 수 있습니다. 서버 API는 백엔드 부분, 즉 플러그인이 Strapi 애플리케이션의 서버 부분과 상호작용하는 방법에 대한 것입니다.

:::prerequisites
[Strapi 플러그인을 생성](/cms/plugins-development/create-a-plugin)했어야 합니다.
:::

서버 API는 다음을 포함합니다:

- 필수 인터페이스를 내보내는 [진입 파일](#entry-file)
- [라이프사이클 함수](#lifecycle-functions)
- [설정](#configuration) API
- [백엔드 서버의 모든 요소를 커스터마이징](#backend-customization)할 수 있는 기능

플러그인 인터페이스를 선언하고 내보낸 후에는 [플러그인 인터페이스를 사용](#usage)할 수 있습니다.

:::note
플러그인의 서버 부분에 대한 모든 코드는 `/server/src/index.ts|js` 파일에 있을 수 있습니다. 하지만 플러그인 SDK에서 생성된 [구조](/cms/plugins-development/plugin-structure)처럼 코드를 다른 폴더로 분할하는 것이 권장됩니다.
:::

## 진입 파일

플러그인 폴더 루트의 `/src/server/index.js` 파일은 필수 인터페이스를 내보내며, 다음 매개변수들을 사용할 수 있습니다:

| 매개변수 타입 | 사용 가능한 매개변수 |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 라이프사이클 함수 | <ul><li> [register](#register)</li><li>[bootstrap](#bootstrap)</li><li>[destroy](#destroy)</li></ul> |
| 설정 | <ul><li>[config](#configuration) 객체</li></ul> |
| 백엔드 커스터마이징 | <ul><li>[contentTypes](#content-types)</li><li>[routes](#routes)</li><li>[controllers](#controllers)</li><li>[services](#services)</li><li>[policies](#policies)</li><li>[middlewares](#middlewares)</li></ul> |

## 라이프사이클 함수

<br/>

### register()

이 함수는 애플리케이션이 [부트스트랩](#bootstrap)되기 전에 플러그인을 로드하기 위해 호출되며, [권한](/cms/features/users-permissions), [커스텀 필드](/cms/features/custom-fields#registering-a-custom-field-on-the-server)의 서버 부분, 또는 데이터베이스 마이그레이션을 등록하는 데 사용됩니다.

**타입**: `Function`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/register.js"

'use strict';

const register = ({ strapi }) => {
  // 일부 register 코드 실행
};

module.exports = register;
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/register.ts"

import type { Core } from '@strapi/strapi';

const register = ({ strapi }: { strapi: Core.Strapi }) => {
  // 일부 register 코드 실행
};

export default register;
```

</TabItem>

</Tabs>

### bootstrap()

[bootstrap](/cms/configurations/functions#bootstrap) 함수는 플러그인이 [등록](#register)된 직후에 호출됩니다.

**타입**: `Function`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/bootstrap.js"
'use strict';

const bootstrap = ({ strapi }) => {
  // 일부 bootstrap 코드 실행
};

module.exports = bootstrap;
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/bootstrap.ts"
import type { Core } from '@strapi/strapi';

const bootstrap = ({ strapi }: { strapi: Core.Strapi }) => {
  // 일부 bootstrap 코드 실행
};

export default bootstrap;

```

</TabItem>

</Tabs>

### destroy()

[destroy](/cms/configurations/functions#destroy) 라이프사이클 함수는 Strapi 인스턴스가 파괴될 때 플러그인을 정리(연결 종료, 리스너 제거 등)하기 위해 호출됩니다.

**타입**: `Function`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/destroy.js"
'use strict';

const destroy = ({ strapi }) => {
  // 일부 destroy 코드 실행
};

module.exports = destroy;
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/destroy.ts"
import type { Core } from '@strapi/strapi';

const destroy = ({ strapi }: { strapi: Core.Strapi }) => {
  // destroy 단계
};

export default destroy;
```

</TabItem>
</Tabs>

## 설정

`config`는 기본 플러그인 설정을 저장합니다. [`./config/plugins.js` 설정 파일](/cms/configurations/plugins)에서 사용자가 입력한 설정을 로드하고 유효성을 검사합니다.

**타입**: `Object`

| 매개변수 | 타입 | 설명 |
| ----------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default` | Object, 또는 Object를 반환하는 Function | 기본 플러그인 설정, 사용자 설정과 병합됩니다 |
| `validator` | Function | <ul><li>기본 플러그인 설정과 사용자 설정을 병합한 결과가 유효한지 확인합니다</li><li>결과 설정이 유효하지 않을 때 오류를 발생시킵니다</li></ul> |

**예시:**

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/config/index.js"

module.exports = {
  default: ({ env }) => ({ optionA: true }),
  validator: (config) => { 
    if (typeof config.optionA !== 'boolean') {
      throw new Error('optionA는 불린 값이어야 합니다');
    }
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/config/index.ts"

export default {
  default: ({ env }) => ({ optionA: true }),
  validator: (config) => { 
    if (typeof config.optionA !== 'boolean') {
      throw new Error('optionA는 불린 값이어야 합니다');
    }
  },
};
```

</TabItem>
</Tabs>

## 백엔드 커스터마이징

모든 [백엔드 커스터마이징](/cms/backend-customization) 요소는 플러그인에서 사용할 수 있습니다.

### 콘텐츠 타입

플러그인에 [콘텐츠 타입](/cms/backend-customization/models)을 추가할 수 있습니다.

`contentTypes` 키는 플러그인의 콘텐츠 타입 정의를 저장하는 객체를 기대하며, 여기서 키는 콘텐츠 타입 이름이고 값은 콘텐츠 타입 정의입니다([스키마 생성](/cms/backend-customization/models#defining-a-model-schema) 참고).

**타입**: `Object`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/content-types/index.js"

'use strict';

const myContentType = require('./my-content-type');

module.exports = {
  'my-content-type': myContentType, // 콘텐츠 타입의 키는 콘텐츠 타입 파일명과 일치해야 함
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/content-types/index.ts"

import myContentType from './my-content-type';

export default {
  'my-content-type': myContentType, // 콘텐츠 타입의 키는 콘텐츠 타입 파일명과 일치해야 함
};
```

</TabItem>

</Tabs>

### 라우트

플러그인은 [라우트](/cms/backend-customization/routes)를 추가할 수 있으며, 이는 다음 형식으로 정의할 수 있습니다:

```js
module.exports = {
  routes: [
    {
      method: 'GET',
      path: '/my-plugin/my-route',
      handler: 'myController.myAction',
      config: {
        auth: false,
        policies: [],
        middlewares: [],
      },
    },
  ],
};
```

`routes` 키는 플러그인의 라우트를 저장하는 배열 또는 객체를 기대합니다. 자세한 구문은 [라우트 문서](/cms/backend-customization/routes)를 참고하세요.

**타입**: `Object` 또는 `Array`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/routes/index.js"
module.exports = [
  {
    method: 'GET',
    path: '/my-plugin',
    handler: 'myController.index',
    config: {
      policies: [],
      auth: false,
    },
  },
  {
    method: 'GET',
    path: '/my-plugin/:id',
    handler: 'myController.findOne',
    config: {
      policies: [],
      auth: false,
    },
  },
];
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/routes/index.ts"
export default [
  {
    method: 'GET',
    path: '/my-plugin',
    handler: 'myController.index',
    config: {
      policies: [],
      auth: false,
    },
  },
  {
    method: 'GET',
    path: '/my-plugin/:id',
    handler: 'myController.findOne',
    config: {
      policies: [],
      auth: false,
    },
  },
];
```

</TabItem>

</Tabs>

### 컨트롤러

플러그인은 [컨트롤러](/cms/backend-customization/controllers)를 추가할 수 있습니다.

`controllers` 키는 플러그인의 컨트롤러를 저장하는 객체를 기대하며, 여기서 키는 컨트롤러 이름이고 값은 컨트롤러 구현체입니다.

**타입**: `Object`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/controllers/index.js"

'use strict';

const myController = require('./my-controller');

module.exports = {
  myController,
};
```

```js title="/src/plugins/my-plugin/server/src/controllers/my-controller.js"

'use strict';

module.exports = {
  index(ctx) {
    ctx.body = `Hello World!`;
  },

  async findOne(ctx) {
    return strapi.plugin('my-plugin').service('myService').findOne(ctx.params.id);
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/controllers/index.ts"

import myController from './my-controller';

export default {
  myController,
};
```

```js title="/src/plugins/my-plugin/server/src/controllers/my-controller.ts"

import type { Core } from '@strapi/strapi';

export default {
  index(ctx) {
    ctx.body = `Hello World!`;
  },

  async findOne(ctx) {
    return strapi.plugin('my-plugin').service('myService').findOne(ctx.params.id);
  },
};
```

</TabItem>

</Tabs>

### 서비스

플러그인은 [서비스](/cms/backend-customization/services)를 추가할 수 있습니다.

`services` 키는 플러그인의 서비스를 저장하는 객체를 기대하며, 여기서 키는 서비스 이름이고 값은 서비스 구현체입니다.

**타입**: `Object`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/services/index.js"

'use strict';

const myService = require('./my-service');

module.exports = {
  myService,
};
```

```js title="/src/plugins/my-plugin/server/src/services/my-service.js"

'use strict';

module.exports = ({ strapi }) => ({
  findOne(id) {
    return strapi.documents('plugin::my-plugin.my-content-type').findOne({
      documentId: id,
    });
  },
});
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/services/index.ts"

import myService from './my-service';

export default {
  myService,
};
```

```js title="/src/plugins/my-plugin/server/src/services/my-service.ts"

import type { Core } from '@strapi/strapi';

export default ({ strapi }: { strapi: Core.Strapi }) => ({
  findOne(id: string) {
    return strapi.documents('plugin::my-plugin.my-content-type').findOne({
      documentId: id,
    });
  },
});
```

</TabItem>

</Tabs>

### 정책

플러그인은 [정책](/cms/backend-customization/policies)을 추가할 수 있습니다.

`policies` 키는 플러그인의 정책을 저장하는 객체를 기대하며, 여기서 키는 정책 이름이고 값은 정책 구현체입니다.

**타입**: `Object`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/policies/index.js"

'use strict';

const myPolicy = require('./my-policy');

module.exports = {
  myPolicy,
};
```

```js title="/src/plugins/my-plugin/server/src/policies/my-policy.js"

'use strict';

module.exports = (policyContext, config, { strapi }) => {
  // Add your own logic here.
  console.log('In my-policy policy.');

  const canDoSomething = true;

  if (canDoSomething) {
    return true;
  }

  return false;
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/policies/index.ts"

import myPolicy from './my-policy';

export default {
  myPolicy,
};
```

```js title="/src/plugins/my-plugin/server/src/policies/my-policy.ts"

export default (policyContext, config, { strapi }) => {
  // Add your own logic here.
  console.log('In my-policy policy.');

  const canDoSomething = true;

  if (canDoSomething) {
    return true;
  }

  return false;
};
```

</TabItem>

</Tabs>

### 미들웨어

플러그인은 [미들웨어](/cms/backend-customization/middlewares)를 추가할 수 있습니다.

`middlewares` 키는 플러그인의 미들웨어를 저장하는 객체를 기대하며, 여기서 키는 미들웨어 이름이고 값은 미들웨어 구현체입니다.

**타입**: `Object`

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js title="/src/plugins/my-plugin/server/src/middlewares/index.js"

'use strict';

const myMiddleware = require('./my-middleware');

module.exports = {
  myMiddleware,
};
```

```js title="/src/plugins/my-plugin/server/src/middlewares/my-middleware.js"

'use strict';

/**
 * `my-middleware` 미들웨어
 */

module.exports = (config, { strapi }) => {
  // 여기에 자신만의 로직을 추가하세요.
  return async (ctx, next) => {
    console.log('In my-middleware middleware.');

    await next();
  };
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/src/plugins/my-plugin/server/src/middlewares/index.ts"

import myMiddleware from './my-middleware';

export default {
  myMiddleware,
};
```

```js title="/src/plugins/my-plugin/server/src/middlewares/my-middleware.ts"

/**
 * `my-middleware` 미들웨어
 */

export default (config, { strapi }) => {
  // 여기에 자신만의 로직을 추가하세요.
  return async (ctx, next) => {
    console.log('In my-middleware middleware.');

    await next();
  };
};
```

</TabItem>

</Tabs>

## 사용법

서버 API에서 정의된 요소에 액세스하려면, `strapi.plugin(pluginName).service|controller|contentType|policy|middleware(elementName)` 문법을 사용하세요.

**예시:**

<Tabs groupId="js-ts">

<TabItem value="js" label="JavaScript">

```js
module.exports = {
  async findOne(ctx) {
    // 플러그인의 서비스에 액세스
    return await strapi
      .plugin('my-plugin')
      .service('myService')
      .findOne(ctx.params.id);
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js
export default {
  async findOne(ctx) {
    // 플러그인의 서비스에 액세스
    return await strapi
      .plugin('my-plugin')
      .service('myService')
      .findOne(ctx.params.id);
  },
};
```

</TabItem>

</Tabs>
