---
title: 문서화 플러그인
displayed_sidebar: cmsSidebar
description: Swagger UI를 사용하여 API 문서화 플러그인은 문서 생성의 대부분의 어려움을 해결해줍니다.
toc_max_heading_level: 5
tags:
- 관리자 패널
- excludeFromGeneration 함수
- OpenAPI 사양
- override 서비스
- pluginOrigin
- 플러그인
- register 함수
- Swagger UI
---

# 문서화 플러그인

문서화 플러그인은 API 문서 생성을 자동화합니다. 기본적으로 swagger 파일을 생성하며 <ExternalLink to="https://swagger.io/specification/" text="Open API 사양 버전"/>을 따릅니다.

<IdentityCard isPlugin>
  <IdentityCardItem icon="navigation-arrow" title="위치">
    관리자 패널을 통해 사용 가능.<br/>관리자 패널과 서버 코드 양쪽에서 다른 옵션 세트로 설정.
  </IdentityCardItem>
  <IdentityCardItem icon="package" title="패키지 이름">
    `@strapi/plugin-documentation`
  </IdentityCardItem>
    <IdentityCardItem icon="plus-square" title="추가 리소스">
    <ExternalLink to="https://market.strapi.io/plugins/@strapi-plugin-documentation" text="Strapi 마켓플레이스 페이지" />
  </IdentityCardItem>
</IdentityCard>

:::caution 유지보수되지 않는 플러그인
문서화 플러그인은 현재 적극적으로 유지보수되지 않으며 Strapi 5와 작동하지 않을 수 있습니다.
:::

<Guideflow lightId="5pvjz4zswp" darkId="6kw4vdwizp"/>

설치되면 문서화 플러그인은 프로젝트의 모든 API와 설정에 지정된 플러그인에서 발견된 콘텐츠 타입과 라우트를 검사합니다. 그런 다음 플러그인은 <ExternalLink to="https://swagger.io/specification/" text="OpenAPI 사양"/>에 맞는 문서를 프로그래밍 방식으로 생성합니다. 문서화 플러그인은 <ExternalLink to="https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.1.0.md#paths-object" text="paths 객체"/>와 <ExternalLink to="https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.1.0.md#schema-object" text="schema 객체"/>를 생성하고 모든 Strapi 타입을 <ExternalLink to="https://swagger.io/docs/specification/data-models/data-types/" text="OpenAPI 데이터 타입"/>으로 변환합니다.

생성된 문서 JSON 파일은 애플리케이션의 다음 경로에서 찾을 수 있습니다: `src/extensions/documentation/documentation/<version>/full_documentation.json`

## 설치

