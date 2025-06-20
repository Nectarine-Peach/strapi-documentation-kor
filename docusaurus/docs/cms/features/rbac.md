---
title: 역할 기반 접근 제어 (RBAC)
description: 관리자 패널의 사용자를 관리할 수 있는 RBAC 기능 사용법을 알아보세요.
toc_max_heading_level: 5
tags:
- 관리자 패널
- RBAC
- 역할 기반 접근 제어
- 기능
---

import ScreenshotNumberReference from '/src/components/ScreenshotNumberReference.jsx';

# 역할 기반 접근 제어 (RBAC)

역할 기반 접근 제어(RBAC) 기능은 관리자 패널의 사용자인 관리자를 관리할 수 있게 해줍니다. 보다 구체적으로, RBAC는 관리자의 계정과 역할을 관리합니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="요금제">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">역할 > 설정 - 사용자 및 역할에서 CRUD 권한</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">기본적으로 사용 가능하며 활성화됨</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<Guideflow lightId="gky9n4mtdp" darkId="zkjloxzaep"/>

## 구성

**기능 구성 경로:** <Icon name="gear-six" /> *설정 > 관리 패널 > 역할*

*역할* 인터페이스는 Strapi 애플리케이션의 관리자를 위해 생성된 모든 역할을 표시합니다.

이 인터페이스에서 다음이 가능합니다:

