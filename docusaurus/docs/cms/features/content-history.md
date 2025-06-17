---
title: 콘텐츠 이력
description: Strapi 5의 콘텐츠 이력(Content History) 기능을 사용하여 콘텐츠 매니저에서 문서의 이전 버전을 탐색하고 복원하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
toc_max_heading_level: 5
tags:
 - content manager
 - content history
 - features
---

# 콘텐츠 이력
<GrowthBadge /> <EnterpriseBadge/> <VersionBadge version="5.0.0" />

<Icon name="feather" /> 콘텐츠 매니저의 콘텐츠 이력(Content History) 기능을 사용하면 [콘텐츠 매니저](/cms/features/content-manager)로 생성한 문서의 이전 버전을 탐색하고 복원할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">CMS Growth 또는 Enterprise 플랜</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">없음</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">필요한 플랜이 있다면 기본적으로 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 운영 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<Guideflow lightId="9r2m2y1sok" darkId="er566mli6p"/>

## 사용법

**기능 사용 경로:** <Icon name="feather" /> 콘텐츠 매니저 <br/> 콘텐츠 타입의 편집 화면에서: 오른쪽 상단의 <Icon name="dots-three-outline" /> 클릭 후 <Icon name="clock-counter-clockwise" /> **콘텐츠 이력** 선택

### 콘텐츠 이력 탐색

콘텐츠 이력을 통해 다음과 같이 콘텐츠를 탐색할 수 있습니다:

- 왼쪽의 메인 뷰: 선택한 버전의 필드와 내용을 나열합니다.
- 오른쪽의 사이드바: 사용 가능한 전체 버전 수와 각 버전에 대해
  - 버전이 생성된 날짜 및 시간
  - 버전을 생성한 사용자
  - 상태(초안, 수정됨, 발행됨) 표시([초안 & 발행](/cms/features/draft-and-publish) 참고)

<ThemedImage
alt="문서의 콘텐츠 이력 접근"
sources={{
  light:'/img/assets/content-manager/browsing-content-history.png',
  dark:'/img/assets/content-manager/browsing-content-history_DARK.png',
}}
/>

:::note
콘텐츠 이력의 메인 뷰에서는 필드가 다른 버전에서 존재하지 않거나, 삭제되었거나, 이름이 변경된 경우를 명확하게 표시합니다. 선택한 버전에서 알 수 없는 필드는 _알 수 없는 필드(Unknown fields)_ 항목 아래에 표시됩니다.
:::

### 이전 버전 복원

문서의 이전 버전을 복원할 수 있습니다. 버전을 복원하면 해당 버전의 내용이 현재 초안 버전의 내용으로 덮어써집니다. 문서는 "수정됨(Modified)" 상태로 전환되며, 이후 언제든지 발행할 수 있습니다([초안 발행](/cms/features/draft-and-publish#publishing-a-draft) 참고).

1. 콘텐츠 이력에서 오른쪽 사이드바를 통해 복원할 버전을 선택합니다.
2. **복원** 버튼을 클릭합니다.
3. _확인_ 창에서 **복원**을 클릭합니다.

:::note
콘텐츠 타입에 [국제화(i18n)](/cms/features/internationalization) 기능이 활성화되어 있고, 복원하려는 버전에 고유 필드(모든 로케일에서 동일한 값)가 있다면, 해당 필드는 모든 로케일에 대해 복원됩니다.
:::

<ThemedImage
alt="콘텐츠 이력으로 버전 복원"
sources={{
  light:'/img/assets/content-manager/restoring-content-history.png',
  dark:'/img/assets/content-manager/restoring-content-history_DARK.png',
}}
/>
