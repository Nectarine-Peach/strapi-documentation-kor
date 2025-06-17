---
title: 콘텐츠 타입 빌더
description: 콘텐츠 타입 빌더 사용법을 알아보세요.
toc_max_heading_level: 5
tags:
- 관리자 패널
- 콘텐츠 타입 빌더
- 콘텐츠 타입
- 컴포넌트
- 동적 영역
- 커스텀 필드
---

import ScreenshotNumberReference from '/src/components/ScreenshotNumberReference.jsx';

# 콘텐츠 타입 빌더

관리자 패널의 메인 네비게이션을 통해 접근할 수 있는 <Icon name="layout" /> 콘텐츠 타입 빌더에서 사용자는 콘텐츠 타입을 생성하고 편집할 수 있습니다.

<IdentityCard>
  <IdentityCardItem icon="user" title="역할 및 권한">역할 > 플러그인 - 콘텐츠 타입 빌더에서 최소 "읽기" 권한 필요.</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 환경에서만 사용 가능.</IdentityCardItem>
</IdentityCard>

## 개요

<Guideflow lightId="lpnm3w1ber" darkId="zklmd4gijr" />

<Icon name="layout" /> 콘텐츠 타입 빌더를 통해 다음과 같은 콘텐츠 타입을 생성하고 관리할 수 있습니다:

- 컬렉션 타입: 여러 엔트리를 관리할 수 있는 콘텐츠 타입.
- 싱글 타입: 하나의 엔트리만 관리할 수 있는 콘텐츠 타입.
- 컴포넌트: 여러 컬렉션 타입과 싱글 타입에서 사용할 수 있는 콘텐츠 구조. 기술적으로는 독립적으로 존재할 수 없기 때문에 진정한 콘텐츠 타입은 아니지만, 컴포넌트도 컬렉션 타입과 싱글 타입과 동일한 방식으로 콘텐츠 타입 빌더에서 생성하고 관리됩니다.

이 3가지 모두 <Icon name="layout" /> 콘텐츠 타입 빌더의 서브 네비게이션에서 카테고리로 표시됩니다. 각 카테고리에는 이미 생성된 모든 콘텐츠 타입과 컴포넌트가 나열됩니다.

:::tip
<Icon name="layout" /> 콘텐츠 타입 빌더 서브 네비게이션에서 검색 아이콘 <Icon name="magnifying-glass" classes="ph-bold" />을 클릭하여 특정 컬렉션 타입, 싱글 타입 또는 컴포넌트를 찾을 수 있습니다.
:::

콘텐츠 타입 빌더의 서브 네비게이션에는 모든 콘텐츠 타입과 컴포넌트에 적용되는 중앙 집중식 **저장** 버튼도 표시됩니다. 콘텐츠 타입/컴포넌트와 필드 모두의 상태 표시와 함께, 이를 통해 여러 콘텐츠 타입과 컴포넌트를 동시에 작업할 수 있습니다. 다음 상태가 표시될 수 있습니다:

- `New` 또는 `N`은 콘텐츠 타입/컴포넌트 또는 필드가 새로 만들어져 아직 저장되지 않았음을 나타냅니다.
- `Modified` 또는 `M`은 콘텐츠 타입/컴포넌트 또는 필드가 마지막 저장 이후 수정되었음을 나타냅니다.
- `Deleted` 또는 `D`는 콘텐츠 타입/컴포넌트 또는 필드가 삭제되었지만 저장해야만 확정됨을 나타냅니다.

:::note
**저장** 옆의 **...** 버튼을 클릭하면 **마지막 변경 실행 취소/다시 실행** 및 **모든 변경 사항 삭제**와 같은 다른 옵션에 접근할 수 있습니다. 이러한 옵션들도 중앙 집중식으로, 마지막 저장 이후 모든 콘텐츠 타입, 컴포넌트 및 필드에서 수행된 마지막 액션(들)에 적용됩니다.
:::

## 사용법

<br/>

### 콘텐츠 타입 생성

