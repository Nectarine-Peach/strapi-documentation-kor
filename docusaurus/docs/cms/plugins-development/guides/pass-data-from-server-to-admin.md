---
title: Strapi 플러그인으로 서버에서 관리자 패널로 데이터 전달하는 방법
description: Strapi 플러그인으로 서버에서 관리자 패널로 데이터를 전달하는 방법을 알아보세요
sidebar_label: 서버에서 관리자로 데이터 전달
displayed_sidebar: cmsSidebar
tags:
- 관리자 패널
- 관리자 라우트
- 콘텐츠 타입
- 가이드
- 플러그인
- 플러그인 개발 가이드
---

import NotV5 from '/docs/snippets/_not-updated-to-v5.md'

# Strapi 플러그인으로 서버에서 관리자 패널로 데이터 전달하는 방법

<NotV5 />

Strapi는 **헤드리스** <HeadlessCms />입니다. 관리자 패널은 서버와 완전히 분리되어 있습니다.

[Strapi 플러그인을 개발](/cms/plugins-development/developing-plugins)할 때 `/server` 폴더에서 `/admin` 폴더로 데이터를 전달하고 싶을 수 있습니다. `/server` 폴더 내에서는 Strapi 객체에 접근하여 데이터베이스 쿼리를 할 수 있지만 `/admin` 폴더에서는 할 수 없습니다.

`/server` 폴더에서 `/admin` 폴더로 데이터를 전달하는 것은 관리자 패널의 Axios 인스턴스를 사용하여 수행할 수 있습니다:

<MermaidWithFallback
    chartFile="/diagrams/pass-data.mmd"
    fallbackImage="/img/assets/diagrams/pass-data.png"
    fallbackImageDark="/img/assets/diagrams/pass-data_DARK.png"
    alt="서버에서 관리자로 데이터를 전달하는 방법을 보여주는 다이어그램"
/>

`/server` 폴더에서 `/admin` 폴더로 데이터를 전달하려면 먼저 [커스텀 관리자 라우트를 생성](#create-a-custom-admin-route)한 다음 [관리자 패널에서 반환된 데이터를 가져와야](#get-the-data-in-the-admin-panel) 합니다.

## 커스텀 관리자 라우트 생성

관리자 라우트는 어떤 컨트롤러에든 가질 수 있는 라우트와 같지만, `type: 'admin'` 선언이 일반 API 라우터에서 숨기고 관리자 패널에서 접근할 수 있게 해줍니다.

다음 코드는 `my-plugin` 플러그인용 커스텀 관리자 라우트를 선언합니다:

```js title="/my-plugin/server/routes/index.js"
module.exports = {
  'pass-data': {
    type: 'admin',
    routes: [
      {
        method: 'GET',
        path: '/pass-data',
        handler: 'myPluginContentType.index',
        config: {
          policies: [],
          auth: false,
        },
      },
    ]
  }
  // ...
};
```

이 라우트는 `/my-plugin/pass-data` URL 엔드포인트에 GET 요청을 보낼 때 `myPluginContentType` 컨트롤러의 `index` 메소드를 호출합니다.

간단한 텍스트를 반환하는 기본 커스텀 컨트롤러를 만들어 보겠습니다:

```js title="/my-plugin/server/controllers/my-plugin-content-type.js"
'use strict';

module.exports = {
  async index(ctx) {
    ctx.body = 'You are in the my-plugin-content-type controller!';
  }
}
```

이는 `/my-plugin/pass-data` URL 엔드포인트에 GET 요청을 보낼 때 응답과 함께 `You are in the my-plugin-content-type controller!` 텍스트가 반환되어야 함을 의미합니다.

## 관리자 패널에서 데이터 가져오기

관리자 패널 컴포넌트에서 커스텀 라우트 `/my-plugin/pass-data`를 정의한 엔드포인트로 보낸 모든 요청은 이제 커스텀 컨트롤러에서 반환한 텍스트 메시지를 반환해야 합니다.

예를 들어, `/admin/src/api/foobar.js` 파일을 생성하고 다음 코드 예시를 복사하여 붙여넣으면:

```js title="/my-plugin/admin/src/api/foobar.js"
import axios from 'axios';

const foobarRequests = {
  getFoobar: async () => {
    const data = await axios.get(`/my-plugin/pass-data`);
    return data;
  },
};
export default foobarRequests;
```

관리자 패널 컴포넌트의 코드에서 `foobarRequests.getFoobar()`를 사용할 수 있고 데이터와 함께 `You are in the my-plugin-content-type controller!` 텍스트를 반환하게 됩니다.

예를 들어, React 컴포넌트 내에서 `useEffect`를 사용하여 컴포넌트가 초기화된 후 데이터를 가져올 수 있습니다:

```js title="/my-plugin/admin/src/components/MyComponent/index.js"
import foobarRequests from "../../api/foobar";
const [foobar, setFoobar] = useState([]);

// …
useEffect(() => {
  foobarRequests.getFoobar().then(res => {
    setSchemas(res.data);
  });
}, [setFoobar]);
// …
```

이렇게 하면 컴포넌트 상태의 `foobar` 데이터에 `You are in the my-plugin-content-type controller!` 텍스트가 설정됩니다.
