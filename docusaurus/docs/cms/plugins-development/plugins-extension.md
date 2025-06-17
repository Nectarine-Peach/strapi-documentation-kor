---
title: 플러그인 확장
displayed_sidebar: cmsSidebar
tags:
- bootstrap 함수
- 컨트롤러
- 미들웨어
- 정책
- 플러그인
- 플러그인 개발
- register 함수 
- 서비스
---

# 플러그인 확장

Strapi는 [마켓플레이스](/cms/plugins/installing-plugins-via-marketplace#installing-marketplace-plugins-and-providers)에서 설치하거나 npm 패키지로 설치할 수 있는 플러그인들을 제공합니다. 또한 자신만의 플러그인을 만들거나([플러그인 개발](/cms/plugins-development/developing-plugins) 참고) 기존 플러그인을 확장할 수도 있습니다.

:::warning
* 플러그인 업데이트로 인해 이 플러그인의 확장 기능이 작동하지 않을 수 있습니다.
* 새로운 버전의 Strapi는 필요시 마이그레이션 가이드와 함께 릴리즈되지만, 이러한 가이드는 플러그인 확장을 다루지 않습니다. 광범위한 커스터마이징이 필요한 경우 플러그인을 포크하는 것을 고려하세요.
* 현재 플러그인의 관리자 패널 부분은 <ExternalLink to="https://www.npmjs.com/package/patch-package" text="patch-package"/>를 사용해서만 확장할 수 있지만, 이렇게 하면 향후 Strapi 버전에서 플러그인이 작동하지 않을 수 있다는 점을 고려하세요.
:::

플러그인 확장 코드는 `./src/extensions` 폴더에 있습니다([프로젝트 구조](/cms/project-structure) 참고). 일부 플러그인은 수정 준비가 된 파일들을 자동으로 생성합니다.

<details> 
<summary>확장 폴더 구조 예시</summary>

```bash
/extensions
  /some-plugin-to-extend
    strapi-server.js|ts
    /content-types
      /some-content-type-to-extend
        model.json
      /another-content-type-to-extend
        model.json
  /another-plugin-to-extend
    strapi-server.js|ts
```
</details>

플러그인은 2가지 방법으로 확장할 수 있습니다:

- [플러그인의 콘텐츠 타입 확장](#extending-a-plugins-content-types)
- [플러그인의 인터페이스 확장](#extending-a-plugins-interface) (예: 컨트롤러, 서비스, 정책, 미들웨어 등을 추가)

## 플러그인의 콘텐츠 타입 확장

플러그인의 콘텐츠 타입은 2가지 방법으로 확장할 수 있습니다: `strapi-server.js|ts` 내에서 프로그래밍 인터페이스를 사용하거나 콘텐츠 타입 스키마를 재정의하는 방법입니다.

콘텐츠 타입의 최종 스키마는 다음 로딩 순서에 따라 결정됩니다:

1. 원본 플러그인의 콘텐츠 타입
2. `./src/extensions/plugin-name/content-types/content-type-name/schema.json`에 정의된 [스키마](/cms/backend-customization/models#model-schema) 선언으로 재정의된 콘텐츠 타입
3. [`strapi-server.js|ts`에서 내보낸 `content-types` 키](/cms/plugins-development/server-api#content-types)의 콘텐츠 타입 선언
4. Strapi 애플리케이션의 [`register()` 함수](/cms/configurations/functions#register)에서의 콘텐츠 타입 선언

플러그인의 [콘텐츠 타입](/cms/backend-customization/models)을 덮어쓰려면:

1. _(선택사항)_ 폴더가 아직 없다면 앱 루트에 `./src/extensions` 폴더를 생성합니다.
2. 확장할 플러그인과 같은 이름의 하위 폴더를 생성합니다.
3. `content-types` 하위 폴더를 생성합니다.
4. `content-types` 하위 폴더 안에 덮어쓸 콘텐츠 타입과 같은 [singularName](/cms/backend-customization/models#model-information)으로 또 다른 하위 폴더를 생성합니다.
5. 이 `content-types/name-of-content-type` 하위 폴더 안에서 `schema.json` 파일로 콘텐츠 타입의 새로운 스키마를 정의합니다([스키마](/cms/backend-customization/models#model-schema) 문서 참고).
6. _(선택사항)_ 덮어쓸 각 콘텐츠 타입에 대해 4단계와 5단계를 반복합니다.

## 플러그인의 인터페이스 확장

Strapi 애플리케이션이 초기화될 때, 플러그인, 확장 및 전역 라이프사이클 함수 이벤트는 다음 순서로 발생합니다:

1. 플러그인이 로드되고 인터페이스가 노출됩니다.
2. `./src/extensions`의 파일들이 로드됩니다.
3. `./src/index.js|ts`의 `register()`와 `bootstrap()` 함수가 호출됩니다.

플러그인의 인터페이스는 2단계(`./src/extensions` 내에서) 또는 3단계(`./src/index.js|ts` 내에서)에서 확장할 수 있습니다.

:::note
Strapi 프로젝트가 TypeScript 기반이라면, `index` 파일이 TypeScript 확장자(즉, `src/index.ts`)를 가지고 있는지 확인하세요. 그렇지 않으면 컴파일되지 않습니다.
:::

### 확장 폴더 내에서

`./src/extensions` 폴더를 사용하여 플러그인의 서버 인터페이스를 확장하려면:

1. _(선택사항)_ 폴더가 아직 없다면 앱 루트에 `./src/extensions` 폴더를 생성합니다.
2. 확장할 플러그인과 같은 이름의 하위 폴더를 생성합니다.
3. [서버 API](/cms/plugins-development/server-api)를 사용하여 플러그인의 백엔드를 확장하기 위한 `strapi-server.js|ts` 파일을 생성합니다.
4. 이 파일 내에서 함수를 정의하고 내보냅니다. 이 함수는 `plugin` 인터페이스를 인수로 받아 확장할 수 있습니다.

<details>
<summary>백엔드 확장 예시</summary>

```js title="./src/extensions/some-plugin-to-extend/strapi-server.js|ts"

module.exports = (plugin) => {
  plugin.controllers.controllerA.find = (ctx) => {};

  plugin.policies[newPolicy] = (ctx) => {};

  plugin.routes['content-api'].routes.push({
    method: 'GET',
    path: '/route-path',
    handler: 'controller.action',
  });

  return plugin;
};
```
</details>

### register와 bootstrap 함수 내에서

`./src/index.js|ts` 내에서 플러그인의 인터페이스를 확장하려면, 전체 프로젝트의 `bootstrap()`과 `register()` [함수](/cms/configurations/functions)를 사용하고, [게터](/cms/plugins-development/server-api#usage)를 통해 프로그래밍 방식으로 인터페이스에 접근합니다.

<details>
<summary>./src/index.js|ts 내에서 플러그인의 콘텐츠 타입 확장 예시</summary>

```js title="./src/index.js|ts"

module.exports = {
  register({ strapi }) {
    const contentTypeName = strapi.contentType('plugin::my-plugin.content-type-name')  
    contentTypeName.attributes = {
      // 이전에 정의된 속성들을 확산
      ...contentTypeName.attributes,
      // 새로운 속성 추가 또는 기존 속성 재정의
      'toto': {
        type: 'string',
      }
    }
  },
  bootstrap({ strapi }) {},
};
```
</details>

