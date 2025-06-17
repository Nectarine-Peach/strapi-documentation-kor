---
title: 커스텀 필드
description: 커스텀 필드를 사용하여 Strapi의 콘텐츠 타입 기능을 확장하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
toc_max_heading_level: 5
canonicalUrl: https://docs.strapi.io/cms/development/custom-fields.html
tags:
- 관리자 패널
- 컴포넌트
- 콘텐츠 타입 빌더
- 콘텐츠 매니저
- 커스텀 필드
- 등록 함수
---

import CustomFieldRequiresPlugin from '/docs/snippets/custom-field-requires-plugin.md'

# 커스텀 필드

커스텀 필드는 콘텐츠 타입과 컴포넌트에 새로운 유형의 필드를 추가하여 Strapi의 기능을 확장합니다. 생성되거나 플러그인을 통해 Strapi에 추가되면, 커스텀 필드는 내장 필드와 마찬가지로 콘텐츠 타입 빌더와 콘텐츠 매니저에서 사용할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">없음</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본적으로 사용 가능하며 활성화됨</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

## 구성

기성품 커스텀 필드는 [마켓플레이스](https://market.strapi.io/plugins?categories=Custom+fields)에서 찾을 수 있습니다. 설치 후에는 다른 구성이 필요하지 않으며 바로 사용할 수 있습니다([사용법](#usage) 참고).

직접 커스텀 필드를 개발할 수도 있습니다.

### 자체 커스텀 필드 개발

커스텀 필드를 추가하는 권장 방법은 플러그인을 생성하는 것이지만, 앱별 커스텀 필드는 `src/index` 및 `src/admin/app` 파일에 있는 전역 `register` [함수](/cms/configurations/functions) 내에서도 등록할 수 있습니다.

:::note 현재 제한사항
* 커스텀 필드는 플러그인을 사용해야만 마켓플레이스에서 공유하고 배포할 수 있습니다.
* 커스텀 필드는 Strapi에 새로운 데이터 타입을 추가할 수 없으며, [모델의 속성](/cms/backend-customization/models#model-attributes) 문서에 설명된 기존 내장 Strapi 데이터 타입을 사용해야 합니다.
* 기존 데이터 타입을 수정할 수도 없습니다.
* 관계, 미디어, 컴포넌트, 다이나믹 존과 같은 Strapi 고유의 특수 데이터 타입은 커스텀 필드에서 사용할 수 없습니다.
:::

:::prerequisites
<CustomFieldRequiresPlugin components={props.components} />
:::

커스텀 필드 플러그인은 서버와 관리자 패널 부분을 모두 포함합니다. 커스텀 필드가 Strapi의 관리자 패널에서 사용 가능하려면 두 부분 모두에 등록되어야 합니다.

#### 서버에서 커스텀 필드 등록

Strapi의 서버는 커스텀 필드를 사용하는 속성이 유효한지 확인하기 위해 모든 커스텀 필드를 알고 있어야 합니다.

`strapi.customFields` 객체는 `Strapi` 인스턴스에서 `register()` 메서드를 노출합니다. 이 메서드는 플러그인의 서버 [등록 라이프사이클](/cms/plugins-development/server-api#register) 중에 서버에서 커스텀 필드를 등록하는 데 사용됩니다.

`strapi.customFields.register()`는 일부 매개변수가 있는 객체(또는 객체 배열)를 전달하여 서버에서 하나 또는 여러 커스텀 필드를 등록합니다.

<details>
<summary>서버에서 커스텀 필드를 등록하는 데 사용 가능한 매개변수:</summary>

| 매개변수                         | 설명                                                                                                                                             | 타입     |
| --------------------------------- |---------------------------------------------------------------------------------------------------------------------------------------------------------| -------- |
| `name`                            | 커스텀 필드의 이름                                                                                                                            | `String` |
| `plugin`<br/><br/>(_선택사항_)    | 커스텀 필드를 생성하는 플러그인 이름<br/><br/>❗️ 정의된 경우, 관리자 패널 등록 시 `pluginId` 값이 동일해야 함([관리자 패널에 커스텀 필드 등록](#registering-a-custom-field-in-the-admin-panel) 참고) | `String` |
| `type`                            | 커스텀 필드가 사용할 데이터 타입                                                                                                                 | `String` |
| `inputSize`<br/><br/>(_선택사항_) | 관리자 패널에서 커스텀 필드 입력란의 너비를 정의하는 매개변수                                                                             | `Object` |

`inputSize` 객체를 지정하면 다음 매개변수를 모두 포함해야 합니다:

| 매개변수     | 설명                                                                                                                                               | 타입      |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| `default`     | 입력 필드가 관리자 패널의 12-컬럼 그리드에서 차지할 기본 크기.<br/>값은 `4`, `6`, `8`, `12` 중 하나여야 함. | `Integer` |
| `isResizable` | 입력란의 크기 조절 가능 여부                                                                                                                   | `Boolean` |

</details>

**예시: 서버에서 "color" 커스텀 필드 등록:**

아래 예시는 CLI 생성기를 사용해 `color-picker` 플러그인을 생성한 경우입니다([플러그인 개발](/cms/plugins-development/developing-plugins) 참고):

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/plugins/color-picker/server/register.js"
module.exports = ({ strapi }) => {
  strapi.customFields.register({
    name: "color",
    plugin: "color-picker",
    type: "string",
    inputSize: {
      // optional
      default: 4,
      isResizable: true,
    },
  });
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```ts title="/src/plugins/color-picker/server/register.ts"
export default ({ strapi }: { strapi: any }) => {
  strapi.customFields.register({
    name: "color",
    plugin: "color-picker",
    type: "string",
    inputSize: {
      // optional
      default: 4,
      isResizable: true,
    },
  });
};
```

</TabItem>
</Tabs>

The custom field could also be declared directly within the `strapi-server.js` file if you didn't have the plugin code scaffolded by the CLI generator:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```js title="/src/plugins/color-picker/strapi-server.js"
module.exports = {
  register({ strapi }) {
    strapi.customFields.register({
      name: "color",
      plugin: "color-picker",
      type: "text",
      inputSize: {
        // optional
        default: 4,
        isResizable: true,
      },
    });
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```ts title="/src/plugins/color-picker/strapi-server.ts"
export default {
  register({ strapi }: { strapi: any }) {
    strapi.customFields.register({
      name: "color",
      plugin: "color-picker",
      type: "text",
      inputSize: {
        // optional
        default: 4,
        isResizable: true,
      },
    });
  },
};
```

</TabItem>
</Tabs>

#### Registering a custom field in the admin panel

:::prerequisites
<CustomFieldRequiresPlugin components={props.components} />
:::

Custom fields must be registered in Strapi's admin panel to be available in the Content-type Builder and the Content Manager.

The `app.customFields` object exposes a `register()` method on the `StrapiApp` instance. This method is used to register custom fields in the admin panel during the plugin's admin [register lifecycle](/cms/plugins-development/admin-panel-api#register).

`app.customFields.register()` registers one or several custom field(s) in the admin panel by passing an object (or an array of objects) with some parameters.

<details>
<summary>Parameters available to register the custom field on the server:</summary>

| Parameter                        | Description                                                                                                                                  | Type                                                 |
| -------------------------------- |----------------------------------------------------------------------------------------------------------------------------------------------| ---------------------------------------------------- |
| `name`                           | Name of the custom field                                                                                                                     | `String`                                             |
| `pluginId`<br/><br/>(_optional_) | Name of the plugin creating the custom field<br/><br/>❗️ If defined, the `plugin` value on the server registration must have the same value (see [Registering a custom field on the server](#registering-a-custom-field-on-the-server))  | `String`                                             |
| `type`                           | Existing Strapi data type the custom field will use<br/><br/>❗️ Relations, media, components, or dynamic zones cannot be used.               | `String`                                             |
| `icon`<br/><br/>(_optional_)     | Icon for the custom field                                                                                                                    | `React.ComponentType`                                |
| `intlLabel`                      | Translation for the name                                                                                                                     | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/> |
| `intlDescription`                | Translation for the description                                                                                                              | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/> |
| `components`                     | Components needed to display the custom field in the Content Manager (see [components](#components))                                         |
| `options`<br/><br/>(_optional_)  | Options to be used by the Content-type Builder (see [options](#options))                                                                     | `Object`                                             |

</details>

**예시: 관리자 패널에서 "color" 커스텀 필드 등록:**

다음 예시에서는 CLI 생성기를 사용하여 `color-picker` 플러그인을 생성했습니다([플러그인 개발](/cms/plugins-development/developing-plugins.md) 참고):

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/plugins/color-picker/admin/src/index.js"
import ColorPickerIcon from "./components/ColorPicker/ColorPickerIcon";

export default {
  register(app) {
    // ... app.addMenuLink() goes here
    // ... app.registerPlugin() goes here

    app.customFields.register({
      name: "color",
      pluginId: "color-picker", // 커스텀 필드가 color-picker 플러그인에 의해 생성됨
      type: "string", // 색상은 문자열로 저장됨
      intlLabel: {
        id: "color-picker.color.label",
        defaultMessage: "Color",
      },
      intlDescription: {
        id: "color-picker.color.description",
        defaultMessage: "Select any color",
      },
      icon: ColorPickerIcon, // 아이콘 컴포넌트를 생성/가져오는 것을 잊지 마세요
      components: {
        Input: async () =>
          import('./components/Input').then((module) => ({
            default: module.Input,
          })),
      },
      options: {
        // 여기에 옵션을 선언하세요
      },
    });
  },

  // ... bootstrap() goes here
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```ts title="/src/plugins/color-picker/admin/src/index.ts"
import ColorPickerIcon from "./components/ColorPicker/ColorPickerIcon";

export default {
  register(app) {
    // ... app.addMenuLink() goes here
    // ... app.registerPlugin() goes here

    app.customFields.register({
      name: "color",
      pluginId: "color-picker", // 커스텀 필드가 color-picker 플러그인에 의해 생성됨
      type: "string", // 색상은 문자열로 저장됨
      intlLabel: {
        id: "color-picker.color.label",
        defaultMessage: "Color",
      },
      intlDescription: {
        id: "color-picker.color.description",
        defaultMessage: "Select any color",
      },
      icon: ColorPickerIcon, // 아이콘 컴포넌트를 생성/가져오는 것을 잊지 마세요
      components: {
        Input: async () =>
          import('./components/Input').then((module) => ({
            default: module.Input,
          })),
      },
      options: {
        // 여기에 옵션을 선언하세요
      },
    });
  },

  // ... bootstrap() goes here
};
```

</TabItem>
</Tabs>

##### 컴포넌트

`app.customFields.register()`는 콘텐츠 매니저의 편집 뷰에서 사용할 `Input` React 컴포넌트가 포함된 `components` 객체를 전달해야 합니다.

**예시: Input 컴포넌트 등록:**

다음 예시에서는 CLI 생성기를 사용하여 `color-picker` 플러그인을 생성했습니다([플러그인 개발](/cms/plugins-development/developing-plugins.md) 참고):

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/plugins/color-picker/admin/src/index.js"
export default {
  register(app) {
    app.customFields.register({
      // …
      components: {
        Input: async () =>
          import('./components/Input').then((module) => ({
            default: module.Input,
          })),
      },
      // …
    });
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```jsx title="/src/plugins/color-picker/admin/src/index.js"
export default {
  register(app) {
    app.customFields.register({
      // …
      components: {
        Input: async () =>
          import('./components/Input').then((module) => ({
            default: module.Input,
          })),
      },
      // …
    });
  },
};
```

</TabItem>
</Tabs>

<details>
<summary>커스텀 필드 <code>Input</code> 컴포넌트에 전달되는 Props:</summary>

| Prop             | 설명                                                                                                                                                                                                                               | 타입                                                                 |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `attribute`      | 커스텀 필드의 기본 Strapi 타입과 옵션이 포함된 속성 객체                                                                                                                                                                               | `{ type: String, customField: String }`                              |
| `description`    | [뷰 구성](/cms/features/content-manager#edit-view-settings)에서 설정한 필드 설명                                                                                                  | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/>                 |
| `placeholder`    | [뷰 구성](/cms/features/content-manager#edit-view-settings)에서 설정한 필드 플레이스홀더                                                                                                  | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/>                 |
| `hint`           | [뷰 구성](/cms/features/content-manager#edit-view-settings)에서 설정한 필드 설명과 최소/최대 [유효성 검사 요구사항](/cms/backend-customization/models#validations) | `String`                                                             |
| `name`           | 콘텐츠 타입 빌더에서 설정한 필드 이름                                                                                                                                                                                            | `String`                                                             |
| `intlLabel`      | 콘텐츠 타입 빌더 또는 뷰 구성에서 설정한 필드 이름                                                                                                                                                                      | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/>                 |
| `onChange`       | 입력 변경 이벤트 핸들러. `name` 인수는 필드 이름을, `type` 인수는 기본 Strapi 타입을 참조합니다                                                                                          | `({ target: { name: String value: unknown type: String } }) => void` |
| `contentTypeUID` | 필드가 속한 콘텐츠 타입                                                                                                                                                                                                     | `String`                                                             |
| `type`           | 커스텀 필드 uid, 예: `plugin::color-picker.color`                                                                                                                                                                            | `String`                                                             |
| `value`          | 기본 Strapi 타입이 예상하는 입력 값                                                                                                                                                                                        | `unknown`                                                            |
| `required`       | 필드가 필수인지 여부                                                                                                                                                                                                      | `boolean`                                                            |
| `error`          | 유효성 검사 후 받은 오류                                                                                                                                                                                                           | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/>                 |
| `disabled`       | 입력이 비활성화되었는지 여부                                                                                                                                                                                                      | `boolean`                                                            |

Strapi v4.13.0부터 콘텐츠 매니저의 필드는 `URLSearchParam` `field`를 통해 자동 포커스될 수 있습니다. 입력 컴포넌트를 React의 <ExternalLink to="https://react.dev/reference/react/forwardRef" text="`forwardRef`"/> 메서드로 래핑하는 것이 권장되며, 해당 `ref`를 `input` 요소에 전달해야 합니다.

<br/>
</details>

**예시: 커스텀 텍스트 입력**

다음 예시에서는 제어되는 커스텀 텍스트 입력을 제공합니다. 모든 입력은 제어되어야 하며, 그렇지 않으면 저장 시 데이터가 제출되지 않습니다.

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/plugins/<plugin-name>/admin/src/components/Input.js"
import * as React from "react";

import { useIntl } from "react-intl";

const Input = React.forwardRef((props, ref) => {
  const { attribute, disabled, intlLabel, name, onChange, required, value } =
    props; // 콘텐츠 매니저에서 전달하는 props 중 일부입니다

  const { formatMessage } = useIntl();

  const handleChange = (e) => {
    onChange({
      target: { name, type: attribute.type, value: e.currentTarget.value },
    });
  };

  return (
    <label>
      {formatMessage(intlLabel)}
      <input
        ref={ref}
        name={name}
        disabled={disabled}
        value={value}
        required={required}
        onChange={handleChange}
      />
    </label>
  );
});

export default Input;
```

</TabItem>
<TabItem value="ts" label="TypeScript">

```tsx title="/src/plugins/<plugin-name>/admin/src/components/Input.ts"
import * as React from "react";

import { useIntl } from "react-intl";

const Input = React.forwardRef((props, ref) => {
  const { attribute, disabled, intlLabel, name, onChange, required, value } =
    props; // these are just some of the props passed by the content-manager

  const { formatMessage } = useIntl();

  const handleChange = (e) => {
    onChange({
      target: { name, type: attribute.type, value: e.currentTarget.value },
    });
  };

  return (
    <label>
      {formatMessage(intlLabel)}
      <input
        ref={ref}
        name={name}
        disabled={disabled}
        value={value}
        required={required}
        onChange={handleChange}
      />
    </label>
  );
});

export default Input;
```

</TabItem>
</Tabs>

:::tip
For a more detailed view of the props provided to the customFields and how they can be used check out the <ExternalLink to="https://github.com/strapi/strapi/blob/main/packages/plugins/color-picker/admin/src/components/ColorPickerInput.tsx#L80-L95" text="ColorPickerInput file"/> in the Strapi codebase.
:::

##### Options

`app.customFields.register()` can pass an additional `options` object. with the following parameters:

<details>
<summary>Parameters passed to the custom field <code>options</code> object:</summary>

| Options parameter | Description                                                                                                                               | Type                           |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `base`            | Settings available in the _Base settings_ tab of the field in the Content-type Builder                                                    | `Object` or `Array of Objects` |
| `advanced`        | Settings available in the _Advanced settings_ tab of the field in the Content-type Builder                                                | `Object` or `Array of Objects` |
| `validator`       | Validator function returning an object, used to sanitize input. Uses a <ExternalLink to="https://github.com/jquense/yup/tree/pre-v1" text="`yup` schema object"/>. | `Function`                     |

Both `base` and `advanced` settings accept an object or an array of objects, each object being a settings section. Each settings section could include:

- a `sectionTitle` to declare the title of the section as an <ExternalLink to="https://formatjs.io/docs/react-intl/" text="IntlObject"/>
- and a list of `items` as an array of objects.

Each object in the `items` array can contain the following parameters:

| Items parameter | Description                                                        | Type                                                 |
| --------------- | ------------------------------------------------------------------ | ---------------------------------------------------- |
| `name`          | Label of the input.<br/>Must use the `options.settingName` format. | `String`                                             |
| `description`   | Description of the input to use in the Content-type Builder        | `String`                                             |
| `intlLabel`     | Translation for the label of the input                             | <ExternalLink to="https://formatjs.io/docs/react-intl/" text="`IntlObject`"/> |
| `type`          | Type of the input (e.g., `select`, `checkbox`)                     | `String`                                             |

</details>

**Example: Declaring options for an example "color" custom field:**

In the following example, the `color-picker` plugin was created using the CLI generator (see [plugins development](/cms/plugins-development/developing-plugins.md)):

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/plugins/color-picker/admin/src/index.js"
// imports go here (ColorPickerIcon, pluginId, yup package…)

export default {
  register(app) {
    // ... app.addMenuLink() goes here
    // ... app.registerPlugin() goes here
    app.customFields.register({
      // …
      options: {
        base: [
          /*
            Declare settings to be added to the "Base settings" section
            of the field in the Content-Type Builder
          */
          {
            sectionTitle: {
              // Add a "Format" settings section
              id: "color-picker.color.section.format",
              defaultMessage: "Format",
            },
            items: [
              // Add settings items to the section
              {
                /*
                  Add a "Color format" dropdown
                  to choose between 2 different format options
                  for the color value: hexadecimal or RGBA
                */
                intlLabel: {
                  id: "color-picker.color.format.label",
                  defaultMessage: "Color format",
                },
                name: "options.format",
                type: "select",
                value: "hex", // option selected by default
                options: [
                  // List all available "Color format" options
                  {
                    key: "hex",
                    defaultValue: "hex",
                    value: "hex",
                    metadatas: {
                      intlLabel: {
                        id: "color-picker.color.format.hex",
                        defaultMessage: "Hexadecimal",
                      },
                    },
                  },
                  {
                    key: "rgba",
                    value: "rgba",
                    metadatas: {
                      intlLabel: {
                        id: "color-picker.color.format.rgba",
                        defaultMessage: "RGBA",
                      },
                    },
                  },
                ],
              },
            ],
          },
        ],
        advanced: [
          /*
            Declare settings to be added to the "Advanced settings" section
            of the field in the Content-Type Builder
          */
        ],
        validator: (args) => ({
          format: yup.string().required({
            id: "options.color-picker.format.error",
            defaultMessage: "The color format is required",
          }),
        }),
      },
    });
  },
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```tsx title="/src/plugins/color-picker/admin/src/index.ts"
// imports go here (ColorPickerIcon, pluginId, yup package…)

export default {
  register(app) {
    // ... app.addMenuLink() goes here
    // ... app.registerPlugin() goes here
    app.customFields.register({
      // …
      options: {
        base: [
          /*
            Declare settings to be added to the "Base settings" section
            of the field in the Content-Type Builder
          */
          {
            sectionTitle: {
              // Add a "Format" settings section
              id: "color-picker.color.section.format",
              defaultMessage: "Format",
            },
            items: [
              // Add settings items to the section
              {
                /*
                  Add a "Color format" dropdown
                  to choose between 2 different format options
                  for the color value: hexadecimal or RGBA
                */
                intlLabel: {
                  id: "color-picker.color.format.label",
                  defaultMessage: "Color format",
                },
                name: "options.format",
                type: "select",
                value: "hex", // option selected by default
                options: [
                  // List all available "Color format" options
                  {
                    key: "hex",
                    defaultValue: "hex",
                    value: "hex",
                    metadatas: {
                      intlLabel: {
                        id: "color-picker.color.format.hex",
                        defaultMessage: "Hexadecimal",
                      },
                    },
                  },
                  {
                    key: "rgba",
                    value: "rgba",
                    metadatas: {
                      intlLabel: {
                        id: "color-picker.color.format.rgba",
                        defaultMessage: "RGBA",
                      },
                    },
                  },
                ],
              },
            ],
          },
        ],
        advanced: [
          /*
            Declare settings to be added to the "Advanced settings" section
            of the field in the Content-Type Builder
          */
        ],
        validator: (args) => ({
          format: yup.string().required({
            id: "options.color-picker.format.error",
            defaultMessage: "The color format is required",
          }),
        }),
      },
    });
  },
};
```

</TabItem>
</Tabs>

<!-- TODO: replace these tip and links by proper documentation of all the possible shapes and parameters for `options` -->

:::tip
The Strapi codebase gives an example of how settings objects can be described: check the <ExternalLink to="https://github.com/strapi/strapi/blob/main/packages/core/content-type-builder/admin/src/components/FormModal/attributes/baseForm.ts" text="`baseForm.ts`"/> file for the `base` settings and the <ExternalLink to="https://github.com/strapi/strapi/blob/main/packages/core/content-type-builder/admin/src/components/FormModal/attributes/advancedForm.ts" text="`advancedForm.ts`"/> file for the `advanced` settings. The base form lists the settings items inline but the advanced form gets the items from an <ExternalLink to="https://github.com/strapi/strapi/blob/main/packages/core/content-type-builder/admin/src/components/FormModal/attributes/attributeOptions.js" text="`attributeOptions.js`"/> file.
:::

## Usage

<br/>

### In the admin panel

Custom fields can be added to Strapi either by installing them from the [Marketplace](/cms/plugins/installing-plugins-via-marketplace) or by creating your own.

Once added to Strapi, custom fields can be added to any content type. Custom fields are listed in the _Custom_ tab when selecting a field for a content-type.

<!-- TODO: add screenshot of content-type builder with custom fields tab here -->

Each custom field type can have basic and advanced settings. The <ExternalLink to="https://market.strapi.io/plugins?categories=Custom+fields" text="Marketplace"/> lists available custom fields, and hosts dedicated documentation for each custom field, including specific settings.

### In the code

Once created and used, custom fields are defined like any other attribute in the model's schema. 

Custom fields are explicitly defined in the [attributes](/cms/backend-customization/models#model-attributes) of a model with `type: customField`.

As compared to how other types of models are defined, custom fields' attributes also show the following specificities:

- Custom field have a `customField` attribute. Its value acts as a unique identifier to indicate which registered custom field should be used, and follows one of these 2 formats:

    | Format               |  Origin |
    |----------------------|------------------|
    | `plugin::plugin-name.field-name` | The custom field was created through a plugin |
    | `global::field-name` | The custom field is specific to the current Strapi application and was created directly within the `register` [function](/cms/configurations/functions) |

- Custom fields can have additional parameters depending on what has been defined when registering the custom field (see [server registration](#registering-a-custom-field-on-the-server) and [admin panel registration](#registering-a-custom-field-in-the-admin-panel)).

**Example: A simple `color` custom field model definition:**

```json title="/src/api/[apiName]/[content-type-name]/content-types/schema.json"

{
  // …
  "attributes": {
    "color": { // name of the custom field defined in the Content-Type Builder
      "type": "customField",
      "customField": "plugin::color-picker.color",
      "options": {
        "format": "hex"
      }
    }
  }
  // …
}
```
