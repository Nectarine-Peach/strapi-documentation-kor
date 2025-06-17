---
title: 관리자 패널 API
pagination_prev: cms/plugins-development/plugin-structure
pagination_next: cms/plugins-development/content-manager-apis
toc_max_heading_level: 4
tags:
- 관리자 패널
- 플러그인 API
- 비동기 함수
- bootstrap 함수
- 훅 API
- Injection Zones API
- 라이프사이클 함수
- 메뉴
- 설정
- 플러그인
- 플러그인 개발
- register 함수
- reducers API
- redux
---

# 플러그인용 관리자 패널 API

Strapi 플러그인은 Strapi 애플리케이션의 [백엔드](/cms/plugins-development/server-api)와 프론트엔드 모두와 상호작용할 수 있습니다. 관리자 패널 API는 프론트엔드 부분에 관한 것으로, 플러그인이 Strapi의 [관리자 패널](/cms/intro)을 커스터마이징할 수 있게 해줍니다.

관리자 패널은 다른 React 애플리케이션들을 포함할 수 있는 <ExternalLink to="https://reactjs.org/" text="React"/> 애플리케이션입니다. 이러한 다른 React 애플리케이션들은 각 Strapi 플러그인의 관리자 부분입니다.

:::prerequisites
[Strapi 플러그인을 생성](/cms/plugins-development/create-a-plugin)했어야 합니다.
:::

관리자 패널 API는 다음을 포함합니다:

