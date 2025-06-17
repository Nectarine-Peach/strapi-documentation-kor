---
title: 드래프트 & 발행
description: Strapi 5의 드래프트 & 발행 기능을 활용해 콘텐츠 초안을 관리하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
toc_max_heading_level: 5
tags:
 - 콘텐츠 매니저
 - 콘텐츠 타입 빌더
 - 드래프트 & 발행
 - 초안 발행
 - 콘텐츠 미발행
 - 기능
---

# 드래프트 & 발행

드래프트 & 발행 기능을 사용하면 콘텐츠의 초안을 관리할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">없음</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본 제공(비활성화 상태)</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<Guideflow lightId="3r3wj5lbnk" darkId="ok8y5xehxp"/>

## 설정

**기능 설정 경로:** <Icon name="layout" /> Content Type Builder

콘텐츠 타입을 콘텐츠 매니저에서 드래프트 & 발행 기능으로 관리하려면, Content-type Builder에서 해당 기능을 활성화해야 합니다. 각 콘텐츠 타입별로 개별 설정이 가능합니다.

1. 이미 생성된 콘텐츠 타입을 수정하거나, 새 콘텐츠 타입을 생성합니다([Content Type Builder](/cms/features/content-type-builder) 문서 참고).
2. **고급 설정** 탭으로 이동합니다.
3. Draft & Publish 옵션을 체크합니다.
4. **완료** 버튼을 클릭합니다.

<ThemedImage
  alt="Content-type Builder의 고급 설정"
  sources={{
    light: '/img/assets/content-type-builder/advanced-settings.png',
    dark: '/img/assets/content-type-builder/advanced-settings_DARK.png',
  }}
/>

## 사용법

