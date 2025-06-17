---
title: 플러그인 생성 및 설정
description: Plugin SDK를 사용하여 Strapi 플러그인을 빌드하고 게시하는 방법을 알아보세요
pagination_next: cms/plugins-development/plugin-structure
tags:
  - 가이드
  - 플러그인
  - 플러그인 SDK
  - 플러그인 개발
---

# 플러그인 생성

Strapi 5 플러그인을 생성하는 방법은 여러 가지가 있지만, 가장 빠르고 권장되는 방법은 Plugin SDK를 사용하는 것입니다.

Plugin SDK는 로컬 플러그인으로 사용하거나 NPM에 게시하거나 마켓플레이스에 제출하기 위한 플러그인 개발에 중점을 둔 명령어 세트입니다.

Plugin SDK를 사용하면 플러그인을 생성하기 전에 Strapi 프로젝트를 설정할 필요가 없습니다.

현재 가이드는 처음부터 플러그인을 생성하고, 기존 Strapi 프로젝트에 연결하고, 플러그인을 게시하는 과정을 다룹니다. 이미 기존 플러그인이 있다면, 대신 Plugin SDK 명령어를 활용하도록 플러그인 설정을 개조할 수 있습니다(사용 가능한 명령어의 전체 목록은 [Plugin SDK 참조](/cms/plugins-development/plugin-sdk)를 참고하세요).

