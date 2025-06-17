---
title: 릴리즈
description: 콘텐츠 관리자가 엔트리를 조직화하여 동시에 발행/미발행 작업을 수행할 수 있는 릴리즈 기능 사용법을 알아보세요
toc_max_heading_level: 5
tags:
- 관리자 패널
- 기능
- Enterprise 기능
- Growth 기능
- 릴리즈
---

# 릴리즈
<GrowthBadge/>

릴리즈 기능을 사용하면 콘텐츠 관리자가 엔트리를 컨테이너로 조직화하여 동시에 발행 및 미발행 작업을 수행할 수 있습니다. 릴리즈에는 다양한 콘텐츠 타입의 엔트리가 포함될 수 있으며 로케일을 혼합할 수도 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">CMS Growth 및 Enterprise 플랜</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">프로젝트 관리자 패널의 관리자 역할</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">필요한 플랜이 있으면 기본적으로 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<Guideflow lightId="3r3wvy5tnk" darkId="xrgw472swp"/>

## 구성

릴리즈에 콘텐츠를 포함하고 해당 릴리즈를 예약 및 발행하려면 먼저 릴리즈를 생성해야 합니다. 또한 발행을 예약할 때 사용할 릴리즈의 기본 시간대를 수정하고, 더 이상 사용되지 않거나 관련이 없는 릴리즈를 삭제할 수도 있습니다.

### 기본 시간대 선택

**기능 구성 경로:** <Icon name="gear-six" /> Settings

1. _기본 시간대_ 드롭다운 목록을 클릭하고 사용할 기본 시간대를 선택하세요.
2. **Save**를 클릭하세요.

<ThemedImage
  alt="릴리즈 설정"
  sources={{
    light: '/img/assets/releases/release-settings.png',
    dark: '/img/assets/releases/release-settings_DARK.png',
  }}
/>

### 릴리즈 생성

**기능 구성 경로:** <Icon name="paper-plane-tilt" /> Releases

1. 오른쪽 상단의 <Icon name="plus" classes="ph-bold" /> **New Release** 버튼을 클릭하세요.  
2. 릴리즈에 이름을 부여하세요.
3. _(선택사항)_ 릴리즈를 수동으로 발행하는 대신 예약 발행하려면 **Schedule release** 체크박스를 선택하고 발행할 날짜, 시간, 시간대를 정의하세요.
4. **Continue** 버튼을 클릭하세요.

:::tip
<Icon name="dots-three-outline" />와 <Icon name="pencil-simple" /> **Edit** 버튼을 사용하여 릴리즈를 편집하면 나중에 릴리즈 이름을 변경할 수 있습니다.
:::

<ThemedImage
  alt="새 릴리즈 추가"
  sources={{
    light: '/img/assets/releases/new-release.png',
    dark: '/img/assets/releases/new-release_DARK.png',
  }}
/>

<!-- TO INTEGRATE IF THE CALLOUT ISN'T ENOUGH

### 릴리즈 이름 바꾸기

릴리즈 이름을 바꿀 수 있습니다. 릴리즈 페이지에서:

1. 관리자 패널 오른쪽 상단의 <Icon name="dots-three-outline" /> 버튼을 클릭하세요.
2. <Icon name="pencil-simple" /> **Edit**을 선택하세요.
3. 모달에서 _Name_ 필드의 릴리즈 이름을 변경하세요.
4. **Continue**를 클릭하여 변경사항을 저장하세요.-->

### 릴리즈 삭제

**경로:** <Icon name="paper-plane-tilt" /> Releases

릴리즈를 삭제하면 릴리즈 자체만 삭제되고, 릴리즈에 포함된 콘텐츠 타입 엔트리는 삭제되지 않습니다.

1. 관리자 패널 오른쪽 상단의 <Icon name="dots-three-outline" /> 버튼을 클릭하세요.
2. <Icon name="trash" /> **Delete**를 선택하세요.
3. 확인 대화상자에서 <Icon name="trash" /> **Confirm**을 클릭하세요.

## 사용법

**기능 사용 경로:** <Icon name="paper-plane-tilt" /> Releases 및 <Icon name="feather" /> Content Manager

:::caution
릴리즈로 엔트리를 발행한다는 것은 초안 엔트리를 발행된 엔트리로 전환하는 것을 의미하므로, 콘텐츠 타입에서 [Draft & Publish](/cms/features/draft-and-publish)가 비활성화되어 있으면 릴리즈가 작동하지 않습니다.
:::

### 릴리즈에 콘텐츠 포함하기

:::prerequisites
- 엔트리를 릴리즈에 추가하기 전에 <Icon name="paper-plane-tilt" /> Releases 페이지에서 릴리즈를 생성해야 합니다.
- 릴리즈에 콘텐츠를 추가하려면 Content-Releases 플러그인에 대한 적절한 권한이 필요합니다([관리자 역할 구성](/cms/features/users-permissions) 참고).
:::

#### 한 번에 하나의 엔트리

**경로:** <Icon name="feather" /> Content Manager의 편집 보기

1. 인터페이스 오른쪽의 _Entry_ 영역에서 <Icon name="dots-three-outline" />를 클릭하세요.
2. 목록에서 <Icon name="paper-plane-tilt" /> **Add to release** 버튼을 클릭하세요.
2. 이 엔트리를 추가할 릴리즈를 선택하세요.
3. 릴리즈 자체가 발행될 때 엔트리를 발행할지 미발행할지에 따라 **Publish** 또는 **Unpublish** 버튼을 클릭한 다음 **Continue**를 클릭하세요.

