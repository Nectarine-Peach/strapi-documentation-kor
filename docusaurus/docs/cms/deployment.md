---
title: 배포
displayed_sidebar: cmsSidebar
description: Strapi를 사용하여 로컬에서 개발하고 다양한 호스팅 옵션으로 Strapi를 배포하는 방법을 알아보세요.
tags:
- 데이터베이스 배포
- 배포
- 프로젝트 생성
- 호스팅 제공업체
- 호스팅 서버
---

import DatabaseRequire from '/docs/snippets/database-require.md'
import HardwareRequire from '/docs/snippets/hardware-require.md'
import OperatingSystemRequire from '/docs/snippets/operating-system-require.md'
import InstallPrereq from '/docs/snippets/installation-prerequisites.md'

# 배포

Strapi는 프로젝트나 애플리케이션을 위한 많은 배포 옵션을 제공합니다. Strapi 애플리케이션은 기존 호스팅 서버나 선호하는 호스팅 제공업체에 배포할 수 있습니다.

다음 문서는 여러 일반적인 호스팅 옵션으로 배포하기 위해 Strapi를 준비하는 방법의 기본 사항을 다룹니다.

:::strapi Strapi Cloud
[Strapi Cloud](/cloud/intro)를 사용하여 프로젝트를 빠르게 배포하고 호스팅할 수 있습니다.
:::

:::tip
콘텐츠 타입 빌더로 콘텐츠 구조를 이미 생성하고 콘텐츠 관리자를 통해 로컬(개발) Strapi 인스턴스에 일부 데이터를 추가했다면, [데이터 관리 시스템](/cms/features/data-management)을 활용하여 한 Strapi 인스턴스에서 다른 인스턴스로 데이터를 전송할 수 있습니다.

또 다른 가능한 워크플로는 먼저 로컬에서 콘텐츠 구조를 생성하고, 프로젝트를 git 기반 저장소에 푸시하고, 변경사항을 프로덕션에 배포한 후, 그 다음에 프로덕션 인스턴스에 콘텐츠를 추가하는 것입니다.
:::

## 일반 가이드라인

### 하드웨어 및 소프트웨어 요구사항

Strapi에 최적의 환경을 제공하기 위해 다음 요구사항이 개발(로컬), 준비 및 프로덕션 워크플로에 적용됩니다.

<InstallPrereq />

- OS를 위한 표준 빌드 도구 (대부분의 Debian 기반 시스템에서 `build-essentials` 패키지)
- 서버의 하드웨어 사양 (CPU, RAM, 스토리지):

  <HardwareRequire components={props.components} />

- 지원되는 데이터베이스 버전:
<DatabaseRequire components={props.components} />