- 새 관리자 역할 생성 ([새 역할 생성](#creating-a-new-role) 참조),
- 관리자 역할 삭제 ([역할 삭제](#deleting-a-role) 참조),
- 또는 관리자 역할에 대한 정보에 접근하고 편집 ([역할 편집](#editing-a-role) 참조).

기본적으로 모든 Strapi 애플리케이션에는 3개의 관리자 역할이 정의되어 있습니다:

- 작성자: 자신의 콘텐츠를 생성하고 관리할 수 있습니다.
- 편집자: 콘텐츠를 생성하고, 모든 콘텐츠를 관리하고 게시할 수 있습니다.
- 최고 관리자: 모든 기능과 설정에 접근할 수 있습니다. 이는 Strapi 애플리케이션 생성 시 첫 번째 관리자에게 기본적으로 부여되는 역할입니다.

### 새 역할 생성

*관리 패널 > 역할* 인터페이스의 오른쪽 상단에 **새 역할 추가** 버튼이 표시됩니다. 해당 **새 역할 추가** 버튼을 클릭하여 Strapi 애플리케이션의 관리자를 위한 새 역할을 생성하세요.

역할의 세부 정보를 편집하고 권한을 구성할 수 있는 역할 편집 인터페이스로 리디렉션됩니다 ([역할 편집](#editing-roles-details) 참조).

<ThemedImage
  alt="RBAC를 사용한 새 역할"
  sources={{
    light: '/img/assets/users-permissions/new-role.png',
    dark: '/img/assets/users-permissions/new-role_DARK.png',
  }}
/>

:::tip
*역할* 인터페이스에서 테이블의 복사 버튼 <Icon name="copy" />을 클릭하여 기존 역할을 복제해서 새 역할을 생성할 수 있습니다.
:::

### 역할 삭제

관리자 역할은 *관리 패널 > 역할* 인터페이스에서 삭제할 수 있습니다. 하지만 Strapi 애플리케이션의 어떤 관리자에게도 더 이상 부여되지 않은 경우에만 삭제할 수 있습니다.

1. 삭제하려는 역할이 더 이상 어떤 관리자에게도 부여되지 않았는지 확인하세요.
2. 역할 레코드 오른쪽의 삭제 버튼 <Icon name="trash" />을 클릭하세요.
3. 삭제 창에서 **확인** 버튼을 클릭하여 삭제를 확인하세요.

### 역할 편집

<ThemedImage
  alt="관리자 역할 편집 인터페이스"
  sources={{
    light: '/img/assets/users-permissions/administrator_roles-edition.png',
    dark: '/img/assets/users-permissions/administrator_roles-edition_DARK.png',
  }}
/>

역할 편집 인터페이스에서는 관리자 역할의 세부 정보를 편집하고 Strapi 애플리케이션의 모든 섹션에 대한 권한을 자세히 구성할 수 있습니다.

*관리 패널 > 역할*에서 역할 레코드 오른쪽의 편집 버튼 <Icon name="pencil-simple" />을 클릭하거나, **새 역할 추가** 버튼을 클릭한 후([새 역할 생성](#creating-a-new-role) 참조) 접근할 수 있습니다.

:::caution
최고 관리자 역할의 권한은 편집할 수 없습니다. 모든 구성이 읽기 전용 모드입니다.
:::

#### 역할 세부 정보 편집

관리자 역할 편집 인터페이스의 세부 정보 영역에서는 역할의 이름을 정의하고, 다른 관리자가 역할이 어떤 접근 권한을 제공하는지 이해하는 데 도움이 되는 설명을 제공할 수 있습니다.

| 역할 세부 정보  | 지침   |
| ------------- | -------------- |
| 이름 | 텍스트박스에 역할의 새 이름을 입력하세요. |
| 설명 | 텍스트박스에 역할에 대한 설명을 입력하세요. |

:::tip
오른쪽 상단 모서리에서 역할에 부여된 관리자 수를 나타내는 카운터를 볼 수 있습니다.
:::

#### 역할 권한 구성

관리자 역할 편집 인터페이스의 권한 영역에서는 관리자가 Strapi 애플리케이션의 어떤 부분에 대해 어떤 작업을 수행할 수 있는지 자세히 구성할 수 있습니다.

이는 4개 카테고리로 나뉜 테이블로 표시됩니다: [컬렉션 타입](#collection-and-single-types), [단일 타입](#collection-and-single-types), [플러그인](#plugins-and-settings) 및 [설정](#plugins-and-settings).

##### 컬렉션 및 단일 타입

컬렉션 타입 및 단일 타입 카테고리는 각각 Strapi 애플리케이션에서 사용 가능한 모든 컬렉션 및 단일 타입을 나열합니다.

각 콘텐츠 타입에 대해 관리자는 다음 작업을 수행할 권한을 가질 수 있습니다: 생성, 읽기, 업데이트, 삭제 및 게시.

1. 권한 테이블의 컬렉션 타입 또는 단일 타입 카테고리로 이동하세요.
2. 접근 권한을 부여할 콘텐츠 타입 이름 왼쪽의 박스를 체크하세요. 기본적으로 콘텐츠 타입의 모든 필드에 대해 모든 작업이 수행될 수 있습니다.
3. (선택사항) 원하는 작업을 방지하려면 작업 관련 박스의 체크를 해제하세요.
4. (선택사항) 콘텐츠 타입 이름을 클릭하여 전체 필드 목록을 표시하세요. 선택한 필드에 대한 접근 및/또는 작업을 방지하려면 필드 및 작업 관련 박스의 체크를 해제하세요. [국제화 기능](/cms/features/internationalization)이 설치되어 있다면, 각 사용 가능한 로케일에 대해 부여해야 할 권한도 정의하세요.
5. 역할이 접근 권한을 부여해야 하는 사용 가능한 각 콘텐츠 타입에 대해 2~4단계를 반복하세요.
6. 오른쪽 상단 모서리의 **저장** 버튼을 클릭하세요.

##### 플러그인 및 설정

플러그인 및 설정 카테고리는 모두 Strapi 애플리케이션의 사용 가능한 플러그인 또는 설정별로 하위 카테고리를 표시합니다. 각 하위 카테고리는 고유한 특정 권한 세트를 포함합니다.

1. 권한 테이블의 플러그인 또는 설정 카테고리로 이동하세요.
2. 구성할 권한의 하위 카테고리 이름을 클릭하여 사용 가능한 모든 권한을 표시하세요.
3. 역할이 접근 권한을 부여해야 하는 권한의 박스를 체크하세요. 자세한 정보와 지침은 아래 표를 참조하세요.

<Tabs>

<TabItem value="plugins" label="플러그인">

기본적으로, 패키지 권한은 [콘텐츠 타입 빌더](/cms/features/content-type-builder), [업로드 (즉, 미디어 라이브러리)](/cms/features/media-library), [콘텐츠 관리자](/cms/features/content-manager), 및 [사용자 및 권한](/cms/features/users-permissions) (즉, 최종 사용자를 관리할 수 있는 사용자 및 권한 기능)에 대해 구성할 수 있습니다. 각 패키지는 고유한 특정 권한 세트를 가집니다.

| 패키지 이름          | 권한 |
| -------------------- | ----------- |
| 콘텐츠 릴리즈 <br /> *(릴리즈)* | <ul><li>일반</li><ul><li>"읽기" - 릴리즈 기능에 대한 접근 권한을 부여</li><li>"생성" - 릴리즈 생성 허용</li><li>"편집" - 릴리즈 편집 허용</li><li>"삭제" - 릴리즈 삭제 허용</li><li>"게시" - 릴리즈 게시 허용</li><li>"릴리즈에서 항목 제거"</li><li>"릴리즈에 항목 추가"</li></ul></ul> |
| 콘텐츠 관리자 | <ul><li>단일 타입</li><ul><li>"보기 구성" - 단일 타입의 편집 보기 구성 허용</li></ul></ul><ul><li>컬렉션 타입</li><ul><li>"보기 구성" - 컬렉션 타입의 편집 보기 구성 허용</li></ul></ul><ul><li>컴포넌트</li><ul><li>"레이아웃 구성" - 컴포넌트의 레이아웃 구성 허용</li></ul></ul> |
| 콘텐츠 타입 빌더 | <ul><li>일반</li><ul><li>"읽기" - 콘텐츠 타입 빌더 플러그인에 읽기 전용 모드로 접근 권한 부여</li></ul></ul> |
| 업로드 <br /> *(미디어 라이브러리)* | <ul><li>일반</li><ul><li>"미디어 라이브러리 접근" - 미디어 라이브러리 플러그인에 대한 접근 권한 부여</li><li>"보기 구성" - 미디어 라이브러리 보기 구성 허용</li></ul></ul> <ul><li>자산</li><ul><li>"생성 (업로드)" - 미디어 파일 업로드 허용</li> <li>"업데이트 (자르기, 세부사항, 교체) + 삭제" - 업로드된 미디어 파일 편집 허용</li><li>"다운로드" - 업로드된 미디어 파일 다운로드 허용</li><li>"링크 복사" - 업로드된 미디어 파일의 링크 복사 허용</li></ul></ul> |
| 사용자 권한 | <ul><li>역할</li><ul><li>"생성" - 최종 사용자 역할 생성 허용</li><li>"읽기" - 생성된 최종 사용자 역할 보기 허용</li><li>"업데이트" - 최종 사용자 역할 편집 허용</li><li>"삭제" - 최종 사용자 역할 삭제 허용</li></ul></ul><ul><li>제공자</li><ul><li>"읽기" - 제공자 보기 허용</li><li>"편집" - 제공자 편집 허용</li></ul></ul><ul><li>이메일 템플릿</li><ul><li>"읽기" - 이메일 템플릿 접근 허용</li><li>"편집" - 이메일 템플릿 편집 허용</li></ul></ul><ul><li>고급 설정</li><ul><li>"읽기" - 사용자 및 권한 플러그인의 고급 설정 접근 허용</li><li>"편집" - 고급 설정 편집 허용</li></ul></ul> 👉 사용자 및 권한 플러그인 경로 알림: <br />*일반 > 설정 > 사용자 및 권한 플러그인* |

</TabItem>

<TabItem value="settings" label="설정">

설정 권한은 관리자 패널의 주 네비게이션에서 *일반 > 설정*으로 접근 가능한 모든 설정에 대해 구성할 수 있습니다. 또한 관리자 패널의 플러그인 및 마켓플레이스 섹션에 대한 접근을 구성할 수 있습니다. 각 설정은 고유한 특정 권한 세트를 가집니다.

| 설정 이름            | 권한 |
| ----------------------- | ----------- |
| 콘텐츠 릴리즈 | <ul><li>옵션</li><ul><li>"읽기" - 릴리즈 설정 접근 허용</li><li>"편집" - 릴리즈 설정 편집 허용</li></ul></ul> 👉 릴리즈 설정 경로 알림: <br />*일반 > 설정 > 전역 설정 - 릴리즈* |
| 이메일 | <ul><li>일반</li><ul><li>"이메일 설정 페이지 접근" - 이메일 설정에 대한 접근 권한 부여</li></ul></ul> 👉 이메일 설정 경로 알림: <br /> *일반 > 설정 > 사용자 및 권한 플러그인 - 이메일 템플릿* |
| 미디어 라이브러리 | <ul><li>일반</li><ul><li>"미디어 라이브러리 설정 페이지 접근" - 미디어 라이브러리 설정에 대한 접근 권한 부여</li></ul></ul> 👉 미디어 라이브러리 설정 경로 알림: <br /> *일반 > 설정 > 전역 설정 - 미디어 라이브러리* |
| 국제화 | <ul><li>로케일</li><ul><li>"생성" - 새 로케일 생성 허용</li><li>"읽기" - 사용 가능한 로케일 보기 허용</li><li>"업데이트" - 사용 가능한 로케일 편집 허용</li><li>"삭제" - 로케일 삭제 허용</li></ul></ul> 👉 국제화 설정 경로 알림: <br /> *일반 > 설정 > 전역 설정 - 국제화* |
| 검토 워크플로 <EnterpriseBadge /> | <ul><li>"생성" - 워크플로 생성 허용</li><li>"읽기" - 생성된 워크플로 보기 허용</li><li>"업데이트" - 워크플로 편집 허용</li><li>"삭제" - 워크플로 삭제 허용</li></ul> 👉 검토 워크플로 설정 경로 알림: <br /> *일반 > 설정 > 전역 설정 - 검토 워크플로* |
| 싱글 사인온 <EnterpriseBadge /> <SsoBadge /> | <ul><li>옵션</li><ul><li>"읽기" - SSO 설정 접근 허용</li><li>"업데이트" - SSO 설정 편집 허용</li></ul></ul> 👉 SSO 설정 경로 알림: <br />*일반 > 설정 > 전역 설정 - 싱글 사인온* |
| 감사 로그 | <ul><li>옵션</li><ul><li>"읽기" - 감사 로그 설정 접근 허용</li></ul></ul> 👉 감사 로그 설정 경로 알림: <br />*일반 > 설정 > 관리 패널 - 감사 로그* |
| 플러그인 및 마켓플레이스 | <ul><li>마켓플레이스</li><ul><li>"마켓플레이스 접근" - 마켓플레이스에 대한 접근 권한 부여</li></ul></ul> |
| 웹훅 | <ul><li>일반</li><ul><li>"생성" - 웹훅 생성 허용</li><li>"읽기" - 생성된 웹훅 보기 허용</li><li>"업데이트" - 웹훅 편집 허용</li><li>"삭제" - 웹훅 삭제 허용</li></ul></ul> 👉 웹훅 설정 경로 알림: <br /> *일반 > 설정 > 전역 설정 - 웹훅* |
| 사용자 및 역할 | <ul><li>사용자</li><ul><li>"생성 (초대)" - 관리자 계정 생성 허용</li><li>"읽기" - 기존 관리자 계정 보기 허용</li><li>"업데이트" - 관리자 계정 편집 허용</li><li>"삭제" - 관리자 계정 삭제 허용</li></ul></ul><ul><li>역할</li><ul><li>"생성" - 관리자 역할 생성 허용</li><li>"읽기" - 생성된 관리자 역할 보기 허용</li><li>"업데이트" - 관리자 역할 편집 허용</li><li>"삭제" - 관리자 역할 삭제 허용</li></ul></ul> 👉 RBAC 기능 경로 알림: <br /> *일반 > 설정 > 관리 패널* |
| API 토큰 |  <ul><li>API 토큰</li><ul><li>"API 토큰 설정 페이지 접근" - API 토큰 페이지 접근을 전환</li></ul></ul><ul><li>일반</li><ul><li>"생성 (생성)" - API 토큰 생성 허용</li><li>"읽기" - 생성된 API 토큰 보기 허용 (이 권한을 비활성화하면 *전역 설정 - API 토큰* 설정에 대한 접근이 비활성화됩니다)</li><li>"업데이트" - API 토큰 편집 허용</li><li>"삭제 (폐기)" - API 토큰 삭제 허용</li> <li> "재생성" - API 토큰 재생성 허용</li></ul></ul> 👉 API 토큰 설정 경로 알림: <br /> *일반 > 설정 > 전역 설정 - API 토큰* |
| 프로젝트 | <ul><li>일반</li><ul><li>"프로젝트 수준 설정 업데이트" - 프로젝트 설정 편집 허용</li><li>"프로젝트 수준 설정 읽기" - 프로젝트 설정에 대한 접근 권한 부여</li></ul></ul> |
| 전송 토큰 | <ul><li>전송 토큰</li><ul><li>"전송 토큰 설정 페이지 접근" - 전송 토큰 페이지 접근을 전환</li></ul></ul><ul><li>일반</li><ul><li>"생성 (생성)" - 전송 토큰 생성 허용</li><li>"읽기" - 생성된 전송 토큰 보기 허용 (이 권한을 비활성화하면 *전역 설정 - 전송 토큰* 설정에 대한 접근이 비활성화됩니다)</li><li>"업데이트" - 전송 토큰 편집 허용</li><li>"삭제 (폐기)" - 전송 토큰 삭제 허용</li> <li> "재생성" - 전송 토큰 재생성 허용</li></ul></ul> 👉 전송 토큰 설정 경로 알림: <br /> *일반 > 설정 > 전역 설정 - 전송 토큰* |

</TabItem>

</Tabs>

4. 오른쪽 상단 모서리의 **저장** 버튼을 클릭하세요.

#### 권한에 대한 사용자 정의 조건 설정

각 카테고리의 각 권한에 대해 <Icon name="gear-six" /> **설정** 버튼이 표시됩니다. 이를 통해 관리자가 권한을 부여받기 위한 추가 조건을 정의하여 권한 구성을 더욱 세밀하게 할 수 있습니다.

2개의 기본 추가 조건이 있습니다:
- 관리자가 생성자여야 함,
- 관리자가 생성자와 같은 역할을 가져야 함.

<ThemedImage
  alt="사용자 정의 조건"
  sources={{
    light: '/img/assets/users-permissions/administrator_custom-conditions.png',
    dark: '/img/assets/users-permissions/administrator_custom-conditions_DARK.png',
  }}
/>

1. 역할에 이미 부여된 권한의 <Icon name="gear-six" /> **설정** 버튼을 클릭하세요.
2. *조건 정의* 창에서 사용 가능한 각 권한을 특정 조건으로 사용자 정의할 수 있습니다. 사용자 정의하려는 권한과 관련된 드롭다운 목록을 클릭하세요.
3. 선택한 권한에 대한 사용자 정의 조건을 정의하세요. 다음 중 하나를 선택할 수 있습니다:
   - 사용 가능한 모든 추가 조건이 적용되도록 기본 옵션을 체크하세요.
   - 화살표 버튼 <Icon name="caret-down" />를 클릭하여 사용 가능한 추가 조건을 보고 선택한 조건만 체크하세요.
4. **적용** 버튼을 클릭하세요.

:::tip
권한에 대해 사용자 정의 조건이 설정되면, 권한 이름 옆과 <Icon name="gear-six" /> **설정** 버튼 옆에 점이 표시됩니다.
:::

:::caution
사용자 정의 조건은 역할에 부여되도록 체크된 권한에 대해서만 설정할 수 있습니다. 그렇지 않으면 <Icon name="gear-six" /> **설정** 버튼을 클릭할 때 열리는 창이 비어 있게 되며, 사용자 정의 조건 옵션을 사용할 수 없습니다.
:::

Strapi 애플리케이션에 대해 미리 생성된 다른 사용자 정의 조건을 사용할 수 있습니다. 다음 전용 가이드는 추가 사용자 정의 조건을 생성하는 데 도움이 됩니다:

<CustomDocCardsWrapper>
<CustomDocCard icon="" title="사용자 정의 RBAC 조건 생성" description="Strapi 애플리케이션의 코드를 사용자 정의하여 처음부터 사용자 정의 RBAC 조건을 생성하는 방법을 알아보세요." link="/cms/configurations/guides/rbac" />
</CustomDocCardsWrapper>

## 사용법

**기능 사용 경로:** <Icon name="gear-six" /> *설정 > 관리 패널 > 사용자*

*사용자* 인터페이스는 Strapi 애플리케이션의 모든 관리자를 나열하는 테이블을 표시합니다. 보다 구체적으로, 테이블에 나열된 각 관리자에 대해 이름, 이메일 및 부여된 역할을 포함한 주요 계정 정보가 표시됩니다. 계정의 상태도 표시됩니다: 관리자가 이미 로그인하여 계정을 활성화했는지 여부에 따라 활성 또는 비활성.

<ThemedImage
  alt="사용자 인터페이스"
  sources={{
    light: '/img/assets/users-permissions/usage-interface.png',
    dark: '/img/assets/users-permissions/usage-interface_DARK.png',
  }}
/>

이 인터페이스에서 다음이 가능합니다:

- 특정 관리자를 찾기 위한 텍스트 검색 <ScreenshotNumberReference number="1" />,
- 특정 관리자를 찾기 위한 필터 설정 <ScreenshotNumberReference number="2" />,
- 새 관리자 계정 생성 ([새 계정 생성](#creating-a-new-account) 참조) <ScreenshotNumberReference number="3" />,
- 관리자 계정 삭제 <ScreenshotNumberReference number="4" /> ([계정 삭제](#deleting-an-account) 참조),
- 또는 관리자 계정에 대한 정보에 접근하고 편집 <ScreenshotNumberReference number="5" /> ([계정 편집](#editing-an-account) 참조).


:::tip
테이블에 표시된 대부분의 필드에 대해 정렬을 활성화할 수 있습니다. 테이블 헤더의 필드 이름을 클릭하여 해당 필드로 정렬하세요.
:::

### 새 계정 생성

<ThemedImage
  alt="사용자 초대"
  sources={{
    light: '/img/assets/users-permissions/invite-new-user.png',
    dark: '/img/assets/users-permissions/invite-new-user_DARK.png',
  }}
/>

1. <Icon name="envelope" /> **새 사용자 초대** 버튼을 클릭하세요.
2. *새 사용자 초대* 창에서 새 관리자에 대한 세부사항 정보를 입력하세요:

  | 사용자 정보 | 지침                                                                 |
  | ---------------- | ---------------------------------------------------------------------------- |
  | 이름       | (필수) 텍스트박스에 관리자의 이름을 입력하세요.             |
  | 성        | (필수) 텍스트박스에 관리자의 성을 입력하세요.              |
  | 이메일            | (필수) 텍스트박스에 관리자의 완전한 이메일 주소를 입력하세요. |

3. 새 관리자에 대한 로그인 설정을 입력하세요:

  | 설정          | 지침                                                                                                    |
  | ---------------- | --------------------------------------------------------------------------------------------------------------- |
  | 사용자 역할     | (필수) 드롭다운 목록에서 새 관리자에게 부여할 역할을 선택하세요.                      |
  | SSO로 연결 | (선택사항) 새 관리자 계정을 SSO와 연결하려면 **TRUE** 또는 **FALSE**를 클릭하세요.                       |

4. *새 사용자 추가* 창의 오른쪽 하단 모서리에 있는 **사용자 초대** 버튼을 클릭하세요.
5. 창 상단에 URL이 나타납니다: 이는 새 관리자가 Strapi 애플리케이션에 처음 로그인하기 위해 보내야 할 URL입니다. 복사 버튼 <Icon name="copy" />를 클릭하여 URL을 복사하세요.
6. 오른쪽 하단 모서리의 **완료** 버튼을 클릭하여 새 관리자 계정 생성을 완료하세요. 새 관리자가 이제 테이블에 나열되어야 합니다.

:::note
관리자 초대 URL은 활성화될 때까지 관리자 계정에서 접근할 수 있습니다.
:::

### 계정 삭제

하나 또는 여러 관리자 계정을 동시에 삭제할 수 있습니다.

1. 계정 레코드 오른쪽의 삭제 버튼 <Icon name="trash" />을 클릭하거나, 계정 레코드 왼쪽의 박스를 체크하여 하나 이상의 계정을 선택한 다음 테이블 위의 <Icon name="trash" /> **삭제** 버튼을 클릭하세요.
2. 삭제 창에서 **확인** 버튼을 클릭하여 삭제를 확인하세요.

### 계정 편집

<ThemedImage
  alt="관리자 계정 편집"
  sources={{
    light: '/img/assets/users-permissions/administrator_edit-info.png',
    dark: '/img/assets/users-permissions/administrator_edit-info_DARK.png',
  }}
/>

1. 편집하려는 관리자의 이름을 클릭하세요.
2. *세부사항* 영역에서 선택한 계정 세부사항을 편집하세요:

| 사용자 정보      | 지침  |
| --------------------- | ----------------------- |
| 이름            | 텍스트박스에 관리자의 이름을 입력하세요.                                        |
| 성             | 텍스트박스에 관리자의 성을 입력하세요.                                         |
| 이메일                 | 텍스트박스에 관리자의 완전한 이메일 주소를 입력하세요.                            |
| 사용자명              | 텍스트박스에 관리자의 사용자명을 입력하세요.                                          |
| 비밀번호              | 텍스트박스에 새 관리자 계정의 비밀번호를 입력하세요.                              |
| 비밀번호 확인      | 확인을 위해 텍스트박스에 새 비밀번호를 입력하세요.                                     |
| 활성                | 관리자 계정을 활성화하려면 **TRUE**를 클릭하세요.                                  |

3. (선택사항) *역할* 영역에서 관리자의 역할을 편집하세요:
  - 드롭다운 목록을 클릭하여 새 역할을 선택하고/또는 이미 부여된 역할에 추가하세요.
  - 이미 부여된 역할을 삭제하려면 삭제 버튼 <Icon name="x" classes="ph-bold" />을 클릭하세요.
4. 오른쪽 상단 모서리의 **저장** 버튼을 클릭하세요.