오른쪽의 *Releases* 박스에 엔트리가 포함된 릴리즈가 표시됩니다.

:::info
[릴리즈 예약](/cms/features/releases#scheduling-a-release)이 활성화되어 있고 엔트리가 예약된 릴리즈에 추가된 경우, 릴리즈 날짜와 시간도 표시됩니다.
:::

#### 한 번에 여러 엔트리

**경로:** <Icon name="feather" /> Content Manager의 목록 보기

1. 엔트리 기록 왼쪽의 박스를 체크하여 추가하려는 엔트리를 선택하세요.
2. 테이블 헤더 위에 있는 **Add to release** 버튼을 클릭하세요.
3. 모달에서 이들 엔트리를 추가할 릴리즈를 선택하세요.
4. 릴리즈가 발행될 때 이들 엔트리를 발행할지 미발행할지 결정하려면 **Publish** 또는 **Unpublish** 버튼을 클릭한 다음 **Continue**를 클릭하세요.

<ThemedImage
  alt="릴리즈에 콘텐츠 포함하기"
  sources={{
    light: '/img/assets/releases/releases-cm-list-view.png',
    dark: '/img/assets/releases/releases-cm-list-view_DARK.png',
  }}
/>

### 릴리즈에서 콘텐츠 제거하기 {#removing-an-entry-from-a-release}

**경로:** <Icon name="feather" /> Content Manager의 편집 보기

1. 오른쪽 사이드바의 *Releases* 박스에서 릴리즈 이름 아래의 <Icon name="dots-three-outline" />를 클릭하세요.
2. **Remove from release** 버튼을 클릭하세요.

### 릴리즈 예약

**경로:** <Icon name="paper-plane-tilt" /> Releases

릴리즈는 [수동으로 발행](#publishing-a-release)하거나 지정된 날짜와 시간에 원하는 시간대로 자동 발행되도록 예약할 수 있습니다.

릴리즈를 예약할 수 있는 경우:
- [릴리즈를 생성](#creating-a-release)할 때,
- 또는 릴리즈가 이미 생성된 후 편집을 통해.

기존 릴리즈를 예약하려면, 릴리즈 페이지에서:
1. 관리자 패널 오른쪽 상단의 <Icon name="dots-three-outline" /> 버튼을 클릭하세요.
2. <Icon name="pencil-simple" /> **Edit**을 선택하세요.
3. 모달에서 **Schedule release** 체크박스를 선택하세요.
4. 릴리즈가 발행될 날짜, 시간, 시간대를 선택하세요.
5. **Save**를 클릭하세요.

<ThemedImage
  alt="릴리즈 예약"
  sources={{
    light: '/img/assets/releases/release-scheduling.png',
    dark: '/img/assets/releases/release-scheduling_DARK.png',
  }}
/>

<!--
:::tip
릴리즈 페이지에서 엔트리를 로케일, 콘텐츠 타입, 또는 작업(발행 또는 미발행)별로 그룹화하여 표시할 수 있습니다. 엔트리 그룹화 방식을 변경하려면 **Group by …** 드롭다운을 클릭하고 목록에서 옵션을 선택하세요.
:::
-->

### 릴리즈 발행

**경로:** <Icon name="paper-plane-tilt" /> Releases

릴리즈를 발행한다는 것은 릴리즈에 포함된 각 엔트리에 대해 정의된 모든 작업(발행 또는 미발행)이 동시에 수행된다는 의미입니다. 릴리즈를 발행하려면 관리자 패널 오른쪽 상단의 **Publish** 버튼을 클릭하세요.

_Status_ 열에는 각 엔트리의 상태가 표시됩니다:

- <Icon name="check-circle" color="rgb(58,115,66)"/> 이미 발행됨: 해당 엔트리는 이미 발행된 상태이며, 릴리즈를 발행해도 이 엔트리는 변경되지 않습니다.
- <Icon name="check-circle" color="rgb(58,115,66)"/> 이미 미발행됨: 해당 엔트리는 이미 미발행 상태이며, 릴리즈를 발행해도 이 엔트리는 변경되지 않습니다.
- <Icon name="check-circle" color="rgb(58,115,66)"/> 발행 준비 완료: 릴리즈와 함께 발행할 준비가 된 엔트리입니다.
- <Icon name="check-circle" color="rgb(58,115,66)"/> 미발행 준비 완료: 릴리즈와 함께 미발행할 준비가 된 엔트리입니다.
- <Icon name="x-circle" color="rgb(190,51,33)" /> 발행 불가: 일부 필드가 올바르게 입력되지 않았거나, 발행에 필요한 단계를 완료하지 않아 발행할 수 없습니다. 이 경우 릴리즈는 *Blocked*로 표시되며, 모든 문제를 해결해야 발행할 수 있습니다.

일부 엔트리가 <Icon name="x-circle" color="rgb(190,51,33)" /> 상태라면, <Icon name="dots-three-outline" />를 클릭하고 **Edit the entry** 버튼을 눌러 문제를 해결하세요. 각 엔트리의 문제를 해결할 때마다 **Refresh** 버튼을 눌러 릴리즈 페이지를 갱신해야 합니다.

:::caution
릴리즈를 한 번 발행하면 해당 릴리즈는 더 이상 수정할 수 없습니다. 동일한 엔트리 그룹으로 일부만 수정해 다시 릴리즈할 수 없으며, 반드시 새 릴리즈를 생성해야 합니다.
:::

<ThemedImage
  alt="릴리즈 발행"
  sources={{
    light: '/img/assets/releases/publish-release.png',
    dark: '/img/assets/releases/publish-release_DARK.png',
  }}
/>
