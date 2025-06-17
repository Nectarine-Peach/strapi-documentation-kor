---
title: 홈페이지 커스터마이징
description: Strapi 관리자 패널의 홈페이지와 위젯 커스터마이징 방법을 알아보세요.
toc_max_heading_level: 6
tags:
- 관리자 패널
- 홈페이지
- 위젯
- 기능
---

# 홈페이지 커스터마이징
<VersionBadge version="5.13.0"/>

<Icon name="house" /> 홈페이지는 Strapi 관리자 패널의 랜딩 페이지입니다. 기본적으로 2개의 위젯이 제공되어 콘텐츠 개요를 보여줍니다:

- _최근 수정된 항목_: 최근에 수정된 콘텐츠 항목을 표시하며, 콘텐츠 타입, 상태, 수정 시각을 보여줍니다.
- _최근 발행된 항목_: 최근에 발행된 콘텐츠 항목을 표시하여, 빠르게 접근하고 관리할 수 있습니다.

<ThemedImage
  alt="기본 위젯이 포함된 홈페이지"
  sources={{
    light: '/img/assets/admin-homepage/admin-panel-homepage.png',
    dark: '/img/assets/admin-homepage/admin-panel-homepage_DARK.png',
  }}
/>

이 기본 위젯들은 현재 제거할 수 없지만, 직접 위젯을 만들어 홈페이지를 커스터마이징할 수 있습니다.

:::note
Strapi 프로젝트를 최근에 생성했다면, 위젯 위에 빠른 투어(Quick tour)가 표시될 수 있습니다(아직 건너뛰지 않았다면).
:::

## 커스텀 위젯 추가하기

커스텀 위젯을 추가하는 방법은 다음과 같습니다:

- [마켓플레이스](/cms/plugins/installing-plugins-via-marketplace)에서 플러그인을 설치하거나
- 직접 위젯을 만들어 등록할 수 있습니다.

이 문서에서는 직접 위젯을 만들고 등록하는 방법을 안내합니다.

### 커스텀 위젯 등록하기

위젯을 등록하려면 `app.widgets.register()`를 사용합니다:

