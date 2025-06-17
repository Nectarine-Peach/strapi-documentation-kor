---
title: 자주 묻는 질문(FAQ)
description: Strapi를 사용하면서 자주 겪는 문제와 그에 대한 답변 및 해결책을 확인하세요.
tags:
- 콘텐츠 타입
- 관리자 패널
- 배포
- 마이그레이션
- 콘텐츠 매니저
- 서버리스 환경
- PaaS
- 플러그인
- 다이나믹 존
- UUID
- 기본 ID 타입
- 기본 ID 이름
- SSL
- typescript

---

# 자주 묻는 질문(FAQ)

아래는 Strapi를 사용하면서 자주 겪는 문제와 그에 대한 답변 및 해결책입니다.

## 프로덕션/스테이징 환경에서 콘텐츠 타입을 생성하거나 수정할 수 없는 이유는?

Strapi는 모델 스키마를 정의하는 설정 파일(예: `./src/api/restaurant/content-types/restaurant/schema.json`)에 모델 정보를 저장합니다. Node.js의 동작 방식상, 변경 사항을 적용하려면 서버를 재시작해야 하며, 이는 서비스 중단을 유발할 수 있습니다. 또한 이러한 변경은 소스 관리 시스템에서 추적되어야 합니다.

일반적인 개발 플로우는 다음과 같습니다:

- 개발: 로컬에서 Strapi 애플리케이션을 개발하고, 변경 사항을 소스 컨트롤에 푸시
- 스테이징: 소스 컨트롤에서 변경 사항을 가져와 "프로덕션과 유사한" 환경에서 테스트
- 프로덕션: 추가 변경이 없다면 프로덕션에 배포
- 필요시 반복, 애플리케이션 버전 관리 및 테스트 권장

현재 및 앞으로도 프로덕션 환경에서 모델 생성/수정 기능을 제공할 계획이 없으며, 모델 설정을 데이터베이스로 옮길 계획도 없습니다. 이에 대한 우회 방법은 공식적으로 제공되지 않습니다.

## Strapi는 콘텐츠 배포나 마이그레이션을 지원하나요?

Strapi는 [데이터 전송](/cms/data-management/transfer) 기능을 제공하여, 한 Strapi 인스턴스에서 다른 인스턴스로(또는 파일 아카이브로) 콘텐츠를 내보내거나 가져올 수 있습니다. 환경 간 콘텐츠 마이그레이션에 유용합니다.

## 사용자가 관리자 패널에 로그인할 수 없는 경우

Strapi 3.0 베타 버전부터 일반 사용자(REST, GraphQL 사용자)와 관리자(관리자 패널 사용자)가 분리되었습니다. 일반 사용자는 관리자 패널 접근 권한을 가질 수 없습니다. 자세한 배경은 Strapi <ExternalLink to="https://strapi.io/blog/why-we-split-the-management-of-the-admin-users-and-end-users" text="블로그 글"/>을 참고하세요.

Strapi는 RBAC(역할 기반 접근 제어) 기능을 제공하여, 관리자 패널 내에서 사용자의 접근 권한을 세밀하게 제어할 수 있습니다. 역할별로 콘텐츠 타입, 싱글 타입, 플러그인, 설정 등에 대한 권한을 부여할 수 있습니다.

## PaaS 환경에서 데이터베이스와 업로드 파일이 초기화되는 이유는?

`--quickstart`로 Strapi 프로젝트를 생성하면 기본적으로 SQLite 데이터베이스가 사용됩니다. Heroku, DigitalOcean Apps, Google App Engine 등 PaaS 시스템의 파일 시스템은 <ExternalLink to="https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem" text="휘발성"/>이거나 읽기 전용이기 때문에, 컨테이너가 재시작될 때마다 파일 시스템의 변경 사항이 모두 사라집니다. SQLite와 로컬 업로드 파일이 파일 시스템에 저장되므로, 컨테이너가 재시작되면 데이터가 모두 삭제됩니다.

PostgreSQL 등 데이터베이스 애드온을 사용하고, 파일 업로드는 Cloudinary, AWS S3 등 외부 스토리지 서비스를 이용하는 것이 좋습니다.

## 무료 Strapi Cloud를 유료 플랜으로 업그레이드하려면?

