---
title: 주요 변경사항
description: Strapi v4와 v5 간에 도입된 모든 주요 변경사항 목록을 확인하세요.
displayed_sidebar: cmsSidebar
pagination_prev: cms/migration/v4-to-v5/step-by-step
pagination_next: cms/migration/v4-to-v5/additional-resources/introduction
tags:
 - 주요 변경사항
 - Strapi 5로 업그레이드
---

# Strapi v4에서 Strapi 5로의 주요 변경사항

이 페이지는 Strapi 5에서 도입된 모든 주요 변경사항을 나열합니다.
주요 변경사항은 주제별 카테고리로 그룹화되어 있으며, 다음 표의 각 줄에서 다음 정보를 확인할 수 있습니다:

- 주요 변경사항에 대한 간단한 설명,
- 그리고 다른 2개 열인 "플러그인에 영향" 및 "코드모드로 처리"는 주요 변경사항이 플러그인에도 영향을 주는지와 [업그레이드 CLI 도구](/cms/upgrade-tool)의 코드모드에 의해 자동으로 처리되는지를 요약합니다.

다음 표에서 주요 변경사항의 설명을 클릭하면 자세한 내용이 있는 해당 페이지로 이동할 수 있습니다.

:::tip 팁
* 사용 가능한 코드모드의 전체 목록을 보려면 터미널에서 `npx @strapi/upgrade codemods ls` 명령을 실행하세요.
* 코드모드에서 실행되는 코드를 자세히 살펴보려면 GitHub 저장소의 <ExternalLink to="https://github.com/strapi/strapi/tree/develop/packages/utils/upgrade/resources/codemods/5.0.0" text="코드모드 목록"/>을 확인하세요.
:::

## 데이터베이스

| 설명 | 플러그인에 영향 | 코드모드로 처리 |
|-------------|-----------------|---------------------|
| [콘텐츠 타입은 항상 기능 컬럼을 가집니다](/cms/migration/v4-to-v5/breaking-changes/database-columns) | 예 | 아니요|
| [MySQL v5는 더 이상 지원되지 않습니다](/cms/migration/v4-to-v5/breaking-changes/mysql5-unsupported) | 아니요 | 아니요 |
| [55자보다 긴 데이터베이스 식별자는 자동으로 단축됩니다](/cms/migration/v4-to-v5/breaking-changes/database-identifiers-shortened) | 예 | ✅ 예 |
| [SQLite 클라이언트는 `better-sqlite3` 패키지만 지원됩니다](/cms/migration/v4-to-v5/breaking-changes/only-better-sqlite3-for-sqlite) | 아니요 | ✅ 예 |
| [MySQL 클라이언트는 `mysql2` 패키지만 지원됩니다](/cms/migration/v4-to-v5/breaking-changes/only-mysql2-package-for-mysql) | 아니요 | ✅ 예 |

## 종속성

| 설명 | 플러그인에 영향 | 코드모드로 처리 |
|-------------|-----------------|---------------------|
| [CLI 기본 패키지 매니저가 더 이상 yarn이 아닙니다](/cms/migration/v4-to-v5/breaking-changes/yarn-not-default) | 아니요 | 아니요 |
| [Vite가 Strapi 5의 기본 번들러입니다](/cms/migration/v4-to-v5/breaking-changes/vite) | 예 | 아니요 |
| [Strapi 5는 `react-router-dom` v6을 사용합니다](/cms/migration/v4-to-v5/breaking-changes/react-router-dom-6) | 예 | ✅ 예 |
| [Strapi 5는 `koa-body` v6을 사용합니다](/cms/migration/v4-to-v5/breaking-changes/koa-body-v6) | 예 | 아니요 |
| [Webpack 별칭이 Strapi 5에서 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/webpack-aliases-removed) | 예 | 아니요 |
| [Apollo Server v3가 Apollo Server v4로 업그레이드됩니다](/cms/migration/v4-to-v5/breaking-changes/upgrade-to-apollov4) | 예 | 아니요 |

## 구성

| 설명 | 플러그인에 영향 | 코드모드로 처리 |
|-------------|-----------------|---------------------|
| [일부 `env` 전용 구성 옵션이 서버 구성에서 처리됩니다](/cms/migration/v4-to-v5/breaking-changes/removed-support-for-some-env-options) | 아니요 | 아니요 |
| [구성 파일명은 엄격한 요구사항을 충족해야 합니다](/cms/migration/v4-to-v5/breaking-changes/strict-requirements-config-files) | 아니요 | 아니요 |
| [서버 로그 레벨이 `http`입니다](/cms/migration/v4-to-v5/breaking-changes/server-default-log-level) | 아니요 | 아니요 |
| [모델 구성 경로는 점 표기법 대신 uid를 사용합니다](/cms/migration/v4-to-v5/breaking-changes/model-config-path-uses-uid) | 예 | 👷 부분적으로 |
| [`webhooks.populateRelations` 서버 구성이 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/remove-webhook-populate-relations) | 예 | 아니요 |
| [`public` 미들웨어에서 `defaultIndex` 옵션이 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/default-index-removed) | 아니요 | 아니요 |
| [서버 프록시 구성 옵션이 `server.proxy` 객체 아래로 그룹화됩니다](/cms/migration/v4-to-v5/breaking-changes/server-proxy) | 아니요 | 아니요 |

## Strapi 객체, 메서드, 패키지 및 백엔드 커스터마이징