콘텐츠 타입 빌더를 통해 싱글 타입과 컬렉션 타입뿐만 아니라 컴포넌트까지 새로운 콘텐츠 타입을 생성할 수 있습니다.

#### 새 콘텐츠 타입

<ThemedImage
  alt="콘텐츠 타입 생성"
  sources={{
    light: '/img/assets/content-type-builder/content-type-creation.png',
    dark: '/img/assets/content-type-builder/content-type-creation_DARK.png',
  }}
/>

1. 컬렉션 타입 또는 싱글 타입 중 원하는 유형을 선택합니다.
2. <Icon name="layout" /> 콘텐츠 타입 빌더의 해당 카테고리에서 **새 컬렉션/싱글 타입 생성**을 클릭합니다.
3. 생성 창에서 *표시 이름* 텍스트박스에 새 콘텐츠 타입의 이름을 입력합니다.
4. *API ID*를 확인하여 자동으로 미리 채워진 값이 올바른지 확인합니다. 컬렉션 타입 이름은 콘텐츠 매니저에 표시될 때 자동으로 복수형으로 표시됩니다. 단수 이름을 선택하는 것이 권장되지만, *API ID* 필드를 통해 복수형 오류를 수정할 수 있습니다.
5. (선택사항) 고급 설정 탭에서 새 콘텐츠 타입에 사용 가능한 설정을 구성합니다:
      | 설정 이름    | 지침                                                                                                                                     |
      |-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
      | 초안 및 발행 | 콘텐츠 타입의 엔트리가 발행되기 전에 초안 버전으로 관리될 수 있도록 체크박스를 선택합니다([초안 및 발행](/cms/features/draft-and-publish) 참고). |
      | 국제화 | 콘텐츠 타입의 엔트리가 다른 로케일로 번역될 수 있도록 체크박스를 선택합니다. |