Draft & Publish가 활성화된 경우, [콘텐츠 매니저의 편집 뷰](/cms/features/content-manager#overview) 상단에서 해당 엔트리의 현재 상태를 확인할 수 있습니다. 콘텐츠는 3가지 상태를 가질 수 있습니다:

- <span style={{color:"#5cb176"}}>발행됨(Published)</span>: 이전에 발행된 콘텐츠로, 저장된 초안 변경사항이 없습니다.
- <span style={{color:"#ac73e6"}}>수정됨(Modified)</span>: 이전에 발행된 콘텐츠로, 초안 버전에서 변경 후 저장했지만 아직 발행하지 않은 상태입니다.
- <span style={{color:"#7b79ff"}}>초안(Draft)</span>: 아직 한 번도 발행되지 않은 콘텐츠입니다.

### 초안 작업하기

**경로:** <Icon name="feather" /> 콘텐츠 매니저, 해당 콘텐츠 타입의 편집 뷰

문서를 편집할 때 2개의 탭이 표시됩니다:

- _Draft_ 탭: 콘텐츠를 편집할 수 있는 영역입니다.
- _Published_ 탭: 읽기 전용으로, 발행된 버전의 필드 내용을 보여줍니다. (편집 불가)

<ThemedImage
  alt="초안 버전 편집 화면"
  sources={{
    light: '/img/assets/content-manager/editing_draft_version3.png',
    dark: '/img/assets/content-manager/editing_draft_version3_DARK.png',
  }}
/>

새로 생성된 콘텐츠는 기본적으로 초안 상태입니다. 초안은 **저장** 버튼을 통해 언제든 저장할 수 있으며, 발행 준비가 될 때까지 자유롭게 수정할 수 있습니다.

초안에서 변경사항을 저장하면, 오른쪽 _Entry_ 박스에서 3가지 옵션을 사용할 수 있습니다:
- **발행**: 문서를 발행합니다([초안 발행](#publishing-a-draft) 참고)
- **저장**: 초안을 저장합니다
- **변경사항 폐기**: <Icon name="dots-three-outline" /> 클릭 후 <Icon name="x-circle" /> **Discard changes** 선택

### 초안 발행하기

**경로:** <Icon name="feather" /> 콘텐츠 매니저, 해당 콘텐츠 타입의 편집 뷰

초안을 발행하려면, 오른쪽 _Entry_ 박스의 **발행** 버튼을 클릭하세요.

초안이 발행되면:
- _Draft_와 _Published_ 탭의 내용이 동일해집니다(단, _Published_ 탭은 읽기 전용).
- 문서 제목 아래 상태가 "발행됨"으로 변경됩니다.

:::caution
초안을 발행하기 전, 다른 미발행 콘텐츠와의 관계가 없는지 반드시 확인하세요. 그렇지 않으면 일부 콘텐츠가 API를 통해 노출되지 않을 수 있습니다.
:::

문서에 초안과 발행 버전이 모두 존재하는 경우, _Published_ 탭에서 발행 버전을 확인할 수 있습니다. 초안만 존재한다면 _Published_ 탭은 비활성화됩니다.

<ThemedImage
  alt="발행 버전 편집 화면"
  sources={{
    light: '/img/assets/content-manager/editing_published_version3.png',
    dark: '/img/assets/content-manager/editing_published_version3_DARK.png',
  }}
/>

:::tip
발행 예약(특정 날짜/시간에 초안을 발행) 기능은 [릴리즈 기능](/cms/features/releases) 문서를 참고하세요.
:::

### 콘텐츠 미발행 처리하기

**경로:** <Icon name="feather" /> 콘텐츠 매니저, 해당 콘텐츠 타입의 편집 뷰

이전에 발행된 콘텐츠를 미발행 처리하려면: _Draft_ 탭에서 오른쪽 _Entry_ 박스의 <Icon name="dots-three-outline" />를 클릭한 뒤 **Unpublish** 버튼을 선택하세요.

만약 초안 버전과 발행 버전의 내용이 다르다면, 미발행 시 두 버전의 콘텐츠를 어떻게 처리할지 선택할 수 있습니다:

1. _Draft_ 탭에서 오른쪽 _Entry_ 박스의 <Icon name="dots-three-outline" />를 클릭한 뒤 **Unpublish** 버튼을 선택합니다.
2. 확인 대화상자가 열리면, 다음 중 하나를 선택할 수 있습니다:
    - **Unpublish and keep last draft**, so that all the content you currently have in the _Draft_ tab is preserved, but the all the content from the _Published_ tab is definitely gone
    - **Unpublish and replace last draft** to discard any existing content in the _Draft_ tab and replace it with the content of all fields from the _Published_ tab
3. **Confirm** 버튼을 클릭하면 원하는 변경사항이 _Draft_와 _Published_ 탭에 적용되며 엔트리의 새로운 상태도 해당 엔트리 제목 아래에 반영됩니다.

<ThemedImage
  alt="콘텐츠 미발행 처리 화면"
  sources={{
    light: '/img/assets/content-manager/content-manager_unpublish.png',
    dark: '/img/assets/content-manager/content-manager_unpublish_DARK.png',
  }}
/>

### Bulk actions

**Path:** <Icon name="feather" /> Content Manager, list view of your content type

Selecting multiple entries from the [Content Manager's list view](/cms/features/content-manager#overview) will display additional buttons to publish or unpublish several entries simultaneously. This is what is called "bulk publishing/unpublishing".

:::caution
If the [Internationalization feature](/cms/features/internationalization) is installed, the bulk publish/unpublish actions only apply to the currently selected locale.
:::

<ThemedImage
  alt="Unpublish a document"
  sources={{
    light: '/img/assets/content-manager/bulk-publish.png',
    dark: '/img/assets/content-manager/bulk-publish_DARK.png',
  }}
/>

#### Bulk publishing drafts

To publish several entries at the same time:

1. From the list view of the Content Manager, select your entries to publish by ticking the box on the left side of the entries' record.
2. Click on the **Publish** button located above the header of the table.
3. In the _Publish entries_ dialog, check the list of selected entries and their status:
   - <Icon name="check-circle" color="rgb(58,115,66)"/> Ready to publish: the entry can be published
   - <Icon name="x-circle" color="rgb(190,51,33)" /> "[field name] is required", "[field name] is too short" or "[field name] is too long": the entry cannot be published because of the issue stated in the red warning message.
4. (optional) If some of your entries have a <Icon name="x-circle" color="rgb(190,51,33)" /> status, click the <Icon name="pencil-simple" /> edit buttons to fix the issues until all entries have the <Icon name="check-circle" color="rgb(58,115,66)"/> Ready to publish status. Note that you will have to click on the **Refresh** button to update the _Publish entries_ dialog as you fix the various entries issues.
5. Click the **Publish** button.
6. In the confirmation dialog box, confirm your choice by clicking again on the **Publish** button.

#### Bulk unpublishing content

To unpublish several entries at the same time:

1. From the list view of the Content Manager, select your entries to unpublish by ticking the box on the left side of the entries' record.
2. Click on the **Unpublish** button located above the header of the table.
3. In the confirmation dialog box, confirm your choice by clicking again on the **Unpublish** button.

### Usage with APIs

Draft or published content can be requested, created, updated, and deleted using the `status` parameter through the various front-end APIs accessible from [Strapi's Content API](/cms/api/content-api):

<CustomDocCardsWrapper>
<CustomDocCard icon="cube" title="REST API" description="Learn how to use the status parameter with the REST API." link="/cms/api/rest/status"/>
<CustomDocCard icon="cube" title="GraphQL API" description="Learn how to use the status parameter with GraphQL API." link="/cms/api/graphql#status"/>
</CustomDocCardsWrapper>

On the back-end server of Strapi, the Document Service API can also be used to interact with localized content:

<CustomDocCardsWrapper>
<CustomDocCard icon="cube" title="Document Service API" description="Learn how to use the status parameter with the Document Service API." link="/cms/api/document-service/status"/>
</CustomDocCardsWrapper>

