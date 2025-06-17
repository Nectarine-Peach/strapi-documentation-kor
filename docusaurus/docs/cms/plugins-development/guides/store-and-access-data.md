---
title: Strapi 플러그인에서 데이터 저장 및 접근하는 방법
description: Strapi 플러그인에서 데이터를 저장하고 접근하는 방법을 알아보세요
sidebar_label: 데이터 저장 및 접근
displayed_sidebar: cmsSidebar
tags:
- 콘텐츠 타입
- 가이드
- 플러그인
- 플러그인 개발
- 플러그인 개발 가이드
---

import NotV5 from '/docs/snippets/_not-updated-to-v5.md'

# Strapi 플러그인에서 데이터 저장 및 접근하는 방법

<NotV5/>

Strapi [플러그인](/cms/plugins-development/developing-plugins)으로 데이터를 저장하려면 플러그인 콘텐츠 타입을 사용하세요. 플러그인 콘텐츠 타입은 다른 [콘텐츠 타입](/cms/backend-customization/models)과 정확히 동일하게 작동합니다. 콘텐츠 타입을 [생성](#create-a-content-type-for-your-plugin)한 후에는 [데이터와 상호작용](#interact-with-data-from-the-plugin)을 시작할 수 있습니다.

## 플러그인용 콘텐츠 타입 생성

CLI 생성기로 콘텐츠 타입을 생성하려면 플러그인의 `server/src/` 디렉토리에서 터미널에서 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">
<TabItem value="yarn" label="Yarn">

```bash
yarn strapi generate content-type
```

</TabItem>

<TabItem value="npm" label="NPM">

```bash
npm run strapi generate content-type
```

</TabItem>
</Tabs>

생성기 CLI는 대화형이며 콘텐츠 타입과 포함할 속성에 대한 몇 가지 질문을 합니다. 첫 번째 질문들에 답한 후, `Where do you want to add this model?` 질문에서 `Add model to existing plugin` 옵션을 선택하고 요청하면 관련 플러그인의 이름을 입력하세요.

<figure style={{width: '100%', margin: '0' }}>
  <img src="/img/assets/development/generate-plugin-content-type.png" alt="CLI로 콘텐츠 타입 플러그인 생성" />
  <em><figcaption style={{fontSize: '12px'}}><code>strapi generate content-type</code> CLI 생성기는 플러그인용 기본 콘텐츠 타입을 만드는 데 사용됩니다.</figcaption></em>
</figure>

<br />

CLI는 플러그인을 사용하는 데 필요한 코드를 생성하며, 다음이 포함됩니다:

- [콘텐츠 타입 스키마](/cms/backend-customization/models#model-schema)
- 콘텐츠 타입용 기본 [컨트롤러](/cms/backend-customization/controllers), [서비스](/cms/backend-customization/services), [라우트](/cms/backend-customization/routes)

:::tip
CLI 생성기로 콘텐츠 타입의 전체 구조를 완전히 생성하거나 `schema.json` 파일을 직접 생성하고 편집할 수 있습니다. 먼저 CLI 생성기로 간단한 콘텐츠 타입을 생성한 다음 관리자 패널의 [콘텐츠 타입 빌더](/cms/features/content-type-builder)를 활용하여 콘텐츠 타입을 편집하는 것을 권장합니다.

콘텐츠 타입이 관리자 패널에 표시되지 않으면 콘텐츠 타입 스키마의 `pluginOptions` 객체에서 `content-manager.visible`과 `content-type-builder.visible` 매개변수를 `true`로 설정해야 할 수 있습니다:

<details>
<summary>관리자 패널에서 플러그인 콘텐츠 타입을 표시하기:</summary>

다음 예시 `schema.json` 파일의 강조된 줄은 플러그인 콘텐츠 타입을 콘텐츠 타입 빌더와 콘텐츠 매니저에 표시하는 방법을 보여줍니다:

```json title="/server/content-types/my-plugin-content-type/schema.json" {13-20} showLineNumbers
{
  "kind": "collectionType",
  "collectionName": "my_plugin_content_types",
  "info": {
    "singularName": "my-plugin-content-type",
    "pluralName": "my-plugin-content-types",
    "displayName": "My Plugin Content-Type"
  },
  "options": {
    "draftAndPublish": false,
    "comment": ""
  },
  "pluginOptions": {
    "content-manager": {
      "visible": true
    },
    "content-type-builder": {
      "visible": true
    }
  },
  "attributes": {
    "name": {
      "type": "string"
    }
  }
}

```

</details>
:::

### 플러그인 콘텐츠 타입 가져오기 확인

CLI 생성기가 플러그인의 모든 관련 콘텐츠 타입 파일을 가져오지 않았을 수 있으므로, `strapi generate content-type` CLI 명령이 실행 완료된 후 다음 조정을 해야 할 수 있습니다:

1. `/server/index.js` 파일에서 콘텐츠 타입을 가져오세요:

  ```js {7,22} showLineNumbers title="/server/index.js"
  'use strict';

  const register = require('./register');
  const bootstrap = require('./bootstrap');
  const destroy = require('./destroy');
  const config = require('./config');
  const contentTypes = require('./content-types');
  const controllers = require('./controllers');
  const routes = require('./routes');
  const middlewares = require('./middlewares');
  const policies = require('./policies');
  const services = require('./services');

  module.exports = {
    register,
    bootstrap,
    destroy,
    config,
    controllers,
    routes,
    services,
    contentTypes,
    policies,
    middlewares,
  };

  ```

2. `/server/content-types/index.js` 파일에서 콘텐츠 타입 폴더를 가져오세요:

  ```js title="/server/content-types/index.js"
  'use strict';

  module.exports = {
    // 아래 줄에서 my-plugin-content-type을
    // 콘텐츠 타입의 실제 이름과 폴더 경로로 바꾸세요
    "my-plugin-content-type": require('./my-plugin-content-type'),
  };
  ```

3. `/server/content-types/[your-content-type-name]` 폴더에 CLI로 생성된 `schema.json` 파일뿐만 아니라 다음 코드로 콘텐츠 타입을 내보내는 `index.js` 파일도 포함되어 있는지 확인하세요:

  ```js title="/server/content-types/my-plugin-content-type/index.js
  'use strict';

  const schema = require('./schema');

  module.exports = {
    schema,
  };
  ```

## 플러그인에서 데이터와 상호작용하기

플러그인용 콘텐츠 타입을 생성한 후에는 데이터를 생성, 읽기, 업데이트, 삭제할 수 있습니다.

:::note
플러그인은 `/server` 폴더에서만 데이터와 상호작용할 수 있습니다. 관리자 패널에서 데이터를 업데이트해야 하는 경우 [데이터 전달 가이드](/cms/plugins-development/guides/pass-data-from-server-to-admin)를 참고하세요.
:::

데이터를 생성, 읽기, 업데이트, 삭제하려면 [Entity Service API](/cms/api/entity-service) 또는 [Query Engine API](/cms/api/query-engine)를 사용할 수 있습니다. 특히 컴포넌트나 동적 영역에 접근해야 하는 경우 Entity Service API를 사용하는 것이 권장되지만, 기본 데이터베이스에 대한 무제한 접근이 필요한 경우 Query Engine API가 유용합니다.

Entity Service와 Query Engine API 쿼리에서 콘텐츠 타입 식별자에는 `plugin::your-plugin-slug.the-plugin-content-type-name` 구문을 사용하세요.

**예시:**

다음은 `my-plugin`이라는 플러그인에 생성된 `my-plugin-content-type` 컬렉션 타입의 모든 엔트리를 찾는 방법입니다:

```js
// Document Service API 사용
let data = await strapi.documents('plugin::my-plugin.my-plugin-content-type').findMany();

// Query Engine API 사용
let data = await strapi.db.query('plugin::my-plugin.my-plugin-content-type').findMany();
````

:::tip
`미들웨어`, `정책`, `컨트롤러`, `서비스`뿐만 아니라 `register`, `boostrap`, `destroy` 라이프사이클 함수에서 찾을 수 있는 `strapi` 객체를 통해 데이터베이스에 접근할 수 있습니다.
:::
