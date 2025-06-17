---
title: 설정
description: Strapi 애플리케이션의 설정을 관리하고 커스터마이징하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_label: 소개
pagination_prev: cms/installation
pagination_next: cms/configurations/admin-panel
tags:
- 소개
- 설정
- 기본 설정
- 추가 설정
---

import ProjectStructureConfigFiles from '@site/src/components/ProjectStructureConfigFiles'

# 설정

Strapi 프로젝트의 설정은 `/config` 폴더에 있습니다:

<ProjectStructureConfigFiles />

<em style={{fontSize: '12px'}}>위의 블록은 프로젝트 구조에서 발췌한 것입니다. 보라색으로 표시된 파일명을 클릭하면 해당 문서를 읽을 수 있습니다. 전체 버전은 <a href="/cms/project-structure">프로젝트 구조 페이지</a>를 참고하세요.</em>

## 기본 설정

`/config` 폴더에서 다음과 같은 기본 설정을 찾고 정의할 수 있습니다:

| 설정 주제 | 파일 경로 | 필수 또는 선택 |
|-----|----|----|
| [데이터베이스](/cms/configurations/database) | `config/database` | 필수 |
| [서버](/cms/configurations/server) | `config/server` | 필수
| [관리자 패널](/cms/configurations/admin-panel) | `config/admin` | 필수 |
| [미들웨어](/cms/configurations/middlewares) | `config/middlewares` | 필수 |
| [API 호출](/cms/configurations/api) | `config/api` | 선택사항, 응답 및 기타 REST 관련 매개변수의 일반 설정을 정의하는 데 사용됩니다. |

## 특정 기능을 위한 추가 설정

일부 특정 기능에는 추가 설정이 필요합니다:

| 기능 | 위치 | 필수 또는 선택 |
|---------|------|------|
| [플러그인](/cms/configurations/plugins) | `config/plugins` 파일 | <ul><li>기본 프리셋으로 내장 플러그인만 사용하는 경우 선택사항</li><li>플러그인을 활성화, 설정 또는 비활성화하려면 필수</li></ul>업로드 플러그인(미디어 라이브러리 기능 처리) 및 GraphQL 설정에도 사용할 수 있습니다. |
| [TypeScript](/cms/configurations/typescript) | <ul><li>일반적인 [TypeScript 관련 설정](/cms/configurations/typescript#project-structure-and-typescript-specific-configuration-files)의 경우 `tsconfig.json`</li><li>Strapi 전용 [TypeScript 기능](/cms/configurations/typescript#strapi-specific-configuration-for-typescript)의 경우 `config/typescript` 파일</li></ul> | TypeScript를 효율적으로 사용하려면 필수 |
| [API 토큰](/cms/features/api-tokens) | `config/admin` 파일 | [사용자 및 권한 플러그인](/cms/features/users-permissions) 대신 인증에 API 토큰을 사용하는 경우 필수 |
| [라이프사이클 함수](/cms/configurations/functions) | `/src/index` 파일 | 서버 라이프사이클 중에 발생하는 다양한 작업을 수행하는 데 선택적으로 사용됩니다. `register`, `bootstrap`, `destroy` 함수가 포함됩니다. |
| [크론 작업](/cms/configurations/cron) | <ul><li>기능을 활성화하려면 `/config/server` 파일</li><li>작업을 선언하는 데 사용할 수 있는 전용 선택사항 `cron-tasks` 파일</li></ul> | 서버용 CRON 작업을 설정하려면 필수입니다. |
| [환경 변수](/cms/configurations/environment) | 환경별 전용 파일 및 폴더(예: `config/env/production/server`) | 다양한 환경과 해당 변수를 정의하는 데 선택적으로 사용됩니다. |
| [단일 로그인(SSO)](/cms/configurations/guides/configure-sso) <EnterpriseBadge /> <SsoBadge /> | `config/admin` 파일 | 프로젝트에서 SSO 기능이 활성화된 경우 사용하려면 필수입니다. |
| [기능 플래그](/cms/configurations/features) | `config/features` 파일 | 일반적인 안정적인 Strapi 애플리케이션에는 선택사항입니다.<br/>[미래 플래그](/cms/configurations/features)를 활성화하는 경우에만 필수입니다.|

## 가이드

다음 가이드는 Strapi 설정과 관련된 특정 사용 사례를 다루는 데 도움이 됩니다:

<CustomDocCard small title="역할 기반 접근 제어(RBAC)를 위한 커스텀 조건 생성 방법" link="/cms/configurations/guides/rbac" />

<CustomDocCard small title="환경 변수 접근 및 형변환 방법" link="/cms/configurations/guides/access-cast-environment-variables" />

<CustomDocCard small title="코드에서 설정 값에 접근하는 방법" link="/cms/configurations/guides/access-configuration-values" />
