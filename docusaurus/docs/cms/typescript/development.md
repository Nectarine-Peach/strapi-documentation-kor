---
title: TypeScript 개발
description: Strapi 5에서 TypeScript 사용에 대해 자세히 알아보세요
displayed_sidebar: cmsSidebar
tags:
- strapi() 팩토리
- strapi.compile() 함수
- 타입스크립트
- 플러그인 개발
---

# Strapi와 함께하는 TypeScript 개발

[TypeScript](/cms/typescript) 기반 애플리케이션을 Strapi로 개발하면서 다음을 할 수 있습니다:

- 자동 완성 기능이 있는 [`Strapi`](#use-strapi-typescript-typings) 클래스의 [타이핑에 접근](#use-strapi-typescript-typings),
- 프로젝트의 콘텐츠 타입에 대한 [타이핑 생성](#generate-typings-for-content-types-schemas),
- [프로그래밍 방식으로 Strapi 시작](#start-strapi-programmatically),
- [플러그인 개발](#develop-a-plugin-using-typescript)을 위한 TypeScript 관련 지침 따르기.

:::strapi 문서와 엔트리
TypeScript 기반 프로젝트로 문서와 엔트리를 조작하는 방법에 대한 자세한 정보와 모범 사례는 [전용 가이드](/cms/typescript/documents-and-entries)에서 찾을 수 있습니다.
:::

## `Strapi` TypeScript 타이핑 사용

Strapi는 TypeScript 개발 경험을 향상시키기 위해 `Strapi` 클래스에 타이핑을 제공합니다. 이러한 타이핑은 개발 중 자동으로 제안을 제공하는 자동 완성 기능을 제공합니다.

Strapi 애플리케이션을 개발하면서 TypeScript 기반 자동 완성을 경험하려면 다음을 시도해볼 수 있습니다:

1. 코드 에디터에서 `./src/index.ts` 파일을 열기.
2. 전역 `register` 메서드 내에서 `strapi` 인수를 `Strapi` 타입으로 선언:

    ```typescript title="./src/index.ts"
    import { Strapi } from '@strapi/strapi';

    export default {
      register({ strapi }: { strapi: Strapi }) {
        // ...
      },
    };
    ```

3. `register` 메서드의 본문 내에서 `strapi.`를 입력하기 시작하고 키보드 화살표를 사용하여 사용 가능한 속성을 탐색.

4. 목록에서 `runLifecyclesFunctions`를 선택.

5. `strapi.runLifecyclesFunctions` 메서드가 추가되면, 코드 에디터에서 사용 가능한 라이프사이클 타입 목록 (즉, `register`, `bootstrap`, `destroy`)을 반환합니다. 키보드 화살표를 사용하여 라이프사이클 중 하나를 선택하면 코드가 자동 완성됩니다.

## 콘텐츠 타입 스키마에 대한 타이핑 생성

프로젝트 스키마에 대한 타이핑을 생성하려면 [`ts:generate-types` CLI 명령어](/cms/cli#strapi-ts)를 사용하세요. `ts:generate-types` 명령어는 프로젝트 루트에 `types` 폴더를 생성하여 프로젝트에 대한 타이핑을 저장합니다. 선택적 `--debug` 플래그는 생성된 스키마의 상세한 테이블을 반환합니다.

`ts:generate-types`를 사용하려면 프로젝트 루트의 터미널에서 다음 코드를 실행하세요:

<Tabs groupId="yarn-npm">
<TabItem value="npm">

```sh
npm run strapi ts:generate-types --debug #추가 로깅을 표시하는 선택적 플래그
```

</TabItem>

<TabItem value="yarn">

```sh
yarn strapi ts:generate-types --debug #추가 로깅을 표시하는 선택적 플래그
```

</TabItem>
</Tabs>

:::tip 팁: 자동으로 타입 생성하기
[`config/typescript.js|ts` 구성 파일](/cms/configurations/typescript#strapi-specific-configuration-for-typescript)에 `autogenerate: true`를 추가하면 서버 재시작 시 타입이 자동으로 생성됩니다.
:::

:::tip 팁: 프론트엔드 애플리케이션에서 타입 사용하기
프론트엔드 애플리케이션에서 Strapi 타입을 사용하려면, Strapi가 공식 솔루션을 구현할 때까지 <ExternalLink to="https://github.com/strapi-community/strapi-typed-fronend" text="우회 방법을 사용"/>할 수 있습니다.
:::

### 생성된 타입으로 인한 빌드 문제 해결

생성된 타입은 제외할 수 있어서 Entity Service가 이를 사용하지 않고 콘텐츠 타입에서 사용 가능한 실제 속성을 확인하지 않는 느슨한 타입으로 대체됩니다.

이를 위해 Strapi 프로젝트의 `tsconfig.json`을 편집하고 `exclude` 배열에 `types/generated/**`를 추가하세요:

```json title="./tsconfig.json"
  // ...
  "exclude": [
    "node_modules/",
    "build/",
    "dist/",
    ".cache/",
    ".tmp/",
    "src/admin/",
    "**/*.test.ts",
    "src/plugins/**",
    "types/generated/**"
  ]
  // ...
```

하지만 여전히 프로젝트에서 생성된 타입을 사용하고 싶지만 Strapi가 이를 사용하지 않도록 하려면, 생성된 타입을 복사하여 `generated` 디렉터리 밖에 붙여넣고(타입이 재생성될 때 덮어쓰이지 않도록) 파일 하단에서 `declare module '@strapi/types'`를 제거하는 우회 방법을 사용할 수 있습니다.

:::warning
타입은 호환성 문제를 피하기 위해 `@strapi/strapi`에서만 가져와야 합니다. `@strapi/types`의 타입은 내부 사용 전용이며 예고 없이 변경될 수 있습니다.
:::

## 프로그래밍 방식으로 Strapi 시작

TypeScript 프로젝트에서 프로그래밍 방식으로 Strapi를 시작하려면 Strapi 인스턴스에 컴파일된 코드 위치가 필요합니다. 이 섹션에서는 컴파일된 코드 디렉터리를 설정하고 지정하는 방법을 설명합니다.

### `strapi()` 팩토리 사용 {#use-the-createstrapi-factory}

`strapi()` 팩토리를 사용하여 프로그래밍 방식으로 Strapi를 실행할 수 있습니다. TypeScript 프로젝트의 코드는 특정 디렉터리에 컴파일되므로, 컴파일된 코드를 읽어야 하는 위치를 나타내기 위해 `distDir` 매개변수를 팩토리에 전달해야 합니다:

```js title="./server.js"

const strapi = require('@strapi/strapi');
const app = strapi.createStrapi({ distDir: './dist' });
app.start(); 
```

### `strapi.compile()` 함수 사용

`strapi.compile()` 함수는 주로 Strapi 인스턴스를 시작하고 프로젝트에 TypeScript 코드가 포함되어 있는지 감지해야 하는 도구를 개발하는 데 사용해야 합니다. `strapi.compile()`은 프로젝트 언어를 자동으로 감지합니다. 프로젝트 코드에 TypeScript 코드가 포함되어 있으면, `strapi.compile()`은 코드를 컴파일하고 Strapi가 필요로 하는 디렉터리에 대한 특정 값을 가진 컨텍스트를 반환합니다:

```js
const strapi = require('@strapi/strapi');

strapi.compile().then(appContext => strapi(appContext).start());
```

## TypeScript를 사용하여 플러그인 개발

새로운 플러그인은 [플러그인 개발 문서](/cms/plugins-development/developing-plugins)를 따라 생성할 수 있으며, CLI 도구에서 프롬프트를 받을 때 "TypeScript"를 선택하면 됩니다.

TypeScript 애플리케이션에는 2가지 중요한 구별점이 있습니다:

- 플러그인을 생성한 후, 플러그인 디렉터리 `src/admin/plugins/[my-plugin-name]`에서 `yarn` 또는 `npm install`을 실행하여 플러그인의 종속성을 설치합니다.
- 플러그인 디렉터리 `src/admin/plugins/[my-plugin-name]`에서 `yarn build` 또는 `npm run build`를 실행하여 플러그인을 포함한 관리자 패널을 빌드합니다.

:::note
초기 설치 후에는 `yarn` 또는 `npm install` 명령어를 반복할 필요가 없습니다. `yarn build` 또는 `npm run build` 명령어는 관리자 패널에 영향을 주는 플러그인 개발을 구현하는 데 필요합니다.
:::
