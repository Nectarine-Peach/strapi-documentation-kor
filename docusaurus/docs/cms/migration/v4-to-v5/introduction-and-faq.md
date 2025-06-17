---
title: Strapi 5로 업그레이드 - 소개 및 FAQ
description: Strapi 5로 업그레이드하는 방법에 대해 자세히 알아보세요
sidebar_label: 소개 및 FAQ
pagination_prev: cms/upgrades
pagination_next: cms/migration/v4-to-v5/step-by-step
tags:
- Strapi 5로 업그레이드
---

# Strapi 5로 업그레이드: 소개 및 FAQ

Strapi의 최신 주요 버전은 Strapi 5입니다. Strapi v4는 2026년 3월까지 계속 지원됩니다.

Strapi 5로 업그레이드할 준비가 되었다고 생각되면, 현재 페이지가 도움이 될 것입니다. 여기서는 Strapi 4에서 Strapi 5로 업그레이드하기 위한 모든 사용 가능한 리소스를 나열하고 일반적인 질문에 답변합니다.

## 사용 가능한 리소스

다음의 모든 사용 가능한 리소스는 가장 일반적인 것부터 가장 구체적인 사용 사례까지 애플리케이션과 플러그인을 Strapi 5로 업그레이드하는 데 도움이 될 것입니다:

<CustomDocCard emoji="1️⃣" title="단계별 가이드" description="업그레이드 프로세스의 개요를 보려면 이 가이드를 먼저 읽어보세요." link="/cms/migration/v4-to-v5/step-by-step" />
<CustomDocCard emoji="2️⃣" title="업그레이드 도구 참조" description="업그레이드 도구가 Strapi v4 애플리케이션의 일부를 Strapi 5로 자동 마이그레이션하는 방법에 대해 자세히 알아보세요." link="/cms/upgrade-tool" />
<CustomDocCard emoji="3️⃣" title="주요 변경사항 목록" description="Strapi v4와 v5 간의 차이점, 그로 인한 주요 변경사항, 그리고 수동으로 또는 업그레이드 도구와 함께 제공되는 코드모드의 도움을 받아 처리하는 방법에 대해 자세히 읽어보세요." link="/cms/migration/v4-to-v5/breaking-changes" />
<CustomDocCard emoji="4️⃣" title="특정 리소스" description="새로운 Document Service API를 위한 Entity Service API의 지원 중단, 플러그인 마이그레이션, helper-plugin의 지원 중단과 같은 특정 사용 사례를 처리합니다." link="/cms/migration/v4-to-v5/additional-resources/introduction" />

## 자주 묻는 질문

다음 질문과 답변은 가장 일반적인 사용 사례를 다루는 데 도움이 될 것입니다:

<details style={{backgroundColor: 'transparent', border: 'solid 1px #4945ff' }}>
<summary style={{fontSize: '18px'}}>업그레이드와 최신 의존성 설치를 어떻게 처리할 수 있나요?<br/>코드의 주요 변경사항을 어떻게 처리하고 코드를 Strapi 5에 적응시킬 수 있나요?</summary>

Strapi는 프로세스를 쉽게 하기 위해 [업그레이드 도구](/cms/upgrade-tool)를 제공합니다. 업그레이드 도구는 의존성 업그레이드와 **코드모드** <Codemods/> 실행을 처리하는 일부 명령어가 있는 명령줄 도구입니다.

Strapi 5로 업그레이드하는 맥락에서 이 도구를 사용하는 방법을 알아보려면 <a href="/cms/migration/v4-to-v5/step-by-step">단계별 가이드</a>를 따르세요.

Strapi 5 문서는 또한 [완전한 주요 변경사항 데이터베이스](/cms/migration/v4-to-v5/breaking-changes)와 특정 사용 사례를 다루는 [전용 리소스](/cms/migration/v4-to-v5/additional-resources/introduction)를 제공합니다.

</details>

<details style={{backgroundColor: 'transparent', border: 'solid 1px #4945ff' }}>
<summary style={{fontSize: '18px'}}>데이터 마이그레이션을 어떻게 처리하여 Strapi 5에서 애플리케이션이 여전히 작동하도록 보장할 수 있나요?</summary>
<p>Strapi 5는 Strapi 5에서 애플리케이션이 처음으로 시작될 때 한 번 실행되는 일련의 데이터 마이그레이션 스크립트를 통합합니다.</p>
<p>하지만 <a href="/cms/migration/v4-to-v5/step-by-step">단계별 가이드</a>에서 지시하는 바와 같이 업그레이드를 수행하기 전에 <strong>항상 데이터베이스를 백업</strong>하세요(SQL 데이터베이스를 사용하는 경우 기본적으로 <code style={{color: 'rgb(73, 69, 255)', backgroundColor: 'rgb(240, 240, 255)'}}>.tmp/data.db</code>에 있습니다).</p>
</details>

<details style={{backgroundColor: 'transparent', border: 'solid 1px #4945ff' }}>
<summary style={{fontSize: '18px'}}>Strapi Cloud 고객으로서 Strapi 5 애플리케이션의 전체 업그레이드와 배포를 어떻게 처리할 수 있나요?</summary>

1. [백업을 생성](/cloud/projects/settings#backups)하고 <a href="/cms/migration/v4-to-v5/step-by-step">단계별 가이드</a>를 따라 로컬에서 코드를 업데이트하세요.
2. [Cloud CLI](/cloud/cli/cloud-cli)에서 `yarn deploy` 또는 `npm run deploy` 명령어를 실행하세요.<br/>

Strapi Cloud는 Strapi 5에서 업데이트된 코드를 배포하고 자동으로 데이터 마이그레이션 스크립트를 실행합니다.
<br/>

</details>
