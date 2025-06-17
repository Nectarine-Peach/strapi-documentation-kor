---
title: 백엔드 커스터마이징
description: Strapi 백엔드의 모든 요소(라우트, 정책, 미들웨어, 컨트롤러, 서비스, 모델, 요청, 응답, 웹훅 등)는 커스터마이징이 가능합니다.
pagination_next: cms/backend-customization/requests-responses
tags:
- 백엔드 커스터마이징
- 백엔드 서버
- 콘텐츠 타입 빌더
- 컨트롤러
- Document Service API
- 글로벌 미들웨어
- GraphQL API
- HTTP 서버
- 미들웨어
- Query Engine API
- REST API
- 라우트 미들웨어
---

<div className="custom-mermaid-layout">

:::strapi 용어 구분: Strapi 백엔드
Strapi는 헤드리스 CMS로, 전체 소프트웨어가 웹사이트/애플리케이션의 "백엔드" 역할을 합니다.
하지만 Strapi 자체는 2가지 부분으로 나뉩니다:

- **백엔드**: Strapi가 실행하는 HTTP 서버입니다. 일반적인 HTTP 서버처럼 요청을 받고 응답을 반환합니다. 콘텐츠는 데이터베이스에 저장되며, Strapi 백엔드는 데이터베이스와 상호작용하여 콘텐츠를 생성, 조회, 수정, 삭제합니다.
- **프론트엔드**: 관리자 패널로, 콘텐츠 구조화 및 관리를 위한 그래픽 UI를 제공합니다.

이 개발자 문서에서 '백엔드'란 _오직_ Strapi의 백엔드 부분만을 의미합니다.

[시작하기 > 관리자 패널](/cms/features/admin-panel)에서 관리자 패널 개요를, [관리자 패널 커스터마이징](/cms/admin-panel-customization)에서 다양한 커스터마이징 방법을 확인할 수 있습니다.
:::

Strapi 백엔드는 <ExternalLink to="https://koajs.com/" text="Koa"/> 기반의 HTTP 서버로 동작합니다.

일반적인 HTTP 서버처럼, Strapi 백엔드는 요청을 받고 응답을 반환합니다. [REST](/cms/api/rest) 또는 [GraphQL](/cms/api/graphql) API를 통해 데이터를 생성, 조회, 수정, 삭제할 수 있습니다.

요청이 Strapi 백엔드를 통과하는 흐름은 다음과 같습니다:

1. Strapi 서버가 [요청](/cms/backend-customization/requests-responses)을 받음
2. [글로벌 미들웨어](/cms/backend-customization/middlewares)가 순차적으로 실행됨
3. [라우트](/cms/backend-customization/routes)에 도달<br/>Strapi는 생성한 모든 콘텐츠 타입에 대해 기본 라우트 파일을 생성하며([REST API 문서](/cms/api/rest) 참고), 추가 라우트도 설정 가능
4. [라우트 정책](/cms/backend-customization/policies)은 읽기 전용 검증 단계로, 접근을 차단할 수 있음. [라우트 미들웨어](/cms/backend-customization/routes#middlewares)는 요청 흐름을 제어하거나 요청 자체를 변형할 수 있음
5. [컨트롤러](/cms/backend-customization/controllers)가 라우트에 도달하면 코드 실행. [서비스](/cms/backend-customization/services)는 컨트롤러에서 재사용 가능한 커스텀 로직을 추가로 구현할 때 사용
6. 컨트롤러/서비스에서 실행되는 코드는 데이터베이스에 저장된 콘텐츠 구조를 나타내는 [모델](/cms/backend-customization/models)과 상호작용<br />모델과의 데이터 상호작용은 [Document Service](/cms/api/document-service)와 [Query Engine](/cms/api/query-engine)이 담당
7. [Document Service 미들웨어](/cms/api/document-service/middlewares)로 쿼리 엔진에 전달되기 전 데이터를 제어할 수 있음. 쿼리 엔진은 라이프사이클 훅도 지원하지만, 데이터베이스 직접 접근이 필요한 경우가 아니라면 Document Service 미들웨어 사용 권장
7. 서버가 [응답](/cms/backend-customization/requests-responses)을 반환. 응답도 라우트 미들웨어와 글로벌 미들웨어를 거쳐 전송됨

글로벌/라우트 미들웨어 모두 비동기 콜백 함수(`await next()`)를 포함합니다. 미들웨어의 반환값에 따라 요청이 백엔드를 통과하는 경로가 달라집니다:

* 미들웨어가 아무것도 반환하지 않으면, 요청은 컨트롤러, 서비스, 데이터베이스 등 백엔드의 다양한 핵심 요소를 계속 통과합니다.
* 미들웨어가 `await next()` 호출 전 반환되면, 즉시 응답이 전송되어 나머지 요소를 건너뜁니다. 이후 응답도 동일한 체인을 따라 역방향으로 이동합니다.

:::info
이 섹션에서 설명하는 모든 커스터마이징은 REST API 전용입니다. [GraphQL 커스터마이징](/cms/plugins/graphql#customization)은 GraphQL 플러그인 문서를 참고하세요.
:::

<!-- TODO: v5용 백엔드 예제집이 준비되면 아래 주석 해제 -->
<!-- :::tip 예제로 배우기
실제 예시를 통해 학습하고 싶다면, [예제집](/cms/backend-customization/examples) 섹션에서 다양한 백엔드 커스터마이징 사례를 확인할 수 있습니다.
::: -->

## 인터랙티브 다이어그램

아래 다이어그램은 요청이 Strapi 백엔드를 통과하는 과정을 시각화한 것입니다. 각 도형을 클릭하면 관련 문서로 이동합니다.

<MermaidWithFallback
    chartFile="/diagrams/backend-customization.mmd"
    fallbackImage="/img/assets/diagrams/backend-customization.png"
    fallbackImageDark="/img/assets/diagrams/backend-customization_DARK.png"
    alt="백엔드 커스터마이징 다이어그램"
/>

</div>