문서화 플러그인을 설치하려면 터미널에서 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn add @strapi/plugin-documentation
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm install @strapi/plugin-documentation
```

</TabItem>

</Tabs>

플러그인이 설치되면 Strapi를 시작할 때 API 문서가 생성됩니다.

## 설정

문서화 플러그인의 대부분의 설정 옵션은 Strapi 프로젝트의 코드를 통해 처리됩니다. 관리자 패널에서 몇 가지 설정을 사용할 수 있습니다.

### 관리자 패널 설정

문서화 플러그인은 관리자 패널의 여러 부분에 영향을 미칩니다. 다음 표는 플러그인이 설치되면 Strapi 애플리케이션에 추가되는 모든 추가 옵션과 설정을 나열합니다:

| 영향받는 섹션    | 옵션 및 설정         |
|------------------|-------------------------------------------------------------|
| 문서화    | <ul>메인 네비게이션에 새로운 문서화 옵션 추가 <Icon name="info" /> - 문서를 <Icon name="eye" /> 열고 <Icon name="arrow-clockwise" /> 재생성하는 버튼이 있는 패널을 보여줍니다.</ul>        |
| 설정     | <ul><li>"문서화 플러그인" 설정 섹션 추가 - 문서 엔드포인트가 비공개인지 여부를 제어합니다([접근 제한](#restrict-access) 참고).<br/> 👉 경로 안내: <Icon name="gear-six" /> *설정 > 문서화 플러그인* </li><br/>  <li> 문서 접근, 업데이트, 삭제, 재생성에 대한 역할 기반 접근 제어 활성화. 관리자는 *플러그인* 탭과 *설정* 탭에서 다양한 사용자 유형에 대해 다른 접근 수준을 승인할 수 있습니다([사용자 및 권한 문서](/cms/features/users-permissions) 참고).<br/>👉 경로 안내: <Icon name="gear-six" /> *설정 > 관리 패널 > 역할* </li></ul>| 

#### API 문서 접근 제한 {#restrict-access}

기본적으로 API 문서는 누구나 접근할 수 있습니다.

API 문서 접근을 제한하려면 관리자 패널에서 **접근 제한** 옵션을 활성화하세요:

1. 관리자 패널의 메인 네비게이션에서 <Icon name="gear-six" /> *설정*으로 이동합니다.
2. **문서화**를 선택합니다.
3. **접근 제한**을 `ON`으로 토글합니다.
4. `password` 입력 필드에 비밀번호를 정의합니다.
5. 설정을 저장합니다.

### 코드 기반 설정

문서화 플러그인을 설정하려면 `src/extensions/documentation/config` 폴더에 `settings.json` 파일을 생성하세요. 이 파일에서 모든 환경 변수, 라이선스, 외부 문서 링크, 그리고 <ExternalLink to="https://swagger.io/specification/" text="사양"/>에 나열된 모든 항목을 지정할 수 있습니다.

다음은 설정 예시입니다:

```json title="src/extensions/documentation/config/settings.json"
{
  "openapi": "3.0.0",
  "info": {
    "version": "1.0.0",
    "title": "DOCUMENTATION",
    "description": "",
    "termsOfService": "YOUR_TERMS_OF_SERVICE_URL",
    "contact": {
      "name": "TEAM",
      "email": "contact-email@something.io",
      "url": "mywebsite.io"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "https://www.apache.org/licenses/LICENSE-2.0.html"
    }
  },
  "x-strapi-config": {
    "plugins": ["upload", "users-permissions"],
    "path": "/documentation"
  },
  "servers": [
    {
      "url": "http://localhost:1337/api",
      "description": "Development server"
    }
  ],
  "externalDocs": {
    "description": "Find out more",
    "url": "https://docs.strapi.io/developer-docs/latest/getting-started/introduction.html"
  },
  "security": [
    {
      "bearerAuth": []
    }
  ]
}
```

:::tip
커스텀 키를 추가해야 하는 경우 `x-` 접두사를 붙여주세요(예: `x-strapi-something`).
:::

#### 문서의 새 버전 생성 {#create-a-new-version-of-the-documentation}

새 버전을 생성하려면 `settings.json` 파일의 `info.version` 키를 변경하세요:

```json title="src/extensions/documentation/config/settings.json"
{
  "info": {
    "version": "2.0.0"
  }
}
```

이렇게 하면 자동으로 새 버전이 생성됩니다.

#### 문서 생성이 필요한 플러그인 정의 {#define-which-plugins}

플러그인을 문서 생성에 포함하려면 `x-strapi-config` 객체의 `plugins` 배열에 포함되어야 합니다. 기본적으로 배열은 `["upload", "users-permissions"]`로 초기화됩니다:

```json title="src/extensions/documentation/config/settings.json"
{
  "x-strapi-config": {
    "plugins": ["upload", "users-permissions"]
  }
}
```

커스텀 플러그인과 같은 더 많은 플러그인을 추가하려면 배열에 해당 이름을 추가하세요.

플러그인을 문서 생성에 포함하지 않으려면 빈 배열을 제공하세요(즉, `plugins: []`).

#### 생성된 문서 재정의

문서화 플러그인은 생성된 문서를 재정의하는 3가지 방법을 제공합니다: [`excludeFromGeneration`](#excluding-from-generation), [`registerOverride`](#register-override), [`mutateDocumentation`](#mutate-documentation).

##### excludeFromGeneration() {#excluding-from-generation}

특정 API나 플러그인이 생성되지 않도록 제외하려면 애플리케이션이나 플러그인의 [`register` 라이프사이클](/cms/plugins-development/admin-panel-api#register)에서 문서화 플러그인의 `override` 서비스에 있는 `excludeFromGeneration`을 사용하세요.

:::note
`excludeFromGeneration`은 무엇이 생성되는지에 대한 더 세밀한 제어를 제공합니다.

예를 들어, pluginA는 여러 새로운 API를 생성할 수 있고 pluginB는 그 API 중 일부에 대해서만 문서를 생성하기를 원할 수 있습니다. 이 경우 pluginB는 필요하지 않은 부분만 제외하여 필요한 생성된 문서의 이점을 여전히 누릴 수 있습니다.
:::

*****

| 매개변수 | 타입                       | 설명                                              |
| --------- | -------------------------- | -------------------------------------------------------- |
| `api`       | String 또는 String 배열 | 제외할 API/플러그인의 이름 또는 이름 목록 |

```js title="애플리케이션 또는 플러그인 register 라이프사이클"

