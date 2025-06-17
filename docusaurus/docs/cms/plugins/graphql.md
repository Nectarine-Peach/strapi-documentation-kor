---
title: GraphQL 플러그인
displayed_sidebar: cmsSidebar
toc_max_heading_level: 6
description: Strapi 프로젝트에서 GraphQL 엔드포인트를 사용하여 콘텐츠를 가져오고 변경하세요.
tags:
- 관리자 패널 
- API 토큰
- Apollo Server 
- getters
- GraphQL API
- GraphQL 
- 정책
- 플러그인 
- 미들웨어
- 사용자, 역할 및 권한

---

# GraphQL 플러그인

기본적으로 Strapi는 각 콘텐츠 타입에 대해 [REST 엔드포인트](/cms/api/rest#endpoints)를 생성합니다. GraphQL 플러그인은 콘텐츠를 가져오고 변경할 수 있는 GraphQL 엔드포인트를 추가합니다. GraphQL 플러그인이 설치된 상태에서 Apollo Server 기반 GraphQL Sandbox를 사용하여 대화형으로 쿼리와 뮤테이션을 작성하고 콘텐츠 타입에 맞춘 문서를 읽을 수 있습니다.

<IdentityCard isPlugin>
  <IdentityCardItem icon="navigation-arrow" title="위치">관리자 패널을 통해 사용 가능.<br/>관리자 패널과 서버 코드를 통해 구성되며, 각각 다른 옵션 세트를 제공.</IdentityCardItem>
  <IdentityCardItem icon="package" title="패키지 이름">`@strapi/plugin-graphql`  </IdentityCardItem>
  <IdentityCardItem icon="plus-square" title="추가 리소스"><ExternalLink to="https://market.strapi.io/plugins/@strapi-plugin-graphql" text="Strapi 마켓플레이스 페이지"/> </IdentityCardItem>
</IdentityCard>

<ThemedImage
  alt="GraphQL playground 사용 예시"
  sources={{
    light:'/img/assets/apis/use-graphql-playground.gif',
    dark:'/img/assets/apis/use-graphql-playground_DARK.gif',
  }}
/>

## 설치

GraphQL 플러그인을 설치하려면 터미널에서 다음 명령을 실행하세요:

<Tabs groupId="yarn-npm">
<TabItem value="yarn" label="Yarn">

```sh
yarn add @strapi/plugin-graphql
```

</TabItem>
<TabItem value="npm" label="NPM">

```sh
npm install @strapi/plugin-graphql
```

</TabItem>

</Tabs>

설치가 완료되면 GraphQL 샌드박스는 `/graphql` URL에서 접근할 수 있으며, 콘텐츠 타입에 맞춘 쿼리와 뮤테이션을 대화형으로 작성하고 문서를 읽는 데 사용할 수 있습니다.

플러그인이 설치되면 Strapi 애플리케이션 서버가 실행 중일 때 **GraphQL Sandbox**에 `/graphql` 경로(예: <ExternalLink to="http://localhost:1337/graphql" text="localhost:1337/graphql"/>)에서 접근할 수 있습니다.

## 구성

GraphQL 플러그인의 대부분의 구성 옵션은 Strapi 프로젝트의 코드를 통해 처리되지만, GraphQL playground는 Strapi와 관련이 없는 일부 설정도 제공합니다.

### 관리자 패널 설정

Strapi 관리자 패널은 GraphQL 플러그인에 대한 Strapi 관련 설정을 제공하지 않습니다. 하지만 `/graphql` 경로에서 접근할 수 있는 GraphQL Playground는 내장된 Apollo Server playground이므로, 해당 인스턴스에서 사용 가능한 모든 구성 및 설정을 포함합니다. 자세한 내용은 공식 <ExternalLink to="https://www.apollographql.com/docs/apollo-server/v2/testing/graphql-playground" text="GraphQL playground 문서"/>를 참조하세요.

### 코드 기반 구성

플러그인 구성은 [`config/plugins.js` 파일](/cms/configurations/plugins)에서 정의됩니다. 이 구성 파일에는 GraphQL 플러그인에 대한 특정 구성을 정의하는 `graphql.config` 객체가 포함될 수 있습니다.

#### 사용 가능한 옵션

<ExternalLink to="https://www.apollographql.com/docs/apollo-server/api/apollo-server/#apolloserver" text="Apollo Server"/> 옵션은 `graphql.config.apolloServer` 구성 객체를 통해 Apollo에 직접 전달할 수 있습니다. Apollo Server 옵션은 예를 들어 <ExternalLink to="https://www.apollographql.com/docs/federation/metrics/" text="추적 기능"/>을 활성화하는 데 사용할 수 있으며, 이는 GraphQL Sandbox에서 쿼리의 각 부분의 응답 시간을 추적하는 데 지원됩니다. `Apollo Server`의 기본 캐시 옵션은 `cache: 'bounded'`입니다. `apolloServer` 구성에서 이를 변경할 수 있습니다. 자세한 정보는 <ExternalLink to="https://www.apollographql.com/docs/apollo-server/performance/cache-backends/" text="Apollo Server 문서"/>를 참조하세요.

GraphQL 플러그인에는 `config/plugins` 파일 내의 `graphql.config` 객체에서 선언해야 하는 다음과 같은 특정 구성 옵션이 있습니다. 모든 매개변수는 선택사항입니다:

| 옵션             | 타입                | 설명                                                                                                                                                      | 기본값 | 참고사항                                               |
| ------------------ | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | --------------------------------------------------- |
| `endpoint`         | String              | GraphQL 엔드포인트 경로를 설정합니다.                                                                                                                                  | `'/graphql'`  | 예시: `/custom-graphql`                          |
| `shadowCRUD`       | Boolean             | 콘텐츠 타입에 대한 자동 스키마 생성을 활성화하거나 비활성화합니다.                                                                                               | `true`        |                                                     |
| `depthLimit`       | Number              | 과도한 중첩을 방지하기 위해 GraphQL 쿼리의 깊이를 제한합니다.                                                                                                | `10`          | 잠재적인 DoS 공격을 완화하는 데 사용하세요.         |
| `amountLimit`      | Number              | 단일 응답에서 반환되는 최대 항목 수를 제한합니다.                                                                                                                | `100`         | 성능 문제를 피하기 위해 신중하게 사용하세요.         |
| `playgroundAlways` | Boolean             | [폐기됨] 모든 환경에서 GraphQL Playground를 활성화합니다(폐기됨).                                                                                        | `false`       | 대신 `landingPage`를 사용하는 것을 권장합니다.                 |
| `landingPage`      | Boolean \| Function | GraphQL용 랜딩 페이지를 활성화하거나 비활성화합니다. 불린값 또는 불린값을 반환하는 함수, 또는 `renderLandingPage`를 구현하는 ApolloServerPlugin을 허용합니다. |               | 프로덕션에서는 `false`, 다른 환경에서는 `true` |
| `apolloServer`     | Object              | 구성 옵션을 Apollo Server에 직접 전달합니다.                                                                                                          | `{}`          | 예시: `{ tracing: true }`    

:::caution
응답에서 반환되는 최대 항목 수는 기본적으로 100개로 제한됩니다. 이 값은 `amountLimit` 구성 옵션을 사용하여 변경할 수 있지만, 신중한 고려 후에만 변경해야 합니다: 큰 쿼리는 DDoS(분산 서비스 거부) 공격을 일으킬 수 있으며 Strapi 서버뿐만 아니라 데이터베이스 서버에도 비정상적인 부하를 일으킬 수 있습니다.
:::

:::note
GraphQL Sandbox는 프로덕션을 제외한 모든 환경에서 기본적으로 활성화됩니다. 프로덕션 환경에서도 GraphQL Sandbox를 활성화하려면 `landingPage` 구성 옵션을 `true`로 설정하세요.
:::

다음은 사용자 지정 구성 예시입니다:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/plugins.js"
module.exports = {
  graphql: {
    config: {
      endpoint: '/graphql',
      shadowCRUD: true,
      landingPage: false, // 모든 곳에서 Sandbox 비활성화
      depthLimit: 7,
      amountLimit: 100,
      apolloServer: {
        tracing: false,
      },
    },
  },
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/plugins.ts"
export default () => ({
  graphql: {
    config: {
      endpoint: '/graphql',
      shadowCRUD: true,
      landingPage: false, // 모든 곳에서 Sandbox 비활성화
      depthLimit: 7,
      amountLimit: 100,
      apolloServer: {
        tracing: false,
      },
    },
  },
})
```

</TabItem>

</Tabs>


#### 동적으로 Apollo Sandbox 활성화

환경에 따라 동적으로 Apollo Sandbox를 활성화하는 함수를 사용할 수 있습니다:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```javascript title="./config/plugins.js" {6-12}
module.exports = ({ env }) => {
  graphql: {
    config: {
      endpoint: '/graphql',
      shadowCRUD: true,
      landingPage: (strapi) => {
        if (env("NODE_ENV") !== "production") {
          return true;
        } else {
          return false;
        }
      },
    },
  },
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="./config/plugins.ts" {6-12}
export default ({ env }) => {
  graphql: {
    config: {
      endpoint: '/graphql',
      shadowCRUD: true,
      landingPage: (strapi) => {
        if (env("NODE_ENV") !== "production") {
          return true;
        } else {
          return false;
        }
      },
    },
  },
};
```
</TabItem>

</Tabs>

#### 랜딩 페이지의 CORS 예외

If the landing page is enabled in production environments (which is not recommended), CORS headers for the Apollo Server landing page must be added manually.

To add them globally, you can merge the following into your middleware configuration:

```javascript title="/config/middlewares"
{
  name: "strapi::security",
  config: {
    contentSecurityPolicy: {
      useDefaults: true,
      directives: {
        "connect-src": ["'self'", "https:", "apollo-server-landing-page.cdn.apollographql.com"],
        "img-src": ["'self'", "data:", "blob:", "apollo-server-landing-page.cdn.apollographql.com"],
        "script-src": ["'self'", "'unsafe-inline'", "apollo-server-landing-page.cdn.apollographql.com"],
        "style-src": ["'self'", "'unsafe-inline'", "apollo-server-landing-page.cdn.apollographql.com"],
        "frame-src": ["sandbox.embed.apollographql.com"]
      }
    }
  }
}
```

To add these exceptions only for the `/graphql` path (recommended), you can create a new middleware to handle it. For example:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```javascript title="./middlewares/graphql-security.js"
module.exports = (config, { strapi }) => {
  return async (ctx, next) => {
    if (ctx.request.path === '/graphql') {
      ctx.set('Content-Security-Policy', "default-src 'self'; script-src 'self' 'unsafe-inline' cdn.jsdelivr.net apollo-server-landing-page.cdn.apollographql.com; connect-src 'self' https:; img-src 'self' data: blob: apollo-server-landing-page.cdn.apollographql.com; media-src 'self' data: blob: apollo-server-landing-page.cdn.apollographql.com; frame-src sandbox.embed.apollographql.com; manifest-src apollo-server-landing-page.cdn.apollographql.com;");
    }
    await next();
  };
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="./middlewares/graphql-security.ts"
export default (config, { strapi }) => {
  return async (ctx, next) => {
    if (ctx.request.path === '/graphql') {
      ctx.set('Content-Security-Policy', "default-src 'self'; script-src 'self' 'unsafe-inline' cdn.jsdelivr.net apollo-server-landing-page.cdn.apollographql.com; connect-src 'self' https:; img-src 'self' data: blob: apollo-server-landing-page.cdn.apollographql.com; media-src 'self' data: blob: apollo-server-landing-page.cdn.apollographql.com; frame-src sandbox.embed.apollographql.com; manifest-src apollo-server-landing-page.cdn.apollographql.com;");
    }
    await next();
  };
};
```

</TabItem>

</Tabs>

#### Shadow CRUD

To simplify and automate the build of the GraphQL schema, we introduced the Shadow CRUD feature. It automatically generates the type definitions, queries, mutations and resolvers based on your models.

**Example:**

If you've generated an API called `Document` using [the interactive `strapi generate` CLI](/cms/cli#strapi-generate) or the administration panel, your model looks like this:

```json title="/src/api/[api-name]/content-types/document/schema.json"

{
  "kind": "collectionType",
  "collectionName": "documents",
  "info": {
    "singularName": "document",
    "pluralName": "documents",
    "displayName": "document",
    "name": "document"
  },
  "options": {
    "draftAndPublish": true
  },
  "pluginOptions": {},
  "attributes": {
    "name": {
      "type": "string"
    },
    "description": {
      "type": "richtext"
    },
    "locked": {
      "type": "boolean"
    }
  }
}
```

<details> 
<summary>Generated GraphQL type and queries</summary>

```graphql
# Document's Type definition
input DocumentFiltersInput {
  name: StringFilterInput
  description: StringFilterInput
  locked: BooleanFilterInput
  createdAt: DateTimeFilterInput
  updatedAt: DateTimeFilterInput
  publishedAt: DateTimeFilterInput
  and: [DocumentFiltersInput]
  or: [DocumentFiltersInput]
  not: DocumentFiltersInput
}

input DocumentInput {
  name: String
  description: String
  locked: Boolean
  createdAt: DateTime
  updatedAt: DateTime
  publishedAt: DateTime
}

type Document {
  name: String
  description: String
  locked: Boolean
  createdAt: DateTime
  updatedAt: DateTime
  publishedAt: DateTime
}

type DocumentEntity {
  id: ID
  attributes: Document
}

type DocumentEntityResponse {
  data: DocumentEntity
}

type DocumentEntityResponseCollection {
  data: [DocumentEntity!]!
  meta: ResponseCollectionMeta!
}

type DocumentRelationResponseCollection {
  data: [DocumentEntity!]!
}

# Queries to retrieve one or multiple restaurants.
type Query  {
  document(id: ID): DocumentEntityResponse
  documents(
    filters: DocumentFiltersInput
    pagination: PaginationArg = {}
    sort: [String] = []
    publicationState: PublicationState = LIVE
):DocumentEntityResponseCollection
}

# Mutations to create, update or delete a restaurant.
type Mutation {
  createDocument(data: DocumentInput!): DocumentEntityResponse
  updateDocument(id: ID!, data: DocumentInput!): DocumentEntityResponse
  deleteDocument(id: ID!): DocumentEntityResponse
}
```

</details>

#### Customization

Strapi provides a programmatic API to customize GraphQL, which allows:

* disabling some operations for the [Shadow CRUD](#shadow-crud)
* [using getters](#using-getters) to return information about allowed operations
* registering and using an `extension` object to [extend the existing schema](#extending-the-schema) (e.g. extend types or define custom resolvers, policies and middlewares)

<details> 
<summary>Example of GraphQL customizations</summary>

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/src/index.js"

module.exports = {
  /**
   * An asynchronous register function that runs before
   * your application is initialized.
   *
   * This gives you an opportunity to extend code.
   */
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');
    
    extensionService.shadowCRUD('api::restaurant.restaurant').disable();
    extensionService.shadowCRUD('api::category.category').disableQueries();
    extensionService.shadowCRUD('api::address.address').disableMutations();
    extensionService.shadowCRUD('api::document.document').field('locked').disable();
    extensionService.shadowCRUD('api::like.like').disableActions(['create', 'update', 'delete']);
    
    const extension = ({ nexus }) => ({
      // Nexus
      types: [
        nexus.objectType({
          name: 'Book',
          definition(t) {
            t.string('title');
          },
        }),
      ],
      plugins: [
        nexus.plugin({
          name: 'MyPlugin',
          onAfterBuild(schema) {
            console.log(schema);
          },
        }),
      ],
      // GraphQL SDL
      typeDefs: `
          type Article {
              name: String
          }
      `,
      resolvers: {
        Query: {
          address: {
            resolve() {
              return { value: { city: 'Montpellier' } };
            },
          },
        },
      },
      resolversConfig: {
        'Query.address': {
          auth: false,
        },
      },
    });
    extensionService.use(extension);
  },
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/src/index.ts"

export default {
  /**
   * An asynchronous register function that runs before
   * your application is initialized.
   *
   * This gives you an opportunity to extend code.
   */
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');
    
    extensionService.shadowCRUD('api::restaurant.restaurant').disable();
    extensionService.shadowCRUD('api::category.category').disableQueries();
    extensionService.shadowCRUD('api::address.address').disableMutations();
    extensionService.shadowCRUD('api::document.document').field('locked').disable();
    extensionService.shadowCRUD('api::like.like').disableActions(['create', 'update', 'delete']);
    
    const extension = ({ nexus }) => ({
      // Nexus
      types: [
        nexus.objectType({
          name: 'Book',
          definition(t) {
            t.string('title');
          },
        }),
      ],
      plugins: [
        nexus.plugin({
          name: 'MyPlugin',
          onAfterBuild(schema) {
            console.log(schema);
          },
        }),
      ],
      // GraphQL SDL
      typeDefs: `
          type Article {
              name: String
          }
      `,
      resolvers: {
        Query: {
          address: {
            resolve() {
              return { value: { city: 'Montpellier' } };
            },
          },
        },
      },
      resolversConfig: {
        'Query.address': {
          auth: false,
        },
      },
    });
    extensionService.use(extension);
  },
};
```

</TabItem>

</Tabs>

</details>

##### Disabling operations in the Shadow CRUD

The `extension` service provided with the GraphQL plugin exposes functions that can be used to disable operations on Content-Types:

| Content-type function | Description                                    | Argument type    | Possible argument values |
| --------------------  | ---------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------- |
| `disable()`           | Fully disable the Content-Type                 | -                | -                                                                                                          |
| `disableQueries()`    | Only disable queries for the Content-Type      | -                | -                                                                                                          |
| `disableMutations()`  | Only disable mutations for the Content-Type    | -                | -                                                                                                          |
| `disableAction()`     | Disable a specific action for the Content-Type | String           | One value from the list:<ul><li>`create`</li><li>`find`</li><li>`findOne`</li><li>`update`</li><li>`delete`</li></ul>   |
| `disableActions()`    | Disable specific actions for the Content-Type  | Array of Strings | Multiple values from the list: <ul><li>`create`</li><li>`find`</li><li>`findOne`</li><li>`update`</li><li>`delete`</li></ul>  |

Actions can also be disabled at the field level, with the following functions:

| Field function     | Description                      |
| ------------------ | -------------------------------- |
| `disable()`        | Fully disable the field          |
| `disableOutput()`  | Disable the output on a field    |
| `disableInput()`   | Disable the input on a field     |
| `disableFilters()` | Disable filters input on a field |

**Examples:**

```js
// Disable the 'find' operation on the 'restaurant' content-type in the 'restaurant' API
strapi
  .plugin('graphql')
  .service('extension')
  .shadowCRUD('api::restaurant.restaurant')
  .disableAction('find')

// Disable the 'name' field on the 'document' content-type in the 'document' API
strapi
  .plugin('graphql')
  .service('extension')
  .shadowCRUD('api::document.document')
  .field('name')
  .disable()
```

##### Using getters

The following getters can be used to retrieve information about operations allowed on content-types:

| Content-type getter        | Description                                                       | Argument type | Possible argument values                                                                                              |
| -------------------------- | ----------------------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------- |
| `isEnabled()`              | Returns whether a content-type is enabled                         | -             | -                                                                                                                     |
| `isDisabled()`             | Returns whether a content-type is disabled                        | -             | -                                                                                                                     |
| `areQueriesEnabled()`      | Returns whether queries are enabled on a content-type             | -             | -                                                                                                                     |
| `areQueriesDisabled()`     | Returns whether queries are disabled on a content-type            | -             | -                                                                                                                     |
| `areMutationsEnabled()`    | Returns whether mutations are enabled on a content-type           | -             | -                                                                                                                     |
| `areMutationsDisabled()`   | Returns whether mutations are disabled on a content-type          | -             | -                                                                                                                     |
| `isActionEnabled(action)`  | Returns whether the passed `action` is enabled on a content-type  | String        | One value from the list:<ul><li>`create`</li><li>`find`</li><li>`findOne`</li><li>`update`</li><li>`delete`</li></ul> |
| `isActionDisabled(action)` | Returns whether the passed `action` is disabled on a content-type | String        | One value from the list:<ul><li>`create`</li><li>`find`</li><li>`findOne`</li><li>`update`</li><li>`delete`</li></ul> |

The following getters can be used to retrieve information about operations allowed on fields:

| Field getter          | Description                                   |
| --------------------- | --------------------------------------------- |
| `isEnabled()`         | Returns whether a field is enabled            |
| `isDisabled()`        | Returns whether a field is disabled           |
| `hasInputEnabled()`   | Returns whether a field has input enabled     |
| `hasOutputEnabled()`  | Returns whether a field has output enabled    |
| `hasFiltersEnabled()` | Returns whether a field has filtering enabled |

###### Extending the schema

The schema generated by the Content API can be extended by registering an extension.

This extension, defined either as an object or a function returning an object, will be used by the `use()` function exposed by the `extension` [service](/cms/backend-customization/services) provided with the GraphQL plugin.

The object describing the extension accepts the following parameters:

| Parameter         | Type   | Description                                                                                  |
| ----------------- | ------ | -------------------------------------------------------------------------------------------- |
| `types`           | Array  | Allows extending the schema types using <ExternalLink to="https://nexusjs.org/" text="Nexus"/>-based type definitions |
| `typeDefs`        | String | Allows extending the schema types using <ExternalLink to="https://graphql.org/learn/schema/" text="GraphQL SDL"/>     |
| `plugins`         | Array  | Allows extending the schema using Nexus <ExternalLink to="https://nexusjs.org/docs/plugins" text="plugins"/>          |
| `resolvers`       | Object | Defines custom resolvers                                                                     |
| `resolversConfig` | Object | Defines [configuration options for the resolvers](#custom-configuration-for-resolvers), such as [authorization](#authorization-configuration), [policies](#policies) and [middlewares](#middlewares) |

:::tip
The `types` and `plugins` parameters are based on <ExternalLink to="https://nexusjs.org/" text="Nexus"/>. To use them, register the extension as a function that takes `nexus` as a parameter:

<details>
<summary> Example: </summary>

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/src/index.js"

module.exports = {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');
    
    extensionService.shadowCRUD('api::restaurant.restaurant').disable();
    extensionService.shadowCRUD('api::category.category').disableQueries();
    extensionService.shadowCRUD('api::address.address').disableMutations();
    extensionService.shadowCRUD('api::document.document').field('locked').disable();
    extensionService.shadowCRUD('api::like.like').disableActions(['create', 'update', 'delete']);
    
    const extension = ({ nexus }) => ({
      types: [
        nexus.objectType({
          …
        }),
      ],
      plugins: [
        nexus.plugin({
          …
        })
      ]
    })

    strapi.plugin('graphql').service('extension').use(extension)
  }
}
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="./src/index.ts"

export default {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');
    
    extensionService.shadowCRUD('api::restaurant.restaurant').disable();
    extensionService.shadowCRUD('api::category.category').disableQueries();
    extensionService.shadowCRUD('api::address.address').disableMutations();
    extensionService.shadowCRUD('api::document.document').field('locked').disable();
    extensionService.shadowCRUD('api::like.like').disableActions(['create', 'update', 'delete']);
    
    const extension = ({ nexus }) => ({
      types: [
        nexus.objectType({
          …
        }),
      ],
      plugins: [
        nexus.plugin({
          …
        })
      ]
    })

    strapi.plugin('graphql').service('extension').use(extension)
  }
}
```

</TabItem>

</Tabs>

</details>
:::

###### Custom configuration for resolvers

A resolver is a GraphQL query or mutation handler (i.e. a function, or a collection of functions, that generate(s) a response for a GraphQL query or mutation). Each field has a default resolver.

When [extending the GraphQL schema](#extending-the-schema), the `resolversConfig` key can be used to define a custom configuration for a resolver, which can include:

* [authorization configuration](#authorization-configuration) with the `auth` key
* [policies with the `policies`](#policies) key
* and [middlewares with the `middlewares`](#middlewares) key

###### Authorization configuration

By default, the authorization of a GraphQL request is handled by the registered authorization strategy that can be either [API token](/cms/features/api-tokens) or through the [Users & Permissions plugin](#usage-with-the-users--permissions-plugin). The Users & Permissions plugin offers a more granular control.

<details>
<summary> Authorization with the Users & Permissions plugin</summary>

With the Users & Permissions plugin, a GraphQL request is allowed if the appropriate permissions are given.

For instance, if a 'Category' content-type exists and is queried through GraphQL with the `Query.categories` handler, the request is allowed if the appropriate `find` permission for the 'Categories' content-type is given.

To query a single category, which is done with the `Query.category` handler, the request is allowed if the the `findOne` permission is given.

Please refer to the user guide on how to [define permissions with the Users & Permissions plugin](/cms/features/rbac#editing-a-role).
</details>

To change how the authorization is configured, use the resolver configuration defined at `resolversConfig.[MyResolverName]`. The authorization can be configured:

* either with `auth: false` to fully bypass the authorization system and allow all requests,
* or with a `scope` attribute that accepts an array of strings to define the permissions required to authorize the request.

<details>
<summary> Examples of authorization configuration</summary>

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/src/index.js"

module.exports = {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');

    extensionService.use({
      resolversConfig: {
        'Query.categories': {
          /**
           * Querying the Categories content-type
           * bypasses the authorization system.
           */ 
          auth: false
        },
        'Query.restaurants': {
          /**
           * Querying the Restaurants content-type
           * requires the find permission
           * on the 'Address' content-type
           * of the 'Address' API
           */
          auth: {
            scope: ['api::address.address.find']
          }
        },
      }
    })
  }
}

```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/src/index.ts"

export default {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');

    extensionService.use({
      resolversConfig: {
        'Query.categories': {
          /**
           * Querying the Categories content-type
           * bypasses the authorization system.
           */ 
          auth: false
        },
        'Query.restaurants': {
          /**
           * Querying the Restaurants content-type
           * requires the find permission
           * on the 'Address' content-type
           * of the 'Address' API
           */
          auth: {
            scope: ['api::address.address.find']
          }
        },
      }
    })
  }
}

```

</TabItem>

</Tabs>
</details>

###### Policies

[Policies](/cms/backend-customization/policies) can be applied to a GraphQL resolver through the `resolversConfig.[MyResolverName].policies` key.

The `policies` key is an array accepting a list of policies, each item in this list being either a reference to an already registered policy or an implementation that is passed directly (see [policies configuration documentation](/cms/backend-customization/routes#policies)).

Policies directly implemented in `resolversConfig` are functions that take a `context` object and the `strapi` instance as arguments.
The `context` object gives access to:

* the `parent`, `args`, `context` and `info` arguments of the GraphQL resolver,
* Koa's <ExternalLink to="https://koajs.com/#context" text="context"/> with `context.http` and <ExternalLink to="https://koajs.com/#ctx-state" text="state"/> with `context.state`.

<details>
<summary> Example of GraphQL policies applied to resolvers</summary>

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/src/index.js"

module.exports = {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');

    extensionService.use({
      resolversConfig: {
        'Query.categories': {
          policies: [
            (context, { strapi }) => {
              console.log('hello', context.parent)
              /**
               * If 'categories' have a parent, the function returns true,
               * so the request won't be blocked by the policy.
               */ 
              return context.parent !== undefined;
            }
            /**
             * Uses a policy already created in Strapi.
             */
            "api::model.policy-name",

            /**
             * Uses a policy already created in Strapi with a custom configuration
             */
            {name:"api::model.policy-name", config: {/* all config values I want to pass to the strapi policy */} },
          ],
          auth: false,
        },
      }
    })
  }
}
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/src/index.ts"

export default {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');

    extensionService.use({
      resolversConfig: {
        'Query.categories': {
          policies: [
            (context, { strapi }) => {
              console.log('hello', context.parent)
              /**
               * If 'categories' have a parent, the function returns true,
               * so the request won't be blocked by the policy.
               */ 
              return context.parent !== undefined;
            }
            /**
             * Uses a policy already created in Strapi.
             */
            "api::model.policy-name",

            /**
             * Uses a policy already created in Strapi with a custom configuration
             */
            {name:"api::model.policy-name", config: {/* all the configuration values to pass to the strapi policy */} },
          ],
          auth: false,
        },
      }
    })
  }
}
```

</TabItem>

</Tabs>

</details>

###### Middlewares

[Middlewares](/cms/backend-customization/middlewares) can be applied to a GraphQL resolver through the `resolversConfig.[MyResolverName].middlewares` key. The only difference between the GraphQL and REST implementations is that the `config` key becomes `options`.  

The `middlewares` key is an array accepting a list of middlewares, each item in this list being either a reference to an already registered middleware or an implementation that is passed directly (see [middlewares configuration documentation](/cms/backend-customization/routes#middlewares)).

Middlewares directly implemented in `resolversConfig` can take the GraphQL resolver's <ExternalLink to="https://www.apollographql.com/docs/apollo-server/data/resolvers/#resolver-arguments" text="`parent`, `args`, `context` and `info` objects"/> as arguments.

:::tip
Middlewares with GraphQL can even act on nested resolvers, which offer a more granular control than with REST.
:::

<details>
<summary> Examples of GraphQL middlewares applied to a resolver</summary>

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title"/src/index.js"

module.exports = {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');

    extensionService.use({
      resolversConfig: {
        'Query.categories': {
          middlewares: [
            /**
             * Basic middleware example #1
             * Log resolving time in console
             */
            async (next, parent, args, context, info) => {
              console.time('Resolving categories');
              
              // call the next resolver
              const res = await next(parent, args, context, info);
              
              console.timeEnd('Resolving categories');

              return res;
            },
            /**
             * Basic middleware example #2
             * Enable server-side shared caching
             */
            async (next, parent, args, context, info) => {
              info.cacheControl.setCacheHint({ maxAge: 60, scope: "PUBLIC" });
              return next(parent, args, context, info);
            },
            /**
             * Basic middleware example #3
             * change the 'name' attribute of parent with id 1 to 'foobar'
             */
            (resolve, parent, ...rest) => {
              if (parent.id === 1) {
                return resolve({...parent, name: 'foobar' }, ...rest);
              }

              return resolve(parent, ...rest);
            }
            /**
             * Basic middleware example #4
             * Uses a middleware already created in Strapi.
             */
            "api::model.middleware-name",

            /**
             * Basic middleware example #5
             * Uses a middleware already created in Strapi with a custom configuration
             */
            { name: "api::model.middleware-name", options: { /* all config values I want to pass to the strapi middleware */ } },
          ],
          auth: false,
        },
      }
    })
  }
}
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title"/src/index.ts"

export default {
  register({ strapi }) {
    const extensionService = strapi.plugin('graphql').service('extension');

    extensionService.use({
      resolversConfig: {
        'Query.categories': {
          middlewares: [
            /**
             * Basic middleware example #1
             * Log resolving time in console
             */
            async (next, parent, args, context, info) => {
              console.time('Resolving categories');
              
              // call the next resolver
              const res = await next(parent, args, context, info);
              
              console.timeEnd('Resolving categories');

              return res;
            },
            /**
             * Basic middleware example #2
             * Enable server-side shared caching
             */
            async (next, parent, args, context, info) => {
              info.cacheControl.setCacheHint({ maxAge: 60, scope: "PUBLIC" });
              return next(parent, args, context, info);
            },
            /**
             * Basic middleware example #3
             * change the 'name' attribute of parent with id 1 to 'foobar'
             */
            (resolve, parent, ...rest) => {
              if (parent.id === 1) {
                return resolve({...parent, name: 'foobar' }, ...rest);
              }

              return resolve(parent, ...rest);
            }

            /**
             * Basic middleware example #4
             * Uses a middleware already created in Strapi.
             */
            "api::model.middleware-name",

            /**
             * Basic middleware example #5
             * Uses a middleware already created in Strapi with a custom configuration
             */
            {name:"api::model.middleware-name", options: {/* all the configuration values to pass to the middleware */} },
          ],
          auth: false,
        },
      }
    })
  }
}
```

</TabItem>

</Tabs>

</details>

##### Security

GraphQL is a query language allowing users to use a broader panel of inputs than traditional REST APIs. GraphQL APIs are inherently prone to security risks, such as credential leakage and denial of service attacks, that can be reduced by taking appropriate precautions.

### Disable introspection and Sandbox in production

In production environments, disabling the GraphQL Sandbox and the introspection query is strongly recommended.
If you haven't edited the [configuration file](#available-options), it is already disabled in production by default.

###### Limit max depth and complexity

A malicious user could send a query with a very high depth, which could overload your server. Use the `depthLimit` [configuration parameter](/cms/plugins/graphql#code-based-configuration) to limit the maximum number of nested fields that can be queried in a single request. By default, `depthLimit` is set to 10 but can be set to a higher value during testing and development.

:::tip
To increase GraphQL security even further, 3rd-party tools can be used. See the guide about <ExternalLink to="https://forum.strapi.io/t/use-graphql-armor-with-strapi/" text="using GraphQL Armor with Strapi on the forum"/>.
:::

## Usage

The GraphQL plugin adds a GraphQL endpoint accessible and provides access to a GraphQL playground, accessing at the `/graphql` route of the Strapi admin panel, to interactively build your queries and mutations and read documentation tailored to your content types. For detailed instructions on how to use the GraphQL Playground, please refer to the official <ExternalLink to="https://www.apollographql.com/docs/apollo-server/v2/testing/graphql-playground" text="Apollo Server documentation"/>.

### Usage with the Users & Permissions feature {#usage-with-the-users--permissions-plugin}

The [Users & Permissions feature](/cms/features/users-permissions) allows protecting the API with a full authentication process.

#### Registration

Usually you need to sign up or register before being recognized as a user then perform authorized requests.


<Request title="Mutation">

```graphql
mutation {
  register(input: { username: "username", email: "email", password: "password" }) {
    jwt
    user {
      username
      email
    }
  }
}
```

</Request>

You should see a new user is created in the `Users` collection type in your Strapi admin panel.

#### Authentication

To perform authorized requests, you must first get a JWT:

<Request title="Mutation">

```graphql
mutation {
  login(input: { identifier: "email", password: "password" }) {
    jwt
  }
}
```

</Request>

Then on each request, send along an `Authorization` header in the form of `{ "Authorization": "Bearer YOUR_JWT_GOES_HERE" }`. This can be set in the HTTP Headers section of your GraphQL Sandbox.

#### Usage with API tokens {#api-tokens}

To use API tokens for authentication, pass the token in the `Authorization` header using the format `Bearer your-api-token`.

:::note
Using API tokens in the the GraphQL Sandbox requires adding the authorization header with your token in the `HTTP HEADERS` tab:

```http
{
  "Authorization" : "Bearer <TOKEN>"
}
```

Replace `<TOKEN>` with your API token generated in the Strapi Admin panel.
:::

### GraphQL API

The GraphQL plugin adds a GraphQL endpoint that can accessed through Strapi's GraphQL API:

<CustomDocCardsWrapper>
<CustomDocCard icon="cube" title="GraphQL API" description="Learn how to use the Strapi's GraphQL API." link="/cms/api/graphql"/>
</CustomDocCardsWrapper>