- 플러그인을 개발하는 경우, 플러그인의 [`register` 라이프사이클 메서드](/cms/plugins-development/server-api#register)에서(권장)
- 플러그인을 사용하지 않고 단일 Strapi 애플리케이션에만 추가하는 경우, [애플리케이션의 글로벌 `register()` 라이프사이클 메서드](/cms/configurations/functions#register)에서

:::info
이 문서의 예시는 플러그인을 통한 위젯 등록을 다룹니다. 애플리케이션의 글로벌 `register()`에서 등록할 경우, `pluginId` 속성은 전달하지 않아야 합니다.
:::

<Tabs groupId="js-ts">
<TabItem value="javascript" label="JavaScript">

```jsx title="src/plugins/my-plugin/admin/src/index.js"
import pluginId from './pluginId';
import MyWidgetIcon from './components/MyWidgetIcon';

export default {
  register(app) {
    // 플러그인 자체 등록
    app.registerPlugin({
      id: pluginId,
      name: 'My Plugin',
    });
    
    // 홈페이지용 위젯 등록
    app.widgets.register({
      icon: MyWidgetIcon,
      title: {
        id: `${pluginId}.widget.title`,
        defaultMessage: 'My Widget',
      },
      component: async () => {
        const component = await import('./components/MyWidget');
        return component.default;
      },
      /**
       * named export를 사용했다면 아래처럼 사용
       */
      // component: async () => {
      //   const { Component } = await import('./components/MyWidget');
      //   return Component;
      // },
      id: 'my-custom-widget',
      pluginId: pluginId,
    });
  },
  
  bootstrap() {},
  // ...
};
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```tsx title="src/plugins/my-plugin/admin/src/index.ts"
import pluginId from './pluginId';
import MyWidgetIcon from './components/MyWidgetIcon';
import type { StrapiApp } from '@strapi/admin/strapi-admin';

export default {
  register(app: StrapiApp) {
    // 플러그인 자체 등록
    app.registerPlugin({
      id: pluginId,
      name: 'My Plugin',
    });
    
    // 홈페이지용 위젯 등록
    app.widgets.register({
      icon: MyWidgetIcon,
      title: {
        id: `${pluginId}.widget.title`,
        defaultMessage: 'My Widget',
      },
      component: async () => {
        const component = await import('./components/MyWidget');
        return component.default;
      },
      /**
       * named export를 사용했다면 아래처럼 사용
       */
      // component: async () => {
      //   const { Component } = await import('./components/MyWidget');
      //   return Component;
      // },
      id: 'my-custom-widget',
      pluginId: pluginId,
    });
  },
  
  bootstrap() {},
  // ...
};
```

</TabItem>
</Tabs>

:::note API는 Strapi 5.13+ 필요
`app.widgets.register` API는 Strapi 5.13 이상에서만 동작합니다. 이전 버전에서 호출하면 관리자 패널이 오류로 중단됩니다.
플러그인 개발자는 다음 중 하나를 따라야 합니다:

- 플러그인의 `package.json`에 `@strapi/strapi` peerDependency를 `^5.13.0`으로 지정(마켓플레이스 호환성 체크에 사용)
- 또는 API 존재 여부를 체크 후 호출:

  ```js
  if ('widgets' in app) {
    // 등록 진행
  }
  ```

플러그인 전체 목적이 위젯 등록이라면 peerDependency 방식이 권장됩니다. 위젯 추가가 부가 기능이라면 두 번째 방식이 더 적합합니다.
:::

#### 위젯 API 레퍼런스

`app.widgets.register()`는 단일 위젯 설정 객체 또는 객체 배열을 받을 수 있습니다. 각 위젯 설정 객체는 다음 속성을 가질 수 있습니다:

| 속성         | 타입                                 | 설명                                              | 필수 여부 |
|--------------|--------------------------------------|---------------------------------------------------|----------|
| `icon`       | `React.ComponentType`                | 위젯 제목 옆에 표시할 아이콘 컴포넌트              | 예       |
| `title`      | `MessageDescriptor`                  | 번역 지원되는 위젯 제목                           | 예       |
| `component`  | `() => Promise<React.ComponentType>` | 비동기로 위젯 컴포넌트를 반환하는 함수             | 예       |
| `id`         | `string`                             | 위젯의 고유 식별자                                | 예       |
| `link`       | `Object`                             | 위젯에 추가할 수 있는 링크(아래 표 참고)           | 아니오    |
| `pluginId`   | `string`                             | 위젯을 등록하는 플러그인 ID                        | 아니오    |
| `permissions`| `Permission[]`                       | 위젯을 볼 수 있는 권한                             | 아니오    |

**링크 객체 속성:**

위젯에 링크를 추가하려면(예: 상세 페이지로 이동) 아래 속성의 객체를 `link`로 전달할 수 있습니다:

| 속성    | 타입                | 설명                        | 필수 여부 |
|---------|---------------------|-----------------------------|----------|
| `label` | `MessageDescriptor` | 링크에 표시할 텍스트         | 예       |
| `href`  | `string`            | 이동할 URL                  | 예       |

### 위젯 컴포넌트 만들기

위젯 컴포넌트는 정보를 간결하게 보여주는 방식으로 설계해야 합니다.

아래는 기본 위젯 컴포넌트 구현 예시입니다:

<Tabs groupId="js-ts">
<TabItem value="javascript" label="JavaScript">

```jsx title="src/plugins/my-plugin/admin/src/components/MyWidget/index.js"
import React, { useState, useEffect } from 'react';
import { Widget } from '@strapi/admin/strapi-admin';

const MyWidget = () => {
  const [loading, setLoading] = useState(true);
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    // 데이터 패칭
    const fetchData = async () => {
      try {
        // 실제 API 호출로 교체
        const response = await fetch('/my-plugin/data');
        const result = await response.json();
        
        setData(result);
        setLoading(false);
      } catch (err) {
        setError(err);
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) {
    return <Widget.Loading />;
  }

  if (error) {
    return <Widget.Error />;
  }

  if (!data || data.length === 0) {
    return <Widget.NoData />;
  }

  return (
    <div>
      {/* 위젯 내용 */}
      <ul>
        {data.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default MyWidget;
```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```tsx title="src/plugins/my-plugin/admin/src/components/MyWidget/index.tsx"
import React, { useState, useEffect } from 'react';
import { Widget } from '@strapi/admin/strapi-admin';

interface DataItem {
  id: number;
  name: string;
}

const MyWidget: React.FC = () => {
  const [loading, setLoading] = useState<boolean>(true);
  const [data, setData] = useState<DataItem[] | null>(null);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    // 데이터 패칭
    const fetchData = async () => {
      try {
        // 실제 API 호출로 교체
        const response = await fetch('/my-plugin/data');
        const result = await response.json();
        
        setData(result);
        setLoading(false);
      } catch (err) {
        setError(err instanceof Error ? err : new Error(String(err)));
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) {
    return <Widget.Loading />;
  }

  if (error) {
    return <Widget.Error />;
  }

  if (!data || data.length === 0) {
    return <Widget.NoData />;
  }

  return (
    <div>
      {/* 위젯 내용 */}
      <ul>
        {data.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default MyWidget;
```

</TabItem>
</Tabs>

:::tip
아래 예시는 useEffect에서 직접 데이터 패칭을 하지만, 실제 서비스에서는 [React 공식문서](https://react.dev/learn/build-a-react-app-from-scratch#data-fetching)에서 권장하는 방식이나 [TanStackQuery](https://tanstack.com/query/v3/)와 같은 라이브러리 사용을 추천합니다.
:::

**데이터 관리**:

![렌더링 및 데이터 관리](/img/assets/homepage-customization/rendering-data-management.png)

위 그림의 초록색 박스는 [API](#widget-api-reference)의 `widget.component`로 전달된 사용자의 React 컴포넌트가 렌더링되는 영역입니다. 이 영역 안에서는 원하는 내용을 자유롭게 렌더링할 수 있습니다. 그 외의 영역은 Strapi가 렌더링하여 전체 디자인 일관성을 보장합니다. API에서 전달한 `icon`, `title`, `link`(선택) 속성은 위젯에 표시됩니다.

#### 위젯 헬퍼 컴포넌트 레퍼런스

Strapi는 위젯 간 일관된 UX를 위해 여러 헬퍼 컴포넌트를 제공합니다:

| 컴포넌트              | 설명                                   | 사용 시점                  |
|----------------------|----------------------------------------|----------------------------|
| `Widget.Loading`     | 로딩 스피너와 메시지 표시              | 데이터 패칭 중             |
| `Widget.Error`       | 에러 상태 표시                         | 에러 발생 시               |
| `Widget.NoData`      | 데이터 없음 표시                       | 표시할 데이터가 없을 때     |
| `Widget.NoPermissions` | 권한 부족 안내 표시                  | 위젯 접근 권한 없을 때      |

이 컴포넌트들은 위젯 간 일관된 UI/UX를 제공합니다. 자식 없이 `<Widget.Error />`로 기본 메시지를, 자식 전달 시 `<Widget.Error>커스텀 메시지</Widget.Error>`로 원하는 메시지를 표시할 수 있습니다.

## 예시: 콘텐츠 메트릭 위젯 추가하기

아래는 각 콘텐츠 타입별 엔트리 개수를 보여주는 콘텐츠 메트릭 위젯을 만드는 전체 예시입니다.

최종 결과는 관리자 패널 <Icon name="house" /> 홈페이지에 아래와 같이 표시됩니다:

<ThemedImage
  alt="프로필 페이지의 Billing 탭"
  sources={{
      light: '/img/assets/homepage-customization/content-metrics-widget.png',
      dark: '/img/assets/homepage-customization/content-metrics-widget_DARK.png',
    }}
/>

이 위젯은 Strapi 설치 시 `--example` 플래그를 사용해 자동 생성된 예시 콘텐츠 타입의 개수를 보여줍니다([CLI 설치 옵션](/cms/installation/cli#cli-installation-options) 참고).

이 위젯을 추가하려면:

1. "content-metrics" 플러그인을 생성([플러그인 생성 문서](/cms/plugins-development/create-a-plugin) 참고)
2. 아래 코드 예시를 활용

:::tip
직접 실습해보고 싶다면 <ExternalLink to="https://codesandbox.io/p/sandbox/github/pwizla/strapi-custom-widget-content-metrics" text="CodeSandbox 링크" />를 참고하세요.
:::

<Tabs groupId="js-ts">
<TabItem value="javascript" label="JavaScript">

아래 파일은 플러그인과 위젯을 등록합니다:

```jsx title="src/plugins/content-metrics/admin/src/index.js" {28-42}
import { PLUGIN_ID } from './pluginId';
import { Initializer } from './components/Initializer';
import { PluginIcon } from './components/PluginIcon';
import { Stethoscope } from '@strapi/icons'

export default {
  register(app) {
    app.addMenuLink({
      to: `plugins/${PLUGIN_ID}`,
      icon: PluginIcon,
      intlLabel: {
        id: `${PLUGIN_ID}.plugin.name`,
        defaultMessage: PLUGIN_ID,
      },
      Component: async () => {
        const { App } = await import('./pages/App');
        return App;
      },
    });

    app.registerPlugin({
      id: PLUGIN_ID,
      initializer: Initializer,
      isReady: false,
      name: PLUGIN_ID,
    });

    // 위젯 등록
    app.widgets.register({
      icon: Stethoscope,
      title: {
        id: `${PLUGIN_ID}.widget.metrics.title`, 
        defaultMessage: 'Content Metrics',
      },
      component: async () => {
        const component = await import('./components/MetricsWidget');
        return component.default;
      },
      id: 'content-metrics',
      pluginId: PLUGIN_ID, 
    });
  },

  async registerTrads({ locales }) {
    return Promise.all(
      locales.map(async (locale) => {
        try {
          const { default: data } = await import(`./translations/${locale}.json`);
          return { data, locale };
        } catch {
          return { data: {}, locale };
        }
      })
    );
  },

  bootstrap() {},
};
```

아래 파일은 위젯 컴포넌트와 로직을 정의합니다. 플러그인에서 생성할 컨트롤러와 라우트를 활용합니다:

```jsx title="src/plugins/content-metrics/admin/src/components/MetricsWidget/index.js"
import React, { useState, useEffect } from 'react';
import { Table, Tbody, Tr, Td, Typography, Box } from '@strapi/design-system';
import { Widget } from '@strapi/admin/strapi-admin'

const MetricsWidget = () => {
  const [loading, setLoading] = useState(true);
  const [metrics, setMetrics] = useState(null);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchMetrics = async () => {
      try {
        const response = await fetch('/api/content-metrics/count');
        const data = await response.json();

        console.log("data:", data);
        
        const formattedData = {};
        
        if (data && typeof data === 'object') {
          Object.keys(data).forEach(key => {
            const value = data[key];
            formattedData[key] = typeof value === 'number' ? value : String(value);
          });
        }
        
        setMetrics(formattedData);
        setLoading(false);
      } catch (err) {
        console.error(err);
        setError(err.message || '에러가 발생했습니다');
        setLoading(false);
      }
    };
    
    fetchMetrics();
  }, []);
  
  if (loading) {
    return (
      <Widget.Loading />
    );
  }
  
  if (error) {
    return (
      <Widget.Error />
    );
  }
  
  if (!metrics || Object.keys(metrics).length === 0) {
    return <Widget.NoData>콘텐츠 타입이 없습니다</Widget.NoData>;
  }
  
  return (
    <Table>
      <Tbody>
        {Object.entries(metrics).map(([contentType, count], index) => (
          <Tr key={index}>
            <Td>
              <Typography variant="omega">{String(contentType)}</Typography>
            </Td>
            <Td>
              <Typography variant="omega" fontWeight="bold">{String(count)}</Typography>
            </Td>
          </Tr>
        ))}
      </Tbody>
    </Table>
  );
};

export default MetricsWidget;
```

아래는 모든 콘텐츠 타입의 개수를 세는 커스텀 컨트롤러 예시입니다:

```js title="src/plugins/content-metrics/server/src/controllers/metrics.js"
'use strict';
module.exports = ({ strapi }) => ({
  async getContentCounts(ctx) {
    try {
      // 모든 콘텐츠 타입 가져오기
      const contentTypes = Object.keys(strapi.contentTypes)
        .filter(uid => uid.startsWith('api::'))
        .reduce((acc, uid) => {
          const contentType = strapi.contentTypes[uid];
          acc[contentType.info.displayName || uid] = 0;
          return acc;
        }, {});
      
      // 각 콘텐츠 타입별 엔트리 개수 세기
      for (const [name, _] of Object.entries(contentTypes)) {
        const uid = Object.keys(strapi.contentTypes)
          .find(key => 
            strapi.contentTypes[key].info.displayName === name || key === name
          );
          
        if (uid) {
          // Document Service API의 count() 사용
          const count = await strapi.documents(uid).count();
          contentTypes[name] = count;
        }
      }
      
      ctx.body = contentTypes;
    } catch (err) {
      ctx.throw(500, err);
    }
  }
});
```

아래는 metrics 컨트롤러를 `/count` 경로로 연결하는 라우트 예시입니다:

```js title="src/plugins/content-metrics/server/src/routes/index.js"
export default {
  'content-api': {
    type: 'content-api',
    routes: [
      {
        method: 'GET',
        path: '/count',
        handler: 'metrics.getContentCounts',
        config: {
          policies: [],
        },
      },
    ],
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

(동일한 예시를 TypeScript로 변환하여 제공합니다)

</TabItem>
</Tabs>
