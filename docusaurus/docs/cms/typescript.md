---
title: TypeScript
description: Strapi 애플리케이션에서 TypeScript 시작하기
displayed_sidebar: cmsSidebar
tags:
- 소개
- 타입스크립트
---

# TypeScript 
<VersionBadge version="4.3.0" />

<ExternalLink to="https://www.typescriptlang.org/" text="TypeScript"/>는 JavaScript 위에 추가적인 타입 시스템 레이어를 제공하므로, 유효한 JavaScript 코드는 모두 유효한 TypeScript 코드가 됩니다. Strapi 개발의 맥락에서 TypeScript는 애플리케이션의 보다 안전한 타입 코드베이스를 가능하게 하며, 자동 타입 생성 및 자동 완성을 위한 도구 세트를 제공합니다.

## Strapi에서 TypeScript 시작하기

Strapi에서 TypeScript를 시작하는 방법은 2가지가 있습니다:

- 터미널에서 다음 명령어를 실행하여 Strapi에서 새로운 TypeScript 프로젝트를 생성 (자세한 내용은 [CLI 설치](/cms/installation/cli) 문서에서 확인할 수 있습니다):

  <Tabs groupId="yarn-npm">

  <TabItem value="yarn" label="Yarn">

  ```bash
  yarn create strapi-app my-project --typescript
  ```
  
  </TabItem>

  <TabItem value="npm" label="NPM">

  ```bash
  npx create-strapi-app@latest my-project --typescript
  ```
  
  </TabItem>

  </Tabs>

- 제공된 [변환](/cms/typescript/adding-support-to-existing-project) 단계를 사용하여 기존 Strapi 프로젝트에 TypeScript 지원을 추가.

<br />

:::strapi 다음에 할 일은?
- TypeScript 기반 Strapi 프로젝트의 [구조](/cms/project-structure)를 이해하세요
- TypeScript와 관련된 [구성 옵션](/cms/configurations/typescript) 옵션에 대해 알아보세요
- TypeScript 관련 개발 [옵션 및 기능](/cms/typescript/development)에 대해 자세히 알아보세요
- 특정 사용 사례에 대한 [가이드](/cms/typescript/guides)를 읽어보세요
:::
