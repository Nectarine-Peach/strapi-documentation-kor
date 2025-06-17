---
title: Strapi 5로 업그레이드하는 단계별 가이드
description: Strapi v4에서 Strapi 5로 업그레이드하는 단계별 가이드를 따라하세요
sidebar_label: 단계별 가이드
tags:
- Strapi 5로 업그레이드
- 업그레이드 도구
- 주요 변경사항
- 가이드
---

import DoNotMigrateYet from '/docs/snippets/_do-not-migrate-to-v5-yet.md'

# Strapi 5로 업그레이드하는 단계별 가이드

Strapi의 최신 주요 버전은 Strapi 5입니다.

이 페이지는 Strapi v4 애플리케이션을 Strapi 5로 업그레이드하기 위한 단계별 지침으로 사용하기 위한 것입니다.

:::prerequisites
Strapi v4 애플리케이션이 이미 최신 v4 마이너 및 패치 버전에서 실행되고 있어야 합니다. 그렇지 않다면 [업그레이드 도구](/cms/upgrade-tool)를 `minor` 명령으로 실행하여 최신 버전에 도달하세요: `npx @strapi/upgrade minor`.
:::

## 1단계: 업그레이드 준비하기

업그레이드 과정 자체에 들어가기 전에 다음 예방 조치를 취하세요:

