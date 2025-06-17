---
title: 감사 로그
description: Strapi 5의 감사 로그(Audit Logs) 기능을 사용하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
sidebar_position: 2
toc_max_heading_level: 5
tags:
- audit logs
- admin panel
- Enterprise feature
- payload
- features
---

# 감사 로그
<EnterpriseBadge />

<VersionBadge version="4.6.0" />

감사 로그(Audit Logs) 기능은 Strapi 애플리케이션 사용자가 수행한 모든 활동을 검색 및 필터링 가능한 형태로 보여줍니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">CMS 엔터프라이즈 플랜</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">프로젝트 관리자 패널의 슈퍼 관리자(Super Admin) 역할</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">필요한 플랜이 있다면 기본적으로 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<ThemedImage
  alt="감사 로그 패널"
  sources={{
    light: '/img/assets/settings/settings_audit-logs.png',
    dark: '/img/assets/settings/settings_audit-logs_DARK.png',
  }}
/>

## 사용법

**기능 사용 경로:** <Icon name="gear-six" /> 설정 > 관리자 패널 - 감사 로그

감사 로그 기능은 다음 이벤트를 기록합니다:

| 이벤트 | 동작 |
| --- | --- |
| 콘텐츠 타입 | `create`, `update`, `delete` |
| 항목(초안/발행) | `create`, `update`, `delete`, `publish`, `unpublish` |
| 미디어 | `create`, `update`, `delete` |
| 로그인 / 로그아웃 | `success`, `fail` |
| 역할 / 권한 | `create`, `update`, `delete` |
| 사용자 | `create`, `update`, `delete` |

각 로그 항목에는 다음 정보가 표시됩니다:

- 동작: 사용자가 수행한 동작의 유형(예: `create` 또는 `update`)
- 날짜: 동작이 수행된 날짜 및 시간
- 사용자: 동작을 수행한 사용자
- 세부 정보: 동작에 대한 더 많은 정보를 보여주는 모달(예: 사용자 IP 주소, 요청 본문, 응답 본문 등)


### 로그 필터링

기본적으로 모든 로그는 최신순(내림차순)으로 표시됩니다. 다음 기준으로 로그를 필터링할 수 있습니다:

- 동작: 필터링할 동작 유형 선택(예: `create` 또는 `update`)
- 사용자: 필터링할 사용자 선택
- 날짜: 필터링할 날짜(범위) 및 시간 선택

<ThemedImage
  alt="감사 로그 필터"
  sources={{
    light: '/img/assets/settings/settings_audit-logs-filters.png',
    dark: '/img/assets/settings/settings_audit-logs-filters_DARK.png',
  }}
/>

### 로그 세부 정보 접근 {#log-details}

어떤 로그 항목이든 <Icon name="eye" /> 아이콘을 클릭하면 해당 동작에 대한 더 많은 정보를 볼 수 있는 모달이 열립니다. 모달의 *Payload* 섹션에서는 세부 정보를 인터랙티브한 JSON 컴포넌트로 표시하여, JSON 객체를 펼치거나 접을 수 있습니다.

<ThemedImage
  alt="로그 세부 정보 모달"
  sources={{
    light: '/img/assets/settings/settings_log-details.png',
    dark: '/img/assets/settings/settings_log-details_DARK.png',
  }}
/>
