---
title: 리뷰 워크플로우
description: 다양한 콘텐츠 타입에 대한 워크플로우를 생성 및 관리할 수 있는 리뷰 워크플로우(Review Workflows) 기능 사용법을 알아보세요.
toc_max_heading_level: 5
tags:
- admin panel
- features
- Enterprise feature
- review workflows
---

# 리뷰 워크플로우
<EnterpriseBadge />

리뷰 워크플로우(Review Workflows) 기능을 사용하면 다양한 콘텐츠 타입에 대해 워크플로우를 생성 및 관리할 수 있습니다. 각 워크플로우는 여러 검토 단계를 가질 수 있어, 팀이 초안부터 발행까지 협업하며 콘텐츠를 관리할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">CMS 엔터프라이즈 플랜</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">프로젝트 관리자 패널의 슈퍼 관리자(Super Admin) 역할</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">필요한 플랜이 있다면 기본적으로 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<Guideflow lightId="zpez8vgs3p" darkId="lpx4d4zs4k"/>

## 설정

**기능 설정 경로:** <Icon name="gear-six" /> 설정 > 글로벌 설정 > 리뷰 워크플로우

[콘텐츠 매니저](/cms/features/content-manager)에서 리뷰 워크플로우를 사용하려면, 기본 워크플로우를 설정하거나 새 워크플로우를 생성해야 합니다.

기본 워크플로우에는 4단계가 포함되어 있습니다: To do, In progress, Ready to review, Reviewed. 각 단계는 필요에 따라 수정, 재정렬, 삭제할 수 있으며, 새 단계를 추가할 수도 있습니다.

### 새 워크플로우 생성

1. **새 워크플로우 생성** 버튼을 클릭하거나 워크플로우의 편집 버튼 <Icon name="pencil-simple" />을 클릭합니다.
2. 워크플로우 편집 화면에서 다음을 설정합니다:
    | 설정명   | 설명                                                             |
    | -------- | ---------------------------------------------------------------- |
    | 워크플로우 이름  | 워크플로우의 고유 이름을 입력합니다.                                         |
    | 연결 대상  | (선택) 이 워크플로우를 하나 이상의 기존 콘텐츠 타입에 할당합니다.             |
    | 단계         | 검토 단계를 추가합니다([새 단계 추가](#adding-a-new-stage) 참고).            |
3. **저장** 버튼을 클릭하면 새 워크플로우가 목록에 표시되고, 할당된 모든 콘텐츠 타입에 적용됩니다.

:::note
<ExternalLink to="https://strapi.io/pricing-cloud" text="워크플로우 및 단계별 최대 개수는 제한"/>되어 있습니다.
:::

### 워크플로우 편집

<ThemedImage
  alt="워크플로우 편집 화면"
  sources={{
    light: '/img/assets/review-workflows/edit-view-light.png',
    dark: '/img/assets/review-workflows/edit-view-dark.png',
  }}
/>

#### 새 단계 추가

1. **새 단계 추가** 버튼을 클릭합니다.
2. *단계 이름*을 입력합니다.
3. *색상*을 선택합니다.
4. 해당 단계에 있을 때 단계를 변경할 수 있는 *역할*을 선택합니다.
5. **저장** 버튼을 클릭합니다.

기본적으로 새 단계는 마지막에 추가되지만, <Icon name="dots-six-vertical" classes="ph-bold" /> 버튼으로 언제든지 순서를 변경할 수 있습니다.

:::tip
각 단계별로 역할을 설정할 때 "모든 단계에 적용"을 클릭하면 현재 역할을 모든 단계에 일괄 적용할 수 있습니다. 단계 컨텍스트 메뉴의 "단계 복제" 기능도 활용할 수 있습니다.
:::

#### 단계 복제

1. 단계의 컨텍스트 메뉴에서 **단계 복제**를 클릭합니다.
2. 복제된 단계의 이름을 변경합니다.
3. **저장** 버튼을 클릭합니다.

#### 단계 삭제

단계의 컨텍스트 메뉴에서 <Icon name="dots-three-outline" />를 클릭한 후 **삭제**를 선택하면 해당 단계를 삭제할 수 있습니다.

대기 중인 리뷰가 있는 단계를 삭제하면, 해당 리뷰는 워크플로우의 첫 번째 단계로 이동됩니다. 워크플로우에는 최소 한 단계가 반드시 존재해야 하므로 마지막 단계는 삭제할 수 없습니다.

### 워크플로우 삭제

워크플로우 목록에서 삭제 버튼 <Icon name="trash" />을 클릭하면 워크플로우를 삭제할 수 있습니다.

:::note
마지막 워크플로우는 삭제할 수 없습니다.
:::

## 사용법

**기능 사용 경로:** <Icon name="feather" /> 콘텐츠 매니저

### 리뷰 단계 변경 {#change-review-stage}

팀이 콘텐츠를 작성 및 수정하는 과정에서, 콘텐츠의 리뷰 단계를 워크플로우에 정의된 임의의 단계로 변경할 수 있습니다.

1. 콘텐츠 타입의 편집 화면에 접근합니다.
2. 화면 오른쪽의 *리뷰 워크플로우* 박스에서 _리뷰 단계_ 드롭다운을 클릭합니다.
3. 새 리뷰 단계를 선택하면 자동으로 저장됩니다.

<ThemedImage
  alt="리뷰 단계 드롭다운"
  sources={{
    light: '/img/assets/content-manager/review-stage-dropdown.png',
    dark: '/img/assets/content-manager/review-stage-dropdown_DARK.png',
  }}
/>

### 담당자 지정 {#change-assignee}

리뷰 워크플로우가 적용된 콘텐츠 타입의 항목은 Strapi의 모든 관리자에게 리뷰 담당자로 지정할 수 있습니다.

1. 콘텐츠 타입의 편집 화면에 접근합니다.
2. 화면 오른쪽의 *리뷰 워크플로우* 박스에서 _담당자_ 드롭다운을 클릭합니다.
3. 새 담당자를 선택하면 자동으로 저장됩니다.

<ThemedImage
  alt="리뷰 담당자 드롭다운"
  sources={{
    light: '/img/assets/content-manager/review-assignee-dropdown.png',
    dark: '/img/assets/content-manager/review-assignee-dropdown_DARK.png',
  }}
/>