6. **계속** 버튼을 클릭합니다.
7. 콘텐츠 타입에 선택한 필드를 추가하고 구성합니다([콘텐츠 타입 필드 구성](#configuring-fields-content-type) 참고).
8. **저장** 버튼을 클릭합니다.

:::caution
새 콘텐츠 타입은 저장되어야만 생성된 것으로 간주됩니다. 저장은 최소 하나의 필드가 추가되고 올바르게 구성된 경우에만 가능합니다. 이러한 단계가 완료되지 않으면 콘텐츠 타입을 생성할 수 없고, 콘텐츠 타입 빌더의 해당 카테고리에 나열되지 않으며, [콘텐츠 매니저](/cms/features/content-manager)에서 사용할 수 없습니다.
:::

#### 새 컴포넌트

<ThemedImage
  alt="컴포넌트 생성"
  sources={{
    light: '/img/assets/content-type-builder/component-creation-1.png',
    dark: '/img/assets/content-type-builder/component-creation-1_DARK.png',
  }}
/>

1. <Icon name="layout" /> 콘텐츠 타입 빌더 서브 네비게이션의 컴포넌트 카테고리에서 **새 컴포넌트 생성**을 클릭합니다.
2. 컴포넌트 생성 창에서 새 컴포넌트의 기본 설정을 구성합니다:
   - *표시 이름* 텍스트박스에 컴포넌트의 이름을 입력합니다.
   - 사용 가능한 카테고리를 선택하거나, 텍스트박스에 새 카테고리 이름을 입력하여 생성합니다.
   - _(선택사항)_ 새 컴포넌트를 나타내는 아이콘을 선택합니다. 목록을 스크롤하는 대신 검색 <Icon name="magnifying-glass" classes="ph-bold" />을 사용하여 아이콘을 찾을 수 있습니다.
3. **계속** 버튼을 클릭합니다.
4. 컴포넌트에 선택한 필드를 추가하고 구성합니다([콘텐츠 타입 필드 구성](#configuring-fields-content-type) 참고).
5. **저장** 버튼을 클릭합니다.

### 콘텐츠 타입 편집

콘텐츠 타입 빌더를 통해 기존의 모든 콘텐츠 타입을 관리할 수 있습니다. 편집할 콘텐츠 타입이나 컴포넌트를 선택하면, 콘텐츠 타입 빌더 인터페이스의 오른쪽에 사용 가능한 모든 편집 및 관리 옵션이 표시됩니다.

<ThemedImage
  alt="콘텐츠 타입 빌더의 편집 인터페이스"
  sources={{
    light: '/img/assets/content-type-builder/new_CTB.png',
    dark: '/img/assets/content-type-builder/new_CTB_DARK.png',
  }}
/>

#### 설정

1. 콘텐츠 타입의 <Icon name="pencil-simple" /> **편집** 버튼을 클릭하여 설정에 접근합니다.
2. 원하는 사용 가능한 설정을 편집합니다:

  <Tabs groupId="CTSettings">

  <TabItem value="CTBasicSettings" label="기본 설정">

  <ThemedImage
    alt="콘텐츠 타입 빌더의 기본 설정"
    sources={{
      light: '/img/assets/content-type-builder/basic-settings.png',
      dark: '/img/assets/content-type-builder/basic-settings_DARK.png',
    }}
  />

  * **표시 이름**: 관리자 패널에 표시될 콘텐츠 타입 또는 컴포넌트의 이름.
  * **API ID (단수)**: API에서 사용될 콘텐츠 타입 또는 컴포넌트의 이름. 표시 이름에서 자동으로 생성되지만 편집할 수 있습니다.
  * **API ID (복수)**: API에서 사용될 콘텐츠 타입 또는 컴포넌트의 복수 이름. 표시 이름에서 자동으로 생성되지만 편집할 수 있습니다.
  * **타입**: 콘텐츠 타입 또는 컴포넌트의 유형. **컬렉션 타입** 또는 **싱글 타입**일 수 있습니다.

  </TabItem>

  <TabItem value="CTAdvancedSettings" label="고급 설정">

  <ThemedImage
    alt="콘텐츠 타입 빌더의 고급 설정"
    sources={{
      light: '/img/assets/content-type-builder/advanced-settings.png',
      dark: '/img/assets/content-type-builder/advanced-settings_DARK.png',
    }}
  />

  * **초안 및 발행**: 콘텐츠 타입 또는 컴포넌트에 대해 [초안 및 발행](/cms/features/draft-and-publish) 기능을 활성화합니다. 기본적으로 비활성화되어 있습니다.
  * **국제화**: 콘텐츠 타입 또는 컴포넌트에 대해 [국제화](/cms/features/internationalization) 기능을 활성화합니다. 기본적으로 비활성화되어 있습니다.

  </TabItem>

  </Tabs>
3. 대화 상자에서 **완료** 버튼을 클릭합니다.
4. 콘텐츠 타입 빌더 네비게이션에서 **저장** 버튼을 클릭합니다.

#### 필드

콘텐츠 타입의 필드를 나열하는 테이블에서 다음을 수행할 수 있습니다:
- <Icon name="pencil-simple" /> 버튼을 클릭하여 필드의 기본 및 고급 설정에 접근하여 편집
- **다른 필드 추가** 버튼을 클릭하여 선택한 콘텐츠 타입에 새 필드 생성
- <Icon name="dots-six-vertical" classes="ph-bold"/> 버튼을 클릭하고 필드를 드래그 앤 드롭하여 콘텐츠 타입의 필드 순서 변경
- <Icon name="trash" /> 버튼을 클릭하여 필드 삭제

:::caution
필드를 편집하면 이름을 바꿀 수 있습니다. 하지만 데이터베이스 관점에서 필드 이름을 바꾸는 것은 완전히 새로운 필드를 생성하고 이전 필드를 삭제하는 것임을 명심하세요. 데이터베이스에서 아무것도 삭제되지 않지만, 이전 필드 이름과 연결된 데이터는 더 이상 애플리케이션의 관리자 패널에서 접근할 수 없게 됩니다.
:::

### 콘텐츠 타입 필드 구성 {#configuring-fields-content-type}

콘텐츠 타입은 하나 또는 여러 개의 필드로 구성됩니다. 각 필드는 콘텐츠 매니저에서 입력되는 특정 종류의 데이터를 포함하도록 설계되었습니다([콘텐츠 생성 및 작성](/cms/features/content-manager#creating--writing-content) 참고).

<Icon name="layout" /> 콘텐츠 타입 빌더에서 필드는 새 콘텐츠 타입이나 컴포넌트 생성 시에 추가하거나, 나중에 콘텐츠 타입이나 컴포넌트를 편집하거나 업데이트할 때 추가할 수 있습니다.

:::note
생성하거나 편집 중인 콘텐츠 타입이나 컴포넌트에 따라 컴포넌트와 동적 영역을 포함한 모든 필드가 항상 사용 가능한 것은 아닙니다.
:::

<ThemedImage
  alt="필드 선택"
  sources={{
    light: '/img/assets/content-type-builder/fields-selection.png',
    dark: '/img/assets/content-type-builder/fields-selection_DARK.png',
  }}
/>

#### <img width="28" src="/img/assets/icons/v5/ctb_text.svg" /> 텍스트 {#text}

텍스트 필드는 작은 텍스트를 포함할 수 있는 텍스트박스를 표시합니다. 이 필드는 제목, 설명 등에 사용할 수 있습니다.

<Tabs>

<TabItem value="base" label="기본 설정">

| 설정 이름 | 지침                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------|
| 이름         | 텍스트 필드의 이름을 입력합니다.                                                                       |
| 타입         | 텍스트 필드를 채울 수 있는 공간을 늘리거나 줄이기 위해 *짧은 텍스트* (최대 255자)와 *긴 텍스트* 중에서 선택합니다.     |

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| 기본값  | 텍스트 필드의 기본값을 입력합니다.                                    |
| RegExp 패턴 | 텍스트 필드의 값이 특정 형식과 일치하는지 확인하기 위해 정규 표현식을 입력합니다. |
| 비공개 필드  | 필드를 비공개로 만들어 API를 통해 찾을 수 없도록 하려면 체크합니다.   |
| 이 필드에 대해 로컬라이제이션 활성화 | ([국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우) 필드가 로케일별로 다른 값을 가질 수 있도록 허용합니다. |
| 필수 필드 | 필드가 채워지지 않으면 엔트리 생성 또는 저장이 방지되도록 하려면 체크합니다.    |
| 고유 필드   | 다른 필드가 이 필드와 동일한 값을 갖는 것을 방지하려면 체크합니다.                    |
| 최대 길이 | 허용되는 최대 문자 수를 정의하려면 체크합니다.                        |
| 최소 길이 | 허용되는 최소 문자 수를 정의하려면 체크합니다.                        |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_richtextblocks.svg" /> 리치 텍스트 (블록) {#rich-text-blocks}

리치 텍스트 (블록) 필드는 실시간 렌더링과 리치 텍스트 관리를 위한 다양한 옵션이 있는 편집기를 표시합니다. 이 필드는 이미지와 코드를 포함한 긴 텍스트 콘텐츠에 사용할 수 있습니다.

<Tabs>

<TabItem value="base" label="기본 설정">

| 설정 이름 | 지침                                    |
|--------------|-------------------------------------------------|
| 이름         | 리치 텍스트 (블록) 필드의 이름을 입력합니다. |

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| 비공개 필드  | 필드를 비공개로 만들어 API를 통해 찾을 수 없도록 하려면 체크합니다. |
| 필수 필드 | 필드가 채워지지 않으면 엔트리 생성 또는 저장이 방지되도록 하려면 체크합니다.  |
| 이 필드에 대해 로컬라이제이션 활성화 | ([국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우) 필드가 로케일별로 다른 값을 가질 수 있도록 허용합니다. |

</TabItem>

</Tabs>

:::strapi React renderer
If using the Blocks editor, we recommend that you also use the <ExternalLink to="https://github.com/strapi/blocks-react-renderer" text="Strapi Blocks React Renderer"/> to easily render the content in a React frontend.
:::

#### <img width="28" src="/img/assets/icons/v5/ctb_number.svg" /> 숫자 {#number}

숫자 필드는 정수, 소수, 부동소수점 등 모든 종류의 숫자를 위한 필드를 표시합니다.

<Tabs>

<TabItem value="base" label="기본 설정">

| 설정 이름  | 지침                                                    |
|---------------|-----------------------------------------------------------------|
| 이름          | 숫자 필드의 이름을 입력합니다.                             |
| 숫자 형식 | *정수*, *빅 정수*, *소수* 및 *부동소수점* 중에서 선택합니다. |

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| 기본값  | 숫자 필드의 기본값을 입력합니다.                                |
| 비공개 필드  | 필드를 비공개로 만들어 API를 통해 찾을 수 없도록 하려면 체크합니다. |
| 이 필드에 대해 로컬라이제이션 활성화 | ([국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우) 필드가 로케일별로 다른 값을 가질 수 있도록 허용합니다. |
| 필수 필드 | 필드가 채워지지 않으면 엔트리 생성 또는 저장이 방지되도록 하려면 체크합니다.  |
| 고유 필드   | 다른 필드가 이 필드와 동일한 값을 갖는 것을 방지하려면 체크합니다.                  |
| 최대값  | 허용되는 최대값을 정의하려면 체크합니다.                      |
| 최소값  | 허용되는 최소값을 정의하려면 체크합니다.                      |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_date.svg" /> 날짜 {#date}

날짜 필드는 날짜(년, 월, 일), 시간(시, 분, 초) 또는 날짜시간(년, 월, 일, 시, 분, 초) 선택기를 표시할 수 있습니다.

<Tabs>

<TabItem value="base" label="기본 설정">

| 설정 이름  | 지침                                                    |
|---------------|-----------------------------------------------------------------|
| 이름          | 날짜 필드의 이름을 입력합니다.                               |
| 타입          | *날짜*, *날짜시간* 및 *시간* 중에서 선택합니다                    |

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| 기본값  | 날짜 필드의 기본값을 입력합니다.                                  |
| 비공개 필드  | 필드를 비공개로 만들어 API를 통해 찾을 수 없도록 하려면 체크합니다. |
| 이 필드에 대해 로컬라이제이션 활성화 | ([국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우) 필드가 로케일별로 다른 값을 가질 수 있도록 허용합니다. |
| 필수 필드 | 필드가 채워지지 않으면 엔트리 생성 또는 저장이 방지되도록 하려면 체크합니다.  |
| 고유 필드   | 다른 필드가 이 필드와 동일한 값을 갖는 것을 방지하려면 체크합니다.                  |

</TabItem>

</Tabs>
 
#### <img width="28" src="/img/assets/icons/v5/ctb_password.svg" /> 비밀번호

비밀번호 필드는 암호화된 비밀번호 필드를 표시합니다.

<Tabs>

<TabItem value="base" label="기본 설정">

| 설정 이름  | 지침                                                    |
|---------------|-----------------------------------------------------------------|
| 이름          | 비밀번호 필드의 이름을 입력합니다.                           |

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| 기본값  | 비밀번호 필드의 기본값을 입력합니다.                              |
| 비공개 필드  | 필드를 비공개로 만들어 API를 통해 찾을 수 없도록 하려면 체크합니다. |
| 이 필드에 대해 로컬라이제이션 활성화 | ([국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우) 필드가 로케일별로 다른 값을 가질 수 있도록 허용합니다. |
| 필수 필드 | 필드가 채워지지 않으면 엔트리 생성 또는 저장이 방지되도록 하려면 체크합니다.  |
| 최대 길이 | 허용되는 최대 문자 수를 정의하려면 체크합니다.                      |
| 최소 길이 | 허용되는 최소 문자 수를 정의하려면 체크합니다.                      |

</TabItem>

</Tabs>


#### <img width="28" src="/img/assets/icons/v5/ctb_media.svg" /> 미디어 {#media}

미디어 필드를 사용하면 애플리케이션의 미디어 라이브러리에 업로드된 미디어 파일(예: 이미지, 비디오) 중에서 하나 또는 여러 개를 선택할 수 있습니다.

<Tabs>

<TabItem value="base" label="기본 설정">

| 설정 이름  | 지침                                                    |
|---------------|-----------------------------------------------------------------|
| 이름          | 미디어 필드의 이름을 입력합니다.                              |
| 타입          | 여러 미디어 업로드를 허용하는 *다중 미디어*와 하나의 미디어 업로드만 허용하는 *단일 미디어* 중에서 선택합니다. |

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| 허용되는 미디어 타입 선택  | 드롭다운 목록을 클릭하여 이 필드에서 허용되지 않는 미디어 타입의 체크를 해제합니다. |
| 비공개 필드  | 필드를 비공개로 만들어 API를 통해 찾을 수 없도록 하려면 체크합니다. |
| 이 필드에 대해 로컬라이제이션 활성화 | ([국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우) 필드가 로케일별로 다른 값을 가질 수 있도록 허용합니다. |
| 필수 필드 | 필드가 채워지지 않으면 엔트리 생성 또는 저장이 방지되도록 하려면 체크합니다.  |
| 고유 필드   | 다른 필드가 이 필드와 동일한 값을 갖는 것을 방지하려면 체크합니다.                  |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_relation.svg" /> 관계 {#relation}

관계 필드는 컬렉션 타입이어야 하는 다른 콘텐츠 타입과의 관계를 설정할 수 있습니다.

6가지 다른 관계 타입이 있습니다:

- <img width="25" src="/img/assets/icons/v5/ctb_relation_oneway.svg" /> 단방향: 콘텐츠 타입 A가 콘텐츠 타입 B를 *하나 가짐*
- <img width="25" src="/img/assets/icons/v5/ctb_relation_1to1.svg" /> 일대일: 콘텐츠 타입 A가 콘텐츠 타입 B를 *하나 가지고 속함*
- <img width="25" src="/img/assets/icons/v5/ctb_relation_1tomany.svg" /> 일대다: 콘텐츠 타입 A가 콘텐츠 타입 B에 *여러 개 속함*
- <img width="25" src="/img/assets/icons/v5/ctb_relation_manyto1.svg" /> 다대일: 콘텐츠 타입 B가 콘텐츠 타입 A를 *여러 개 가짐*
- <img width="25" src="/img/assets/icons/v5/ctb_relation_manytomany.svg" /> 다대다: 콘텐츠 타입 A가 콘텐츠 타입 B를 *여러 개 가지고 속함*
- <img width="25" src="/img/assets/icons/v5/ctb_relation_manyway.svg" /> 다방향: 콘텐츠 타입 A가 콘텐츠 타입 B를 *여러 개 가짐*

<Tabs>

<TabItem value="base" label="기본 설정">

관계 필드의 기본 설정 구성은 어떤 기존 콘텐츠 타입과 관계를 설정할지, 그리고 어떤 종류의 관계인지를 선택하는 것으로 구성됩니다. 관계 필드의 편집 창에는 관계에 있는 콘텐츠 타입 중 하나를 나타내는 2개의 회색 박스가 표시됩니다. 회색 박스 사이에는 가능한 모든 관계 타입이 표시됩니다.

1. 두 번째 회색 박스를 클릭하여 콘텐츠 타입 B를 정의합니다. 이미 생성된 컬렉션 타입이어야 합니다.
2. 콘텐츠 타입 간에 설정할 관계를 나타내는 아이콘을 클릭합니다.
3. 콘텐츠 타입 A의 *필드 이름*을 선택합니다. 즉, 콘텐츠 타입 A에서 필드에 사용될 이름입니다.
4. (관계 타입에 의해 비활성화된 경우 선택사항) 콘텐츠 타입 B의 *필드 이름*을 선택합니다.

</TabItem>

<TabItem value="advanced" label="고급 설정">

| 설정 이름   | 지침                                                                |
|----------------|-----------------------------------------------------------------------------|
| Private field  | Tick to make the field private and prevent it from being found via the API. |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_boolean.svg" /> Boolean {#boolean}

The Boolean field displays a toggle button to manage boolean values (e.g. Yes or No, 1 or 0, True or False).

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name  | Instructions                                                    |
|---------------|-----------------------------------------------------------------|
| Name          | Write the name of the Boolean field.                            |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Default value  | Choose the default value of the Boolean field: *true*, *null* or *false*.   |
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the field to have a different value per locale. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |
| Unique field   | Tick to prevent another field to be identical to this one.                  |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_json.svg" /> JSON {#json}

The JSON field allows to configure data in a JSON format, to store JSON objects or arrays.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name  | Instructions                                                    |
|---------------|-----------------------------------------------------------------|
| Name          | Write the name of the JSON field.                               |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the field to have a different value per locale. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_email.svg" /> Email {#email}

The Email field displays an email address field with format validation to ensure the email address is valid.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name  | Instructions                                                    |
|---------------|-----------------------------------------------------------------|
| Name          | Write the name of the Email field.                              |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Default value  | Write the default value of the Email field.                                 |
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the field to have a different value per locale. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |
| Unique field   | Tick to prevent another field to be identical to this one.                  |
| Maximum length | Tick to define a maximum number of characters allowed.                      |
| Minimum length | Tick to define a minimum number of characters allowed.                      |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_password.svg" /> Password {#password}

The Password field displays a password field that is encrypted.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name  | Instructions                                                    |
|---------------|-----------------------------------------------------------------|
| Name          | Write the name of the Password field.                           |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Default value  | Write the default value of the Password field.                              |
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the field to have a different value per locale. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |
| Maximum length | Tick to define a maximum number of characters allowed.                      |
| Minimum length | Tick to define a minimum number of characters allowed.                      |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_enum.svg" /> Enumeration {#enum}

The Enumeration field allows to configure a list of values displayed in a drop-down list.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name  | Instructions                                                    |
|---------------|-----------------------------------------------------------------|
| Name          | Write the name of the Enumeration field.                        |
| Values        | Write the values of the enumeration, one per line.              |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Default value  | Choose the default value of the Enumeration field.                          |
| Name override for GraphQL | Write a custom GraphQL schema type to override the default one for the field. |
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the field to have a different value per locale. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |

</TabItem>

</Tabs>

:::caution
Enumeration values should always have an alphabetical character preceding any number as it could otherwise cause the server to crash without notice when the GraphQL plugin is installed.
:::

#### <img width="28" src="/img/assets/icons/v5/ctb_uid.svg" /> UID {#uid}

The UID field displays a field that sets a unique identifier, optionally based on an existing other field from the same content-type.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name   | Instructions                                                    |
|----------------|-----------------------------------------------------------------|
| Name           | Write the name of the UID field. It must not contain special characters or spaces.                     |
| Attached field | Choose what existing field to attach to the UID field. Choose *None* to not attach any specific field. |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Default value  | Write the default value of the UID field.                                   |
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |
| Maximum length | Tick to define a maximum number of characters allowed.                      |
| Minimum length | Tick to define a minimum number of characters allowed.                      |

</TabItem>

</Tabs>

:::tip
The UID field can be used to create a slug based on the Attached field.
:::

#### <img width="28" src="/img/assets/icons/v5/ctb_richtext.svg" /> Rich Text (Markdown) {#rich-text-markdown}

The Rich Text (Markdown) field displays an editor with basic formatting options to manage rich text written in Markdown. This field can be used for long written content.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name | Instructions                                      |
|--------------|---------------------------------------------------|
| Name         | Write the name of the Rich Text (Markdown) field. |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                |
|----------------|-----------------------------------------------------------------------------|
| Default value  | Write the default value of the Rich Text field.                             |
| Private field  | Tick to make the field private and prevent it from being found via the API. |
| Enable localization for this field | (if [Internationalization plugin](/cms/features/internationalization) is enabled for the content-type) Allow the field to have a different value per locale. |
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.  |
| Maximum length | Tick to define a maximum number of characters allowed.                      |
| Minimum length | Tick to define a minimum number of characters allowed.                      |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_component.svg" /> Components {#components}

Components are a combination of several fields. Components allow to create reusable sets of fields, that can be quickly added to content-types, dynamic zones but also nested into other components.

When configuring a component through the Content-type Builder, it is possible to either:

- create a new component by clicking on *Create a new component* (see [Creating a new component](#new-component)),
- or use an existing one by clicking on *Use an existing component*.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name       | Instructions                                                    |
|--------------------|-----------------------------------------------------------------|
| Name               | Write the name of the component for the content-type.           |
| Select a component | When using an existing component only - Select from the drop-down list an existing component. |
| Type               | Choose between *Repeatable component* to be able to use several times the component for the content-type, or *Single component* to limit to only one time the use of the component. |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                            |
|----------------|-----------------------------------------------------------------------------------------|
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.              |
| Private field  | Tick to make the field private and prevent it from being found via the API.             |
| Maximum value  | For repeatable components only - Tick to define a maximum number of characters allowed. |
| Minimum value  | For repeatable components only - Tick to define a minimum number of characters allowed. |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the component to be translated per available locale. |

</TabItem>

</Tabs>

#### <img width="28" src="/img/assets/icons/v5/ctb_dz.svg" /> Dynamic zones {#dynamiczones}

Dynamic zones are a combination of components that can be added to content-types. They allow a flexible content structure as once in the Content Manager, administrators have the choice of composing and rearranging the components of the dynamic zone how they want.

<Tabs>

<TabItem value="base" label="Base settings">

| Setting name       | Instructions                                                    |
|--------------------|-----------------------------------------------------------------|
| Name               | Write the name of the dynamic zone for the content-type.        |

</TabItem>

<TabItem value="advanced" label="Advanced settings">

| Setting name   | Instructions                                                                            |
|----------------|-----------------------------------------------------------------------------------------|
| Required field | Tick to prevent creating or saving an entry if the field is not filled in.              |
| Maximum value  | Tick to define a maximum number of characters allowed.                                  |
| Minimum value  | Tick to define a minimum number of characters allowed.                                  |
| Enable localization for this field | (if [Internationalization](/cms/features/internationalization) is enabled for the content-type) Allow the dynamic zone to be translated per available locale. |

</TabItem>

</Tabs>

After configuring the settings of the dynamic zone, its components must be configured as well. It is possible to either choose an existing component or create a new one.

:::caution
When using dynamic zones, different components cannot have the same field name with different types (or with enumeration fields, different values).
:::

#### Custom fields

[Custom fields](/cms/features/custom-fields) are a way to extend Strapi's capabilities by adding new types of fields to content-types or components. Once installed (see [Marketplace](/cms/plugins/installing-plugins-via-marketplace) documentation), custom fields are listed in the _Custom_ tab when selecting a field for a content-type.

Each custom field type can have basic and advanced settings. The <ExternalLink to="https://market.strapi.io/plugins?categories=Custom+fields" text="Marketplace"/> lists available custom fields, and hosts dedicated documentation for each custom field, including specific settings.

### Deleting content-types

Content types and components can be deleted through the Content-type Builder. Deleting a content-type automatically deletes all entries from the Content Manager that were based on that content-type. The same goes for the deletion of a component, which is automatically deleted from every content-type or entry where it was used.

1. In the <Icon name="layout" /> Content-type Builder sub navigation, click on the name of the content-type or component to delete.
2. In the edition interface of the chosen content-type or component, click on the <Icon name="pencil-simple" /> **Edit** button on the right side of the content-type's or component's name.
3. In the edition window, click on the **Delete** button.
4. In the confirmation window, confirm the deletion.
5. Click on the **Save** button in the Content-type Builder sub navigation.

:::caution
Deleting a content-type only deletes what was created and available from the Content-type Builder, and by extent from the admin panel of your Strapi application. All the data that was created based on that content-type is however kept in the database. For more information, please refer to the related <ExternalLink to="https://github.com/strapi/strapi/issues/1114" text="GitHub issue"/>.
:::

<ThemedImage
  alt="Deletion of content type in Content-type Builder"
  sources={{
    light: '/img/assets/content-type-builder/new_CTB_deletion.png',
    dark: '/img/assets/content-type-builder/new_CTB_deletion_DARK.png',
  }}
/>
