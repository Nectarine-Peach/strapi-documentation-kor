---
title: 관리자 패널 커스터마이징 - URL, 호스트, 경로 설정
description: Strapi 관리자 패널에 접근하는 URL, 호스트, 경로를 설정하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: URL, 호스트, 포트 설정
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징

---

# 관리자 패널 커스터마이징: 호스트, 포트, 경로 설정

기본적으로 Strapi의 [관리자 패널](/cms/admin-panel-customization)은 <ExternalLink to="http://localhost:1337/admin" text="http://localhost:1337/admin"/> 경로로 노출됩니다. 보안 및 환경에 따라 호스트, 포트, 경로를 변경할 수 있습니다.

## 관리자 패널 경로만 변경하기

Strapi의 백엔드 서버와 관리자 패널 서버를 별도 서버에 배포하지 않았다면([배포 문서 참고](/cms/configurations/admin-panel#deploy-on-different-servers)), 기본적으로:

- Strapi의 백엔드 서버와 관리자 패널 서버는 동일한 호스트와 포트(`http://localhost:1337/`)에서 동작합니다.
- 관리자 패널은 `/admin` 경로, 백엔드 서버는 `/api` 경로로 접근할 수 있습니다.

관리자 패널을 예를 들어 `http://localhost:1337/dashboard` 경로로 접근하고 싶다면, [관리자 패널 설정 파일](/cms/configurations/admin-panel)의 `url` 속성을 아래와 같이 지정하세요:

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  // … 기타 설정
  url: "/dashboard",
});
```

기본적으로 백엔드 서버와 관리자 패널 서버가 동일한 호스트와 포트에서 동작하므로, [서버 설정 파일](/cms/configurations/server)의 `host`와 `port` 값을 변경하지 않았다면 `config/admin.[ts|js]` 파일만 수정하면 됩니다. 서버 설정 예시는 다음과 같습니다:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/config/server.js"
module.exports = ({ env }) => ({
  host: env("HOST", "0.0.0.0"),
  port: env.int("PORT", 1337),
});
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="/config/server.ts"
export default ({ env }) => ({
  host: env("HOST", "0.0.0.0"),
  port: env.int("PORT", 1337),
});
```

</TabItem>
</Tabs>

## 관리자 패널 호스트와 포트 변경하기

Strapi의 관리자 패널과 백엔드 서버를 별도 서버에 배포하는 경우([배포 문서 참고](/cms/configurations/admin-panel#deploy-on-different-servers)), 관리자 패널의 호스트와 포트를 별도로 지정해야 합니다.

이 경우, 관리자 패널 설정 파일에서 아래와 같이 값을 지정합니다. 예를 들어 `my-host.com:3000`에서 관리자 패널을 운영하려면:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="./config/admin.js"
module.exports = ({ env }) => ({
  host: "my-host.com",
  port: 3000,
  // 기본 /admin 경로 대신 다른 경로를 지정할 수도 있습니다 👇
  // url: '/dashboard' 
});
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```js title="./config/admin.ts"
export default ({ env }) => ({
  host: "my-host.com",
  port: 3000,
  // 기본 /admin 경로 대신 다른 경로를 지정할 수도 있습니다 👇
  // url: '/dashboard'
});
```

</TabItem>
</Tabs>

<br/>

:::strapi 기타 관리자 패널 설정
`/config/admin.[ts|js]` 파일에서는 이 외에도 다양한 설정이 가능합니다. 자세한 내용은 [관리자 패널 설정 문서](/cms/configurations/admin-panel)를 참고하세요.
:::