- 필수 인터페이스를 내보내는 [진입 파일](#entry-file)
- [라이프사이클 함수](#lifecycle-functions)와 `registerTrad()` [비동기 함수](#async-function)
- 플러그인이 관리자 패널과 상호작용하기 위한 여러 [특수 API](#available-actions)

:::note
플러그인의 관리자 패널 부분에 대한 모든 코드는 `/strapi-admin.js|ts` 또는 `/admin/src/index.js|ts` 파일에 있을 수 있습니다. 하지만 `strapi generate plugin` CLI 생성기 명령어로 만든 [구조](/cms/plugins-development/plugin-structure)처럼 코드를 다른 폴더로 분할하는 것이 권장됩니다.
:::

## 진입 파일

관리자 패널 API의 진입 파일은 `[plugin-name]/admin/src/index.js`입니다. 이 파일은 필수 인터페이스를 내보내며, 다음 함수들을 사용할 수 있습니다:

| 함수 타입 | 사용 가능한 함수 |
| ------------------- | ------------------------------------------------------------------------ |
| 라이프사이클 함수 | <ul><li> [register](#register)</li><li>[bootstrap](#bootstrap)</li></ul> |
| 비동기 함수 | [registerTrads](#registertrads) |

## 라이프사이클 함수

<br/>

### register()

**타입:** `Function`

이 함수는 앱이 실제로 [부트스트랩](#bootstrap)되기 전에도 플러그인을 로드하기 위해 호출됩니다. 실행 중인 Strapi 애플리케이션을 인수(`app`)로 받습니다.

register 함수 내에서 플러그인은 다음을 할 수 있습니다:

* 관리자 패널에서 사용할 수 있도록 [자신을 등록](#registerplugin)
* 메인 네비게이션에 새 링크 추가 ([Menu API](#menu-api) 참고)
* [새 설정 섹션 생성](#createsettingsection)
* [injection zone](#injection-zones-api) 정의
* [reducer 추가](#reducers-api)

#### registerPlugin()

**타입:** `Function`

플러그인을 등록하여 관리자 패널에서 사용할 수 있게 만듭니다.

이 함수는 다음 매개변수를 가진 객체를 반환합니다:

| 매개변수 | 타입 | 설명 |
| ---------------- | ------------------------ | -------------------------------------------------------------------------------------------------- |
| `id` | String | 플러그인 id |
| `name` | String | 플러그인 이름 |
| `injectionZones` | Object | 사용 가능한 [injection zone](#injection-zones-api)의 선언 |

:::note
일부 매개변수는 `package.json` 파일에서 가져올 수 있습니다.
:::

**예시:**

```js title="my-plugin/admin/src/index.js"

// 자동 생성된 컴포넌트
import PluginIcon from './components/PluginIcon';
import pluginId from './pluginId'

export default {
  register(app) {
    app.addMenuLink({
      to: `/plugins/${pluginId}`,
      icon: PluginIcon,
      intlLabel: {
        id: `${pluginId}.plugin.name`,
        defaultMessage: 'My plugin',
      },
      Component: async () => {
        const component = await import(/* webpackChunkName: "my-plugin" */ './pages/App');

        return component;
      },
      permissions: [], // 권한 배열(객체), 사용자의 권한에 따라 플러그인 접근을 허용
    });
    app.registerPlugin({
      id: pluginId,
      name,
    });
  },
};
```

### bootstrap()

**타입**: `Function`

모든 플러그인이 [등록](#register)된 후 실행되는 bootstrap 함수를 노출합니다.

bootstrap 함수 내에서 플러그인은 예를 들어 다음을 할 수 있습니다:

* `getPlugin('plugin-name')`을 사용하여 다른 플러그인 확장
* 훅 등록 ([Hooks API](#hooks-api) 참고)
* [설정 섹션에 링크 추가](#settings-api)
* Content Manager의 List view와 Edit view에 액션과 옵션 추가 ([Content Manager APIs 페이지](/cms/plugins-development/content-manager-apis)에서 자세한 내용 확인)

**예시:**

```js
module.exports = () => {
  return {
    // ...
    bootstrap(app) {
      // 일부 bootstrap 코드 실행
      app.getPlugin('content-manager').injectComponent('editView', 'right-links', { name: 'my-compo', Component: () => 'my-compo' })
    },
  };
};
```

## 비동기 함수

[`register()`](#register)와 [`bootstrap()`](#bootstrap)은 라이프사이클 함수인 반면, `registerTrads()`는 비동기 함수입니다.

### registerTrads()

**타입**: `Function`

빌드 크기를 줄이기 위해, 관리자 패널은 기본적으로 2개의 로케일(`en`과 `fr`)로만 제공됩니다. `registerTrads()` 함수는 플러그인의 번역 파일을 등록하고 애플리케이션 번역을 위한 별도 청크를 생성하는 데 사용됩니다. 수정할 필요가 없습니다.

<details>
<summary>예시: 플러그인의 번역 파일 등록</summary>

```jsx
export default {
  async registerTrads({ locales }) {
    const importedTrads = await Promise.all(
      locales.map(locale => {
        return import(
          /* webpackChunkName: "[pluginId]-[request]" */ `./translations/${locale}.json`
        )
          .then(({ default: data }) => {
            return {
              data: prefixPluginTranslations(data, pluginId),
              locale,
            };
          })
          .catch(() => {
            return {
              data: {},
              locale,
            };
          });
      })
    );

    return Promise.resolve(importedTrads);
  },
};
```

</details>

## 사용 가능한 액션

관리자 패널 API는 플러그인이 여러 소규모 API를 활용하여 액션을 수행할 수 있게 해줍니다. 다음 표를 참고로 사용하세요:

| 액션 | 사용할 API | 사용할 함수 | 관련 라이프사이클 함수 |
| ---------------------------------------- | --------------------------------------- | ------------------------------------------------- | --------------------------- |
| 메인 네비게이션에 새 링크 추가 | [Menu API](#menu-api) | [`addMenuLink()`](#menu-api) | [`register()`](#register) |
| 새 설정 섹션 생성 | [Settings API](#settings-api) | [`createSettingSection()`](#createsettingsection) | [`register()`](#register) |
| injection zone 선언 | [Injection Zones API](#injection-zones-api) | [`registerPlugin()`](#registerplugin) | [`register()`](#register) |
| reducer 추가 | [Reducers API](#reducers-api) | [`addReducers()`](#reducers-api) | [`register()`](#register) |
| 훅 생성 | [Hooks API](#hooks-api) | [`createHook()`](#hooks-api) | [`register()`](#register) |
| [설정 섹션에 링크 추가](#settings-api) | [Settings API](#settings-api) | [`addSettingsLink()`](#settings-api) | [`bootstrap()`](#bootstrap) |
| Injection zone에 컴포넌트 주입 | [Injection Zones API](#injection-zones-api) | [`injectComponent()`](#injection-zones-api) | [`bootstrap()`](#bootstrap) |
| 훅 등록 | [Hooks API](#hooks-api) | [`registerHook()`](#hooks-api) | [`bootstrap()`](#bootstrap) |

### Menu API

메뉴 API는 플러그인이 관리자 패널의 메인 네비게이션에 링크를 추가할 수 있게 해줍니다.

**함수:** `app.addMenuLink(link)`  
**타입:** `Function`  
**매개변수:** 링크 객체에는 다음 매개변수가 있습니다:

| 매개변수 | 타입 | 설명 |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| `to` | String | 링크가 가리킬 경로 |
| `icon` | React 컴포넌트 | 링크를 나타내는 아이콘 |
| `intlLabel` | Object | 링크의 라벨을 국제화하는 데 사용되는 객체 |
| `Component` | React 컴포넌트 | 플러그인 인터페이스 |
| `permissions` | Array | 사용자의 권한에 따라 플러그인 접근을 허용하는 권한 배열 |

**예시:**

```js
export default {
  register(app) {
    app.addMenuLink({
      to: '/plugins/my-plugin',
      icon: PluginIcon,
      intlLabel: {
        id: 'my-plugin.plugin.name',
        defaultMessage: 'My plugin',
      },
      Component: () => 'Hello World!',
      permissions: [
        // 설정된 권한 배열
      ],
    });
  },
};
```

### Settings API

Settings API는 플러그인이 설정 페이지와 상호작용할 수 있게 해줍니다.

**관련 함수:** `app.createSettingSection()` 및 `app.addSettingsLink()`

#### createSettingSection()

새 설정 섹션을 생성합니다.

**함수:** `app.createSettingSection(section, links)`  
**타입:** `Function`

첫 번째 인수인 `section`은 다음 매개변수를 가진 객체입니다:

| 매개변수 | 타입 | 설명 |
| --------------- | -------- | ---------------------------------------------------------------------------- |
| `id` | String | 섹션 식별자 |
| `intlLabel` | Object | 섹션의 라벨을 국제화하는 데 사용되는 객체 |

두 번째 인수인 `links`는 [링크 객체](#settings-api)의 배열입니다.

**예시:**

```js
export default {
  register(app) {
    app.createSettingSection(
      {
        id: 'my-plugin',
        intlLabel: { id: 'my-plugin.plugin.name', defaultMessage: 'My plugin name' },
      },
      [
        // 링크들
      ]
    );
  },
};
```

#### addSettingsLink()

설정 섹션에 링크를 추가합니다.

**함수:** `app.addSettingsLink(sectionId, link)`  
**타입:** `Function`

첫 번째 인수인 `sectionId`는 섹션 식별자입니다.

두 번째 인수인 `link`는 다음 매개변수를 가진 객체입니다:

| 매개변수 | 타입 | 설명 |
| --------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| `id` | String | React 컴포넌트 식별자 |
| `to` | String | 링크가 가리킬 경로 |
| `intlLabel` | Object | 링크의 라벨을 국제화하는 데 사용되는 객체 |
| `Component` | React 컴포넌트 | 설정 인터페이스 |
| `permissions` | Array | 사용자의 권한에 따라 플러그인 접근을 허용하는 권한 배열 |

**예시:**

```js
export default {
  bootstrap(app) {
    app.addSettingsLink('global', {
      intlLabel: { id: 'my-plugin.plugin.name', defaultMessage: 'My plugin name' },
      id: 'my-plugin',
      to: '/settings/my-plugin',
      Component: () => 'Hello World!',
      permissions: [],
    });
  },
};
```

### Reducers API

Reducers API는 플러그인이 관리자 패널의 Redux store에 reducer를 추가할 수 있게 해줍니다.

**함수:** `app.addReducers(reducers)`  
**타입:** `Function`  
**매개변수:** reducer들의 객체

**예시:**

```js
import { exampleReducer } from './reducers'

export default {
  register(app) {
    app.addReducers({
      // 키는 store에서 접근할 reducer 이름입니다
      'my-plugin_exampleReducer': exampleReducer,
    })
  },
}
```

### Injection Zones API

Injection Zones API는 플러그인이 Strapi의 관리자 패널에서 컴포넌트를 렌더링할 수 있는 영역을 선언하고 사용할 수 있게 해줍니다.

#### injection zone 선언

injection zone을 선언하려면, [`registerPlugin()`](#registerplugin) 함수의 `injectionZones` 매개변수를 사용합니다.

`injectionZones`는 다음 형식을 따르는 객체입니다:

```js
// 객체 구조:
injectionZones: {
  [areaName]: [
    {
      name: 'injection-zone-name',
      Component: () => 'This will be rendered',
    },
  ],
},
```

여기서:

- `[areaName]`은 영역의 이름입니다
- 각 영역은 injection zone들의 배열을 포함합니다
- 각 injection zone은 고유한 `name`과 렌더링할 `Component`를 가져야 합니다

**예시:**

```js
import PluginIcon from './components/PluginIcon';

export default {
  register(app) {
    app.registerPlugin({
      name: 'my-plugin',
      id: 'my-plugin',
      injectionZones: {
        contentManager: [
          {
            name: 'my-compo',
            Component: () => ({
              name: 'my-compo',
              Component: () => 'This component will be injected',
            }),
          },
        ],
      },
    });
  },
};
```

#### injection zone에 컴포넌트 주입

컴포넌트를 injection zone에 주입하려면, `app.getPlugin().injectComponent()` 함수를 사용합니다.

**함수:** `app.getPlugin().injectComponent(areaName, injectionZoneName, component)`  
**타입:** `Function`

| 매개변수 | 타입 | 설명 |
| --------------- | -------- | -------------------------------------------------------------------------------------- |
| `areaName` | String | 영역의 이름 |
| `injectionZoneName` | String | injection zone의 이름 |
| `component` | Object | 주입할 컴포넌트와 해당 이름을 포함하는 객체 |

**예시:**

```js
export default {
  bootstrap(app) {
    app.getPlugin('content-manager').injectComponent('editView', 'right-links', {
      name: 'my-compo',
      Component: () => 'my-compo',
    });
  },
};
```

### Hooks API

Hooks API는 플러그인이 관리자 패널에서 훅을 생성하고 등록할 수 있게 해줍니다.

#### 훅 생성

**함수:** `app.createHook(name)`  
**타입:** `Function`  
**매개변수:** 훅의 이름

**예시:**

```js
export default {
  register(app) {
    app.createHook('My_PLUGIN/EXAMPLE_HOOK');
  },
};
```

#### 훅 등록

**함수:** `app.registerHook(name, fn)`  
**타입:** `Function`  
**매개변수:**

| 매개변수 | 타입 | 설명 |
| --------- | -------- | ---------------------------- |
| `name` | String | 훅의 이름 |
| `fn` | Function | 실행할 함수 |

**예시:**

```js
export default {
  bootstrap(app) {
    app.registerHook('My_PLUGIN/EXAMPLE_HOOK', (...args) => {
      // 뭔가 작업 수행
    });
  },
};
```

#### 훅 실행

훅을 실행하려면, `app.runHook()`을 사용합니다.

**함수:** `runHook(name, ...args)`  
**타입:** `Function`  
**매개변수:**

| 매개변수 | 타입 | 설명 |
| --------- | -------- | --------------------------------------------- |
| `name` | String | 훅의 이름 |
| `...args` | Any | 훅 함수로 전달할 인수들 |

**예시:**

```js
// 훅 실행
app.runHook('My_PLUGIN/EXAMPLE_HOOK', 'someArgument', 'anotherArgument');
```