module.exports = {
  register({ strapi }) {
    strapi
      .plugin("documentation")
      .service("override")
      .excludeFromGeneration(["users-permissions", "upload"]);
  },
};
```

##### registerOverride() {#register-override}

생성된 스키마의 특정 부분을 재정의하려면 `registerOverride` 함수를 사용하세요:

```js title="애플리케이션 또는 플러그인 register 라이프사이클"

module.exports = {
  register({ strapi }) {
    strapi
      .plugin("documentation")
      .service("override")
      .registerOverride({
        url: "/restaurants",
        method: "get",
        config: {
          override: true,
          // 여기에 커스텀 OpenAPI 스펙을 작성하세요
        }
      });
  },
};
```

##### mutateDocumentation() {#mutate-documentation}

전체 문서를 변경하려면 `mutateDocumentation` 함수를 사용하세요:

```js title="애플리케이션 또는 플러그인 register 라이프사이클"

module.exports = {
  register({ strapi }) {
    strapi
      .plugin("documentation")
      .service("override")
      .mutateDocumentation((documentation) => {
        // 전체 문서 객체를 변경
        delete documentation.paths["/api/restaurants"]["get"];
        return documentation;
      });
  },
};
```

## 사용법

문서화 플러그인은 <ExternalLink to="https://swagger.io/tools/swagger-ui/" text="Swagger UI"/>를 사용하여 API를 시각화합니다. UI에 접근하려면 관리자 패널의 메인 네비게이션에서 <Icon name="info" />를 선택하세요. 그런 다음 **문서 열기**를 클릭하여 Swagger UI를 엽니다. Swagger UI를 사용하여 API에서 사용 가능한 모든 엔드포인트를 보고 API 호출을 트리거할 수 있습니다.

:::tip
플러그인이 설치되면 다음 URL에서 플러그인 사용자 인터페이스에 접근할 수 있습니다:
`<서버-url>:<서버-포트>/documentation/<문서-버전>`
(예: <ExternalLink to="http://localhost:1337/documentation/v1.0.0" text="`localhost:1337/documentation/v1.0.0`"/>).
:::

### 문서 재생성 {#regenerate-documentation}

API를 변경한 후 문서를 업데이트하는 방법은 2가지입니다:

- 문서화 플러그인 설정에 지정된 문서 버전을 재생성하기 위해 애플리케이션을 재시작하거나,
- 문서화 플러그인 페이지로 가서 재생성하려는 문서 버전의 **재생성** 버튼을 클릭합니다.

### 요청 인증

Strapi는 기본적으로 보안이 적용되어 있어 대부분의 엔드포인트에서 사용자 인증이 필요합니다. CRUD 액션이 [사용자 및 권한 기능](/cms/features/users-permissions#roles)에서 Public으로 설정되지 않았다면 JSON 웹 토큰(JWT)을 제공해야 합니다. 이를 위해 API 문서를 보는 동안 **Authorize** 버튼을 클릭하고 _bearerAuth_ _value_ 필드에 JWT를 붙여넣으세요.