1. **데이터베이스를 백업하세요**.

  기본 구성으로 SQLite를 사용하고 있다면(Strapi와 함께 제공되는 기본 데이터베이스), 데이터베이스 파일은 `data.db`라는 이름으로 Strapi 애플리케이션 루트의 `.tmp/` 폴더에 위치합니다.
  
  다른 유형의 데이터베이스를 사용하고 있다면, 공식 문서를 참조하세요(<ExternalLink to="https://www.postgresql.org/docs/" text="PostgreSQL 문서"/> 및 <ExternalLink to="https://dev.mysql.com/doc/" text="MySQL 문서"/> 참조).

  프로젝트가 Strapi Cloud에 호스팅되어 있다면, 수동으로 [백업을 생성](/cloud/projects/settings#creating-a-manual-backup)할 수 있습니다.
2. **코드를 백업하세요**:
    * 코드가 git으로 버전 관리되고 있다면, 마이그레이션을 실행하기 위한 새로운 전용 브랜치를 생성하세요.
    * 코드가 git으로 버전 관리되고 있지 _않다면_, 작업 중인 Strapi v4 코드의 백업을 생성하여 안전한 장소에 저장하세요.
3. **사용 중인 플러그인이 Strapi 5와 호환되는지 확인하세요**.

  이를 위해 사용 중인 플러그인을 나열한 다음, <ExternalLink to="https://market.strapi.io/plugins?version=v5" text="마켓플레이스"/> 웹사이트에서 각각의 전용 문서를 읽어 호환성을 확인하세요.

## 2단계: 자동화된 마이그레이션 실행하기

Strapi는 Strapi 5로의 업그레이드 일부를 자동화하는 도구인 [업그레이드 도구](/cms/upgrade-tool)를 제공합니다.

1. **업그레이드 도구를 실행하세요**.  

  ```sh
  npx @strapi/upgrade major
  ```

  이 명령은 Strapi 5의 종속성 업데이트 및 설치를 실행하고, Strapi 5와 함께 제공되는 일부 주요 변경사항을 처리하기 위한 코드모드를 실행합니다.

  코드모드는 다음 변경사항을 처리합니다:

  | 코드모드 이름 및 GitHub 코드 링크 | 설명 |
  |-----------------------------------|-------------|
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/comment-out-lifecycle-files.code.ts" text="comment-out-lifecycle-files"/> | [Document Service Middlewares](/cms/migration/v4-to-v5/breaking-changes/lifecycle-hooks-document-service)를 위해 라이프사이클 파일을 주석 처리 | 
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/dependency-remove-strapi-plugin-i18n.json.ts" text="dependency-remove-strapi-plugin-i18n"/> | i18n이 이제 Strapi 코어에 통합되어 i18n 플러그인 종속성 제거 |
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/dependency-upgrade-react-and-react-dom.json.ts" text="dependency-upgrade-react-and-react-dom"/>  | react 및 react-dom 종속성 업그레이드 | 
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/dependency-upgrade-react-router-dom.json.ts" text="dependency-upgrade-react-router-dom"/>  | react-router-dom 종속성 업그레이드 |
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/dependency-upgrade-styled-components.json.ts" text="dependency-upgrade-styled-components"/>  | styled-components 종속성 업그레이드 |
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/deprecate-helper-plugin.code.ts" text="deprecate-helper-plugin"/>  | `@strapi/helper-plugin`에서의 마이그레이션 부분 처리 |
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/entity-service-document-service.code.ts" text="entity-service-document-service"/>            | Entity Service API에서 새로운 Document Service API로의 마이그레이션 부분 처리 |
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/s3-keys-wrapped-in-credentials.code.ts" text="s3-keys-wrapped-in-credentials"/>            | `aws-s3` 제공업체를 사용하는 사용자를 위해 `accessKeyId` 및 `secretAccessKey` 속성을 `credentials` 객체 내부로 래핑 | 
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/sqlite3-to-better-sqlite3.json.ts" text="sqlite3-to-better-sqlite3"/>                                                                    | sqlite 종속성을 better-sqlite3로 업데이트 | 
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/strapi-public-interface.code.ts" text="strapi-public-interface"/>                          | `@strapi/strapi` 가져오기를 새로운 공개 인터페이스를 사용하도록 변환 | 
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/use-uid-for-config-namespace.code.ts" text="use-uid-for-config-namespace"/>                | 가능한 경우 'plugin' 및 'api' 네임스페이스에서 config get/set/has의 문자열 점 형식을 uid 형식으로 교체 | 
  | <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/utils/upgrade/resources/codemods/5.0.0/utils-public-interface.code.ts" text="utils-public-interface"/>                            | 새로운 공개 인터페이스를 사용하도록 utils 업데이트 | 

:::tip
Strapi 플러그인을 개발한다면, 다른 코드모드가 helper-plugin 사용 중단의 일부 측면을 처리합니다. 자세한 정보는 [관련 주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/helper-plugin-deprecated)을 참조하세요.
:::

2. 업그레이드 도구에서 수행한 변경사항을 검토하여 **일부 코드 업데이트를 수동으로 완료해야 하는지 확인하세요**:

  코드모드에 의해 코드에 자동으로 추가된 `__TODO__`를 찾아보세요. 일부는 Entity Service API에서 Strapi 5에 도입된 새로운 Document Service API로 마이그레이션하는 동안 추가되었을 수 있습니다.
  
  :::info Document Service API
  Document Service API에 대한 추가 정보는 [주요 변경사항 항목 설명](/cms/migration/v4-to-v5/breaking-changes/entity-service-deprecated), [특정 마이그레이션 가이드](/cms/migration/v4-to-v5/additional-resources/from-entity-service-to-document-service), 및 [API 참조](/cms/api/document-service)에서 찾을 수 있습니다.
  :::

## 3단계: 수동 업그레이드 확인 및 처리

다음 주요 변경사항이 Strapi 애플리케이션에 영향을 줄 수 있으며 일부 수동 작업이 필요할 수 있습니다.

각각에 대해 표시된 주요 변경사항 항목을 읽고 업그레이드 도구가 실행된 후에도 여전히 수동 작업이 필요한지 확인하세요:

1. **데이터베이스 마이그레이션**:
    1. MySQL v5는 지원되지 않음 👉 [주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/mysql5-unsupported) 참조
    2. better-sqlite3만 지원됨 👉 [주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/only-better-sqlite3-for-sqlite) 참조
    3. mysql2만 지원됨 👉 [주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/only-mysql2-package-for-mysql) 참조
    4. 라이프사이클 훅이 다르게 트리거됨 👉 [주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/lifecycle-hooks-document-service) 참조
2. **구성**:
    1. 일부 환경 변수가 서버 구성에서 처리됨 👉 [주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/removed-support-for-some-env-options) 참조
    2. 사용자 정의 구성이 특정 요구사항을 충족해야 함 👉 [주요 변경사항](/cms/migration/v4-to-v5/breaking-changes/strict-requirements-config-files) 참조
3. **관리자 패널 커스터마이징**:
    * helper-plugin이 제거됨 👉 [마이그레이션 참조](/cms/migration/v4-to-v5/additional-resources/helper-plugin) 참조

👉 마지막으로, 관심 있을 수 있는 특수한 경우를 위해 나머지 [주요 변경사항 데이터베이스](/cms/migration/v4-to-v5/breaking-changes)를 검토하세요.

## 4단계: API 소비 측 마이그레이션

Strapi 5는 REST 및 GraphQL API를 모두 업데이트했습니다.

아래 단계를 따르고 역호환성 플래그와 가이드된 마이그레이션 리소스를 활용하여 Strapi 5용 코드를 점진적으로 업데이트하세요.

### REST API 호출 마이그레이션

1. `Strapi-Response-Format: v4` 헤더를 설정하여 역호환성 플래그를 활성화하세요.
2. 전용 [REST API용 주요 변경사항 항목](/cms/migration/v4-to-v5/breaking-changes/new-response-format)의 가이드에 따라 쿼리 및 뮤테이션만 업데이트하세요.
3. 클라이언트가 올바르게 실행되는지 검증하세요.
4. `Strapi-Response-Format: v4` 헤더를 제거하여 역호환성 플래그를 비활성화하고 새로운 응답 형식 사용을 시작하세요.

### GraphQL API 호출 마이그레이션

1. [the `/config/plugins.js|ts` 파일](/cms/plugins/graphql#code-based-configuration)의 `graphql.config` 객체에서 `v4ComptabilityMode`를 `true`로 설정하여 역호환성 플래그를 활성화하세요.
2. 전용 [GraphQL용 주요 변경사항 항목](/cms/migration/v4-to-v5/breaking-changes/graphql-api-updated)의 가이드에 따라 쿼리 및 뮤테이션만 업데이트하세요.
3. 클라이언트가 올바르게 실행되는지 검증하세요.
4. `v4ComptabilityMode`를 `false`로 설정하여 역호환성 플래그를 비활성화하고 새로운 응답 형식 사용을 시작하세요.