| 설명 | 플러그인에 영향 | 코드모드로 처리 |
|-------------|-----------------|---------------------|
| [`strapi.fetch`는 네이티브 `fetch()` API를 사용합니다](/cms/migration/v4-to-v5/breaking-changes/fetch) | 예 | 아니요 |
| [strapi 팩토리 가져오기가 변경되었습니다](/cms/migration/v4-to-v5/breaking-changes/strapi-imports) | 예 | 👷 부분적으로 |
| [Strapi 5에서 `isSupportedImage` 메서드가 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/is-supported-image-removed) | 예 | 아니요 |
| [`strapi-utils`가 리팩토링되었습니다](/cms/migration/v4-to-v5/breaking-changes/strapi-utils-refactored) | 예 | ✅ 예 |
| [코어 서비스 메서드는 Document Service API를 사용합니다](/cms/migration/v4-to-v5/breaking-changes/core-service-methods-use-document-service) | 예 | 아니요 |
| [i18n이 이제 strapi 코어의 일부입니다](/cms/migration/v4-to-v5/breaking-changes/i18n-content-manager-locale) | 예 | ✅ 예 |

## 플러그인, 제공업체, 관리자 패널 및 프론트엔드 커스터마이징

| 설명 | 플러그인에 영향 | 코드모드로 처리 |
|-------------|-----------------|---------------------|
| [Users & Permissions `register.allowedFields`는 기본적으로 `[]`입니다](/cms/migration/v4-to-v5/breaking-changes/register-allowed-fields) | 아니요 | ✅ 예 |
| [`helper-plugin`이 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/helper-plugin-deprecated) | 예 | 👷 부분적으로 |
| [`injectContentManagerComponent()`가 `getPlugin('content-manager').injectComponent()`를 위해 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/inject-content-manager-component) | 예 | 아니요 |
| [일부 Mailgun 제공업체 레거시 변수가 지원되지 않습니다](/cms/migration/v4-to-v5/breaking-changes/mailgun-provider-variables) | 예 | 아니요 |
| [`lockIcon` 속성이 `licenseOnly`로 교체됩니다](/cms/migration/v4-to-v5/breaking-changes/license-only) | 예 | 아니요 |
| [`ContentManagerAppState` redux가 수정됩니다](/cms/migration/v4-to-v5/breaking-changes/redux-content-manager-app-state) | 예 | 아니요 |
| [`EditViewLayout`과 `ListViewLayout`이 리팩토링됩니다](/cms/migration/v4-to-v5/breaking-changes/edit-view-layout-and-list-view-layout-rewritten) | 예 | 아니요 |
| [관리자 패널 RBAC redux 스토어가 업데이트됩니다](/cms/migration/v4-to-v5/breaking-changes/admin-panel-rbac-store-updated) | 예 | 아니요 |
| [권한 제공업체 인스턴스의 `getWhere` 메서드가 제거됩니다](/cms/migration/v4-to-v5/breaking-changes/get-where-removed) | 예 | 아니요 |
| [디자인 시스템이 업그레이드됩니다](/cms/migration/v4-to-v5/breaking-changes/design-system) | 예 | 아니요 |

## Content API

| 설명 | 플러그인에 영향 | 코드모드로 처리 |
|-------------|-----------------|---------------------|
| [Strapi 5는 API 호출을 위한 새로운 평면화된 응답 형식을 가집니다](/cms/migration/v4-to-v5/breaking-changes/new-response-format) | 예 | 아니요 |
| [REST API 입력이 컨트롤러에서 기본적으로 검증됩니다](/cms/migration/v4-to-v5/breaking-changes/default-input-validation) | 예 | 아니요 |
| [GraphQL API가 업데이트됩니다](/cms/migration/v4-to-v5/breaking-changes/graphql-api-updated) | 예 | 아니요 |
| [Entity Service API는 사용 중단되고 Document Service API로 교체됩니다](/cms/migration/v4-to-v5/breaking-changes/entity-service-deprecated) | 예 | 👷 부분적으로 |
| [API 호출에서 `id` 대신 `documentId`를 사용해야 합니다](/cms/migration/v4-to-v5/breaking-changes/use-document-id) | 예 | 👷 부분적으로 |
| [데이터베이스 라이프사이클 훅이 Document Service API 메서드에 따라 다르게 트리거됩니다](/cms/migration/v4-to-v5/breaking-changes/lifecycle-hooks-document-service) | 예 | 아니요 |
| [`publicationState` 매개변수는 지원되지 않으며 `status`로 교체됩니다](/cms/migration/v4-to-v5/breaking-changes/publication-state-removed) | 예 | ✅ 예 |
| [Draft & Publish가 비활성화된 콘텐츠 타입은 항상 publishedAt 값이 날짜로 설정됩니다](/cms/migration/v4-to-v5/breaking-changes/publishedat-always-set-when-dandp-disabled) | 예 | 아니요 |
| [id로 정렬하여 시간순으로 정렬하는 것이 더 이상 불가능합니다](/cms/migration/v4-to-v5/breaking-changes/sort-by-id) | 예 | ✅ 예 |
| [Document Service API에는 `findPage()` 메서드가 없습니다](/cms/migration/v4-to-v5/breaking-changes/no-find-page-in-document-service) | 예 | 아니요 |
| [일부 속성과 콘텐츠 타입 이름이 Strapi에 의해 예약됩니다](/cms/migration/v4-to-v5/breaking-changes/attributes-and-content-types-names-reserved) | 예 | 아니요 |
| [항목 생성 시 파일 업로드가 더 이상 불가능합니다](/cms/migration/v4-to-v5/breaking-changes/no-upload-at-entry-creation) | 예 | 아니요 |
| [컴포넌트와 동적 영역은 세부 인구 전략을 사용하여 채워야 합니다](/cms/migration/v4-to-v5/breaking-changes/no-shared-population-strategy-components-dynamic-zones) | 예 | 아니요 |