:::strapi 데이터베이스 배포
Strapi와 함께 데이터베이스 배포는 [데이터베이스 가이드](/cms/configurations/database#databases-installation)에서 다룹니다.
:::

- 지원되는 운영 체제:

  <OperatingSystemRequire components={props.components} />

### 애플리케이션 구성

<br/>

#### 1. 구성

환경에 따라 애플리케이션을 구성하기 위해 환경 변수를 사용하는 것을 권장합니다. 예를 들어:

```js title="/config/server.js"

module.exports = ({ env }) => ({
  host: env('HOST', '0.0.0.0'),
  port: env.int('PORT', 1337),
});
```

그런 다음 `.env` 파일을 생성하거나 선택한 배포 플랫폼에서 환경 변수를 직접 설정할 수 있습니다:

```
HOST=10.0.0.1
PORT=1338
```

:::tip
구성 세부사항에 대해 자세히 알아보려면 [구성](/cms/configurations) 문서를 참조하세요.
:::

#### 2. 서버 시작

프로덕션에서 서버를 실행하기 전에 프로덕션용 관리자 패널을 빌드해야 합니다:

<Tabs groupId="yarn-npm-windows">

<TabItem value="yarn" label="yarn">

```bash
NODE_ENV=production yarn build
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
NODE_ENV=production npm run build
```

</TabItem>

<TabItem value="windows" label="windows">

```bash
npm install cross-env
```

그 다음 `package.json` 스크립트 섹션에서:

```bash
"build:win": "cross-env NODE_ENV=production npm run build",
```

그리고 실행:

```bash
npm run build:win
```

</TabItem>
</Tabs>

`production` 설정으로 서버를 실행합니다:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
NODE_ENV=production yarn start
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
NODE_ENV=production npm run start
```

</TabItem>

<TabItem value="windows" label="windows">

```bash
npm install cross-env
```

그 다음 `package.json` 스크립트 섹션에서:

```bash
"start:win": "cross-env NODE_ENV=production npm start",
```

그리고 실행:

```bash
npm run start:win
```

</TabItem>

</Tabs>

:::caution
프로세스 관리를 위해 <ExternalLink to="https://github.com/Unitech/pm2/" text="pm2"/>를 사용하는 것을 강력히 권장합니다.
:::

`npm run start` 대신 `node server.js`를 실행할 수 있는 server.js 파일이 필요하다면 다음과 같이 `./server.js` 파일을 생성하세요:

```js title="path: ./server.js"

const strapi = require('@strapi/strapi');
strapi.createStrapi(/* {...} */).start();
```

:::caution

`TypeScript` 기반 프로젝트를 개발하고 있다면 서버를 시작하기 위해 `distDir` 옵션을 제공해야 합니다.
자세한 정보는 [TypeScript 문서](/cms/typescript/development#use-the-createstrapi-factory)를 참고하세요.
:::

### 고급 구성

API와 다른 서버에서 관리자를 호스팅하려면 [이 전용 섹션](/cms/configurations/admin-panel#deploy-on-different-servers)을 살펴보세요.

## 추가 리소스

:::prerequisites
* Strapi 프로젝트가 [생성](/cms/installation)되어 있고 코드가 GitHub에 호스팅되어 있습니다.
* [일반 배포 가이드라인](/cms/deployment#general-guidelines)을 읽었습니다.
:::

Strapi 웹사이트의 <ExternalLink to="https://strapi.io/integrations" text="통합 페이지"/>에는 다음 3rd 파티 플랫폼에서 Strapi를 배포하는 방법을 포함하여 Strapi를 많은 리소스와 통합하는 방법에 대한 정보가 포함되어 있습니다:

<CustomDocCard emoji="🔗" small title="Deploy Strapi on AWS"  link="https://strapi.io/integrations/aws" />

<CustomDocCard emoji="🔗" small title="Deploy Strapi on Azure" link="https://strapi.io/integrations/azure" />

<CustomDocCard emoji="🔗" small title="Deploy Strapi on DigitalOcean App Platform"  link="https://strapi.io/integrations/digital-ocean" />

<CustomDocCard emoji="🔗" small title="Deploy Strapi on Heroku" link="https://strapi.io/integrations/heroku" />

<br/>

In addition, community-maintained guides for additional providers are available in the <ExternalLink to="https://forum.strapi.io/c/community-guides/28" text="Strapi Forum"/>. This includes the following guides:

<CustomDocCard emoji="🔗" small title="Proxying with Caddy" link="https://forum.strapi.io/t/caddy-proxying-with-strapi/" />
<CustomDocCard emoji="🔗" small title="Proxying with HAProxy" link="https://forum.strapi.io/t/haproxy-proxying-with-strapi/" />
<CustomDocCard emoji="🔗" small title="Proxying with NGinx" link="https://forum.strapi.io/t/nginx-proxing-with-strapi/" />
<CustomDocCard emoji="🔗" small title="Using the PM2 process manager" link="https://forum.strapi.io/t/how-to-use-pm2-process-manager-with-strapi/" />

<br/>

The following external guide(s), not officially maintained by Strapi, might also help deploy Strapi on various environments:

<CustomDocCard icon="arrow-square-out" small title="[Microsoft Community] Deploying on Azure" link="https://techcommunity.microsoft.com/blog/appsonazureblog/strapi-on-app-service-quick-start/4401398" />

:::strapi Multi-tenancy
If you're looking for multi-tenancy options, the Strapi Blog has a <ExternalLink text="comprehensive guide" to="https://strapi.io/blog/multi-tenancy-in-strapi-a-comprehensive-guide" />.
:::