:::note
이 가이드는 Strapi 프로젝트 외부에서 플러그인을 개발하고자 한다고 가정합니다. 하지만 기존 프로젝트 내에서 플러그인을 개발하고 싶다면 단계는 거의 동일합니다. [모노레포를 사용하지 않는](#monorepo) 경우 단계는 정확히 동일합니다.
:::

:::prerequisites
<ExternalLink to="https://www.npmjs.com/package/yalc" text="yalc"/>가 전역으로 설치되어 있어야 합니다(`npm install -g yalc` 또는 `yarn global add yalc`로 설치).
:::

## Plugin SDK 시작하기

Plugin SDK는 플러그인을 생성하고, 기존 Strapi 프로젝트에 연결하고, 게시를 위해 빌드하는 데 도움을 줍니다.

명령어와 매개변수의 전체 목록은 [Plugin SDK 참조](/cms/plugins-development/plugin-sdk)에서 확인할 수 있습니다. 현재 페이지는 주요 명령어 사용을 안내합니다.

### 플러그인 생성

플러그인을 생성하려면, 생성할 위치의 상위 디렉터리에 있는지 확인하고 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="Yarn">

```bash
yarn dlx @strapi/sdk-plugin init my-strapi-plugin
```

</TabItem>

<TabItem value="npm" label="NPM">

```bash
npx @strapi/sdk-plugin init my-strapi-plugin
```

</TabItem>

</Tabs>

`my-strapi-plugin` 경로는 플러그인을 호출하고 싶은 이름과 생성할 경로로 대체할 수 있습니다(예: `code/strapi-plugins/my-new-strapi-plugin`).

플러그인 설정을 도와주는 일련의 프롬프트가 실행됩니다. 모든 옵션에 yes를 선택하면 최종 구조는 기본 [플러그인 구조](/cms/plugins-development/plugin-structure)와 유사해집니다.

### 플러그인을 프로젝트에 연결

개발 중에 플러그인을 테스트하기 위해 권장되는 접근 방법은 Strapi 프로젝트에 연결하는 것입니다.

플러그인을 프로젝트에 연결하는 것은 `watch:link` 명령어로 수행됩니다. 이 명령어는 플러그인을 Strapi 프로젝트에 연결하는 방법에 대한 설명을 출력합니다.

새 터미널 창에서 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="Yarn">

```bash
cd /path/to/strapi/project
yarn dlx yalc add --link my-strapi-plugin && yarn install
```

</TabItem>

<TabItem value="npm" label="NPM">

```bash
cd /path/to/strapi/project
npx yalc add --link my-strapi-plugin && npm install
```

</TabItem>

</Tabs>

:::note
위 예제에서는 플러그인을 프로젝트에 연결할 때 플러그인의 이름(`my-strapi-plugin`)을 사용합니다. 이는 폴더 이름이 아닌 패키지 이름입니다.
:::

이 플러그인은 `node_modules`를 통해 설치되므로 명시적으로 `plugins` [구성 파일](/cms/configurations/plugins)에 추가할 필요가 없으며, [`develop 명령어`](/cms/cli#strapi-develop)를 실행하여 Strapi 프로젝트를 시작하면 자동으로 플러그인을 인식합니다.

이제 플러그인이 프로젝트에 연결되었으므로, `yarn develop` 또는 `npm run develop`을 실행하여 Strapi 애플리케이션을 시작하세요.

이제 원하는 대로 플러그인을 개발할 준비가 되었습니다! 서버 변경사항을 만들고 있다면, 효과를 보려면 서버를 재시작해야 합니다.

### 게시를 위한 플러그인 빌드

플러그인을 게시할 준비가 되면 빌드해야 합니다. 이를 위해 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="Yarn">

```bash
yarn build && yarn verify
```

</TabItem>

<TabItem value="npm" label="NPM">

```bash
npm run build && npm run verify
```

</TabItem>

</Tabs>

위 명령어는 플러그인을 빌드할 뿐만 아니라 출력이 유효하고 게시할 준비가 되었는지 검증합니다. 그런 다음 다른 패키지와 마찬가지로 플러그인을 NPM에 게시할 수 있습니다.

## 모노레포 환경에서 Plugin SDK 작업하기 {#monorepo}

모노레포 환경에서 플러그인을 개발하는 경우, 모노레포 워크스페이스 설정이 심볼릭 링크를 처리하므로 `watch:link` 명령어를 사용할 필요가 없습니다. 대신 `watch` 명령어를 사용할 수 있습니다.

하지만 관리자 코드를 작성하는 경우, 관리자 패널 컨텍스트에서 작업하기 쉽게 하기 위해 플러그인의 소스 코드를 대상으로 하는 `alias`를 추가할 수 있습니다:

```ts
import path from 'node:path';

export default (config, webpack) => {
  config.resolve.alias = {
    ...config.resolve.alias,
    'my-strapi-plugin': path.resolve(
      __dirname,
      // 플러그인이 로컬에 있다고 가정했습니다.
      '../plugins/my-strapi-plugin/admin/src'
    ),
  };

  return config;
};
```

:::caution
서버는 플러그인 코드를 가져오기 위해 `server/src/index.ts|js` 파일을 봅니다. 코드가 트랜스파일되지 않으면 서버가 플러그인을 찾을 수 없으므로 `watch` 명령어를 사용해야 합니다.
:::

### 로컬 플러그인 구성

Plugin SDK는 주로 플러그인을 로컬이 아닌 곳에서 개발하도록 설계되어 있으므로, 로컬 플러그인의 경우 구성을 수동으로 조정해야 합니다.

플러그인을 로컬에서 개발할 때(`@strapi/sdk-plugin` 사용), 플러그인 구성 파일은 다음 예제와 같습니다:

```js title="/config/plugins.js|ts"
myplugin: {
  enabled: true,
  resolve: `./src/plugins/local-plugin`,
},
```

하지만 이 설정은 때때로 다음과 같은 오류를 일으킬 수 있습니다:

```js
Error: 'X must be used within StrapiApp';
```

이 오류는 플러그인이 핵심 Strapi 기능을 가져오려고 시도할 때 자주 발생합니다. 예를 들어:

```js
import { unstable_useContentManagerContext as useContentManagerContext } from '@strapi/strapi/admin';
```

이 문제를 해결하려면 플러그인에서 `@strapi/strapi`를 개발 의존성으로 제거하세요. 이렇게 하면 플러그인이 메인 애플리케이션과 동일한 Strapi 핵심 모듈 인스턴스를 사용하여 충돌과 관련 오류를 방지할 수 있습니다.

## Plugin SDK 없이 모노레포 환경에서 로컬 플러그인 설정

모노레포에서는 플러그인 루트에 2개의 진입점 파일을 추가하여 Plugin SDK를 사용하지 않고 로컬 플러그인을 구성할 수 있습니다:

- 서버 진입점: `strapi-server.js|ts`
- 관리자 진입점: `strapi-admin.js|ts`

### 서버 진입점

서버 진입점 파일은 플러그인의 서버 사이드 기능을 초기화합니다. `strapi-server.js`(또는 TypeScript 변형)의 예상 구조는 다음과 같습니다:

```js
module.exports = () => {
  return {
    register,
    config,
    controllers,
    // ... 기타 서버 관련 내보내기
  };
};
```

### 관리자 진입점

관리자 진입점 파일은 Strapi 관리자 패널에서 플러그인을 설정하는 데 필요한 구조를 설정합니다. `strapi-admin.js`(또는 TypeScript 변형)의 예상 구조는 다음과 같습니다:

```js
export default {
  register(app) {},
  bootstrap() {},
  registerTrads({ locales }) {},
};
```

이 객체는 플러그인을 관리자 애플리케이션에 등록하고, 부트스트랩 작업을 수행하고, 번역을 처리하는 방법을 포함합니다. 자세한 내용은 [관리자 패널 API 참조](/cms/plugins-development/admin-panel-api)를 참고하세요.

:::tip
완전한 예제는 <ExternalLink to="https://github.com/strapi/strapi/tree/develop/examples/getstarted/src/plugins/local-plugin" text="strapi/strapi 저장소의 예제 설정" />을 확인하세요.
:::