Strapi Cloud는 무료 플랜을 제공합니다. <ExternalLink to="https://strapi.io/pricing-cloud" text="유료 플랜"/>으로 업그레이드하려면, Strapi Cloud 프로젝트의 설정에서 _Plans_ 섹션을 이용하세요(자세한 내용은 [Cloud 문서](/cloud/projects/settings#upgrading-to-another-plan) 참고).

## Strapi를 서버리스 환경에서 실행할 수 있나요?

Strapi는 구조상 서버리스 환경에 적합하지 않습니다. Strapi는 부팅 시 여러 작업을 수행하며, 서버리스 환경에서는 빠른 콜드 부팅이 필수입니다. Strapi는 항상 실행되는 서비스로 설계되어 있어, 서버리스 환경에서는 매 요청마다 수 초가 소요될 수 있습니다. 따라서 서버리스 환경에서의 사용은 권장하지 않습니다.

## Content Manager 레이아웃 설정을 모델 설정에 저장할 수 있나요?

현재 Strapi는 이를 지원하지 않습니다. 대신 `config:dump`와 `config:restore` 명령어로 환경 간 설정 마이그레이션을 쉽게 할 수 있습니다.

설정을 모델에 저장하지 않는 이유는 다음과 같습니다:

- 다국어(i18n) 및 관리자 인터페이스 번역 시 충돌 발생 가능성
- 역할 및 권한에 따라 레이아웃이 달라질 수 있음
- 동일한 모델이라도 기여자 인터페이스(예: 모바일 앱)마다 다를 수 있음

이러한 이유로, 설정을 모델 파일에 저장하는 것은 혼란을 초래할 수 있어 권장하지 않습니다. 대신 환경 간 마이그레이션을 쉽게 하는 방향으로 지원합니다.

## 플러그인 커스터마이징 방법은?

Strapi는 플러그인을 `node_modules`에 저장하며, [확장](/cms/plugins-development/plugins-extension) 시스템을 통해 특정 부분을 오버라이드할 수 있습니다.

## 3rd party 인증 제공자를 직접 추가할 수 있나요?

네, [공식 문서](/cms/configurations/users-and-permissions-providers/new-provider-guide)를 참고하거나, <ExternalLink to="https://github.com/strapi/strapi/tree/master/packages/plugins/users-permissions" text="users-permissions"/> 코드를 참고해 Pull Request를 보낼 수 있습니다. 향후에는 업로드 제공자와 유사한 구조로 분리할 계획이 있으나, 일정은 미정입니다.

## 기본 ID 타입이나 이름을 변경할 수 있나요?

아니요, 현재는 기본 id 이름이나 데이터 타입(예: PostgreSQL의 UUID)을 변경할 수 없습니다. 향후 지원이 논의되고 있습니다.

## 다이나믹 존 및 다형성 관계에서 필터링/딥 필터링이 가능한가요?

복잡성과 성능 문제로 인해, 다이나믹 존이나 다형성 관계에서의 필터링은 지원 계획이 없습니다.

## Strapi에서 SSL을 설정하려면?

Strapi는 자체적으로 SSL을 제공하지 않습니다. Node.js 애플리케이션을 저포트(예: 443)에서 직접 서비스하는 것은 보안상 매우 위험합니다. 리눅스에서는 1024번 이하 포트 바인딩에 root 권한이 필요하며, SSL 인증서 갱신 시 애플리케이션 재시작이 필요합니다.

따라서 <ExternalLink to="https://forum.strapi.io/t/nginx-proxing-with-strapi/" text="Nginx"/>, <ExternalLink to="https://forum.strapi.io/t/caddy-proxying-with-strapi/40616" text="Caddy"/>, <ExternalLink to="https://forum.strapi.io/t/haproxy-proxying-with-strapi/" text="HAProxy"/>, Apache, Traefik 등 프록시 서버를 통해 SSL을 처리하는 것이 좋습니다. [server.json](/cms/configurations/server)에서 프록시 관련 설정을 할 수 있습니다.

## Strapi 프로젝트에서 TypeScript를 사용할 수 있나요?

Strapi v4.2.0-beta.1부터 TypeScript가 공식 지원됩니다. 문서 곳곳에 TypeScript 예제가 포함되어 있으며, [전용 TypeScript 지원 페이지](/cms/typescript)도 참고하세요.

## `Error: Cannot find module @strapi/XXX` 빌드 오류 해결 방법

:::caution
아래 방법을 시도하기 전에, 프로젝트에서 패키지 매니저의 설치 명령어를 실행했는지 확인하세요.
:::

Strapi는 현재 버전에서 의존성 hoisting이 필요합니다.

대부분의 패키지 매니저는 기본적으로 hoisting을 지원하지만, 동작하지 않을 경우 설정 파일에서 강제로 활성화할 수 있습니다.

- npm 또는 pnpm 사용 시: 프로젝트의 `.npmrc` 파일에 `hoist=true` 추가. 자세한 내용은 <ExternalLink to="https://pnpm.io/npmrc#hoist" text="공식 pnpm 문서"/>
- Yarn 사용 시: `.yarnrc` 파일에서 `nmHoistingLimits` 설정. 자세한 내용은 <ExternalLink to="https://yarnpkg.com/configuration/yarnrc#nmHoistingLimits" text="Yarn 공식 문서"/>

## X 기능이 지원되나요?

<ExternalLink to="https://feedback.strapi.io/" text="공개 로드맵"/>에서 현재 개발 중이거나 요청된 기능을 확인하고, 새로운 기능 요청도 할 수 있습니다.
