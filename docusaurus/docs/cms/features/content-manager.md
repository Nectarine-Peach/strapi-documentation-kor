---
title: 콘텐츠 매니저
description: 콘텐츠 매니저 사용법을 알아보세요.
toc_max_heading_level: 4
tags:
- 관리자 패널
- 콘텐츠 매니저
- 리스트 뷰
- 편집 뷰
- 컴포넌트
- 다이나믹 존
- 관계 필드
---

import ScreenshotNumberReference from '/src/components/ScreenshotNumberReference.jsx';

# 콘텐츠 매니저

<Icon name="feather" /> 콘텐츠 매니저는 관리자 패널의 메인 네비게이션에서 접근할 수 있으며, 사용자가 콘텐츠를 작성하고 관리할 수 있도록 합니다.

<IdentityCard>
  <IdentityCardItem icon="user" title="역할 및 권한">Roles > Plugins - Content Manager에서 최소 "Configure view" 권한 필요</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

## 개요

<!--
<ThemedImage
alt="Content Manager"
sources={{
    light: '/img/assets/content-manager-guideflow2.gif',
    dark: '/img/assets/content-manager-guideflow2.gif',
  }}
/>
-->

<!-- TODO: create dark mode version and replace the darkId value -->
<Guideflow lightId="zpen5g4t8p" darkId="9r22q12szr" />

The <Icon name="feather" /> Content Manager contains the available collection and single content-types which were created beforehand using the [Content-type Builder](/cms/features/content-type-builder).

Content can be created, managed and published from the 2 categories displayed in the sub navigation of the <Icon name="feather" /> Content Manager:

- *Collection types*, which lists available content-types managing several entries. For each available collection type, multiple entries can be created, which is why each collection type is divided into 2 interfaces:
  - the list view, which displays a table with all entries created for that collection type.
  - the edit view, which focuses on a chosen entry of your collection type, and from where you can actually manage the content.

- *Single types*, which lists available content-types with only one entry. Unlike collection types, which have multiple entries, single types are not created for multiple uses. In other words, there can only be one default entry per available single type. There is therefore no list view in the Single types category.

:::tip
Click the search icons <Icon name="magnifying-glass" classes="ph-bold" /> to use a text search and find one of your content-types and/or entries more quickly!

Specifically for your collection types' entries, you can also use the <Icon name="funnel-simple" classes="ph-bold" /> **Filters** button to set condition-based filters, which add to one another (i.e., if you set several conditions, only the entries that match all the conditions will be displayed).
:::

:::strapi Strapi AI
A new AI-powered Content Manager is now available for testing in private beta. [Sign up now!](https://strapi.io/ai).
:::

<!-- TO INTEGRATE IN THE PAGE? USE A GUIDEFLOW?

From the list view, it is possible to:

- create a new entry <ScreenshotNumberReference number="1" />,
- make a textual search <ScreenshotNumberReference number="2" /> or set filters <ScreenshotNumberReference number="3" /> to find specific entries,
- if [Internationalization (i18n)](/cms/features/internationalization) is enabled, filter by locale to display only the entries [translated](/cms/features/internationalization) in a chosen locale <ScreenshotNumberReference number="4" />,
- configure the fields displayed in the table of the list view <ScreenshotNumberReference number="5" />,
- if [Draft & Publish](/cms/features/draft-and-publish) is enabled, see the status of each entry <ScreenshotNumberReference number="6" />,
- perform actions on a specific entry by clicking on <Icon name="dots-three-outline" /> <ScreenshotNumberReference number="7" /> at the end of the row:
  - edit <Icon name="pencil-simple" /> (see [Writing content](/cms/features/content-manager/writing-content.md)), duplicate <Icon name="copy" />, or delete <Icon name="trash"/> (see [Deleting content](/cms/features/draft-and-publish#deleting-content)) the entry,
  - if [Draft & Publish](/cms/features/draft-and-publish) is enabled, <Icon name="x-circle" /> unpublish the entry, <Icon name="x-circle" /> or discard its changes,
  - if [Internationalization (i18n)](/cms/features/internationalization) is enabled, ![Delete locale icon](/img/assets/icons/v5/delete-locale.svg) delete a given locale,
- select multiple entries to simultaneously [publish, unpublish](/cms/features/draft-and-publish#bulk-publishing-and-unpublishing), or [delete](/cms/features/draft-and-publish#deleting-content).

:::tip
Sorting can be enabled for most fields displayed in the list view table (see <ExternalLink to="../content-manager/configuring-view-of-content-type.md" text="Configuring the views of a content-type"/>). Click on a field name, in the header of the table, to sort on that field.
:::
-->

<!-- WON'T BE INTEGRATED - TO BE VALIDATED

#### Filtering entries {#filtering-entries}

Right above the list view table, on the left side of the interface, a <Icon name="funnel-simple" classes="ph-bold" /> **Filters** button is displayed. It allows to set one or more condition-based filters, which add to one another (i.e. if you set several conditions, only the entries that match all the conditions will be displayed).

<ThemedImage
  alt="Filters in the Content Manager"
  sources={{
    light: '/img/assets/content-manager/content-manager_filters2.png',
    dark: '/img/assets/content-manager/content-manager_filters2_DARK.png',
  }}
/>

To set a new filter:

1. Click on the <Icon name="funnel-simple" classes="ph-bold" /> **Filters** button.
2. Click on the 1st drop-down list to choose the field on which the condition will be applied.
3. Click on the 2nd drop-down list to choose the type of condition to apply.
4. Enter the value(s) of the condition in the remaining textbox.
5. Click on the **Add filter** button.

:::note
When active, filters are displayed next to the <Icon name="funnel-simple" classes="ph-bold" /> **Filters** button. They can be removed by clicking on the delete icon <Icon name="x" />.
:::
-->

## Configuration

Both the list view and the edit view can be configured, and the former can either be configured temporarily or permanently.

### Configuring the list view {#list-view-settings}

<br/>

#### Temporary configuration

By configuring temporarily the list view, the configurations will be reset as soon as the page is refreshed or when navigating outside the Content Manager. This configuration allows to temporarily choose which fields to display in the list view's table.

1. Click on the settings button <Icon name="gear-six" />.
2. Tick the boxes associated with the field you want to be displayed in the table.
3. Untick the boxes associated with the fields you do not want to be displayed in the table.

<!-- MAY BE REMOVED - NOT SURE ABOUT RELEVANCE

:::tip
Relational fields can also be displayed in the list view. Please refer to <ExternalLink to="../content-manager/configuring-view-of-content-type.md" text="Configuring the views of a content-type"/> for more information on their specificities.
:::
-->

<ThemedImage
  alt="Displayed fields in the settings of a list view in the Content Manager"
  sources={{
    light: '/img/assets/content-manager/content-manager_displayed-fields.png',
    dark: '/img/assets/content-manager/content-manager_displayed-fields_DARK.png',
  }}
/>

#### Permanent & advanced configuration

By configuring permanently the list view, you not only ensure that they are not reset at every page refresh or navigation, but you also have access to more options (e.g., enablement/disablement of search, filters and bulk actions, reordering of the list view table's fields etc.).

:::note
The configurations only apply to the list view of the collection type from which the settings are accessed (i.e., disabling the filters or search options for a collection type will not automatically also disable these same options for all other collection types).
:::

<ThemedImage
  alt="Settings of a list view in the Content Manager"
  sources={{
    light: '/img/assets/content-manager/content-manager_settings-list-view.png',
    dark: '/img/assets/content-manager/content-manager_settings-list-view_DARK.png',
  }}
/>

<Tabs groupId="ListViewConfig">

<TabItem value="ListViewSettings" label="Settings">

1. In the list view of your collection type, click on the settings button <Icon name="gear-six" /> then <Icon name="list-plus" classes="ph-bold" /> **Configure the view** to be redirected to the list view configuration interface.
2. In the Settings area, define your chosen new settings:

| Setting name           | Instructions                                                                                       |
| ---------------------- | -------------------------------------------------------------------------------------------------- |
| Enable search          | Click on **TRUE** or **FALSE** to able or disable the search.                                          |
| Enable filters         | Click on **TRUE** or **FALSE** to able or disable filters.                                             |
| Enable bulk actions    | Click on **TRUE** or **FALSE** to able or disable the multiple selection boxes in the list view table. |
| Entries per page       | Choose among the drop-down list the number of entries per page.                                    |
| Default sort attribute | Choose the sorting field that will be used by default.                                             |
| Default sort order     | Choose the sorting type that will be applied by default.                                           |

3. Click on the **Save** button.

</TabItem>

<TabItem value="ListViewDisplay" label="View">

1. In the list view of your collection type, click on the settings button <Icon name="gear-six" /> then <Icon name="list-plus" classes="ph-bold" /> **Configure the view** to be redirected to the list view configuration interface.
2. In the View area, define what fields to display in the list view table, and in what order:
   - Click the add button <Icon name="plus" classes="ph-bold" /> to add a new field.
   - Click the delete button <Icon name="x" /> to remove a field.
   - Click the reorder button <Icon name="dots-six-vertical" classes="ph-bold" /> and drag and drop it to the place you want it to be displayed among the other fields.
3. Click the edit button <Icon name="pencil-simple" /> to access its available own settings:

| Setting name              | Instructions                                                              |
| ------------------------- | ------------------------------------------------------------------------- |
| Label                     | Write the label to be used for the field in the list view table.          |
| Enable sort on this field | Click on **TRUE** or **FALSE** to able or disable the sort on the field.  |

4. Click on the **Save** button.

:::note
Relational fields can also be displayed in the list view. There are however some specificities to keep in mind:

- Only one field can be displayed per relational field.
- Only first-level fields can be displayed (i.e. fields from the relation of a relation can't be displayed).
- If the displayed field contains more than one value, not all its values will be displayed, but a counter indicating the number of values. You can hover this counter to see a tooltip indicating the first 10 values of the relational field.

Note also that relational fields have a couple limitations when it comes to sorting options:

- 여러 필드를 표시하는 관계형 필드에는 정렬을 활성화할 수 없습니다.
- 관계형 필드는 기본 정렬로 설정할 수 없습니다.
:::

</TabItem>

</Tabs>

### 편집 뷰 구성 {#edit-view-settings}

<ThemedImage
  alt="Configuring the edit view of the Content Manager"
  sources={{
    light: '/img/assets/content-manager/edit-view-config2.png',
    dark: '/img/assets/content-manager/edit-view-config2_DARK.png',
  }}
/>

<Tabs groupId="EditViewConfig">

<TabItem value="EditViewSettings" label="설정">

1. 콘텐츠 타입의 편집 뷰에서 <Icon name="dots-three-outline" /> 버튼을 클릭한 다음 <Icon name="list-plus" classes="ph-bold" /> **뷰 구성**을 클릭합니다.
2. 설정 영역에서 선택한 새 설정을 정의합니다:

| 설정 이름    | 지침                                                                          |
| --------------- | ------------------------------------------------------------------------------------- |
| 항목 제목     | 드롭다운 목록에서 항목의 제목으로 사용할 필드를 선택합니다. |

3. **저장** 버튼을 클릭합니다.

</TabItem>

<TabItem value="EditViewDisplay" label="뷰">

1. 콘텐츠 타입의 편집 뷰에서 <Icon name="dots-three-outline" /> 버튼을 클릭한 다음 <Icon name="list-plus" classes="ph-bold" /> **뷰 구성**을 클릭합니다.
2. 뷰 영역에서 목록 뷰 테이블에 표시할 필드(관계형 필드 포함), 순서 및 크기를 정의합니다:
   - <Icon name="plus" classes="ph-bold" /> **다른 필드 삽입** 버튼을 클릭하여 새 필드를 추가합니다.
   - 삭제 버튼 <Icon name="x" />을 클릭하여 필드를 제거합니다.
   - 재정렬 버튼 <Icon name="dots-six-vertical" classes="ph-bold" />을 클릭하고 드래그 앤 드롭으로 다른 필드들 사이에서 원하는 위치로 이동합니다.
3. 필드의 편집 버튼 <Icon name="pencil-simple" />을 클릭하여 사용 가능한 설정에 접근합니다:

| 설정 이름    | 지침                                                                              |
| --------------- | ----------------------------------------------------------------------------------------- |
| 라벨           | 필드에 사용할 라벨을 작성합니다.                                        |
| 설명     | 다른 관리자가 필드를 올바르게 채울 수 있도록 필드에 대한 설명을 작성합니다.         |
| 플레이스홀더     | 필드에 기본적으로 표시될 플레이스홀더를 작성합니다.                   |
| 편집 가능 필드  | **TRUE** 또는 **FALSE**를 클릭하여 관리자가 필드를 편집할 수 있도록 활성화하거나 비활성화합니다. |
| 크기            | 콘텐츠 매니저에서 필드가 표시될 크기를 선택합니다. 이 설정은 JSON 및 Rich Text 필드, 동적 영역 및 컴포넌트에서는 사용할 수 없습니다. |
| 항목 제목     | *(관계형 필드만)* 관계형 필드에 사용할 항목 제목을 작성합니다. 관계형 필드의 항목 제목은 포괄적일수록 관리자가 편집 뷰에서 관계형 필드의 콘텐츠를 관리하기 쉬워지므로 신중하게 선택하는 것이 좋습니다. |

4. **저장** 버튼을 클릭합니다.

:::caution
컴포넌트 필드의 설정과 표시는 항목의 편집 뷰 구성 페이지를 통해 관리하고 재정렬할 수 없습니다. 컴포넌트의 **컴포넌트 레이아웃 설정** 버튼을 클릭하여 컴포넌트 자체의 구성 페이지에 접근하세요. 항목과 동일한 설정 및 표시 옵션을 찾을 수 있지만, 이는 해당 컴포넌트에만 적용됩니다.

또한 설정은 컴포넌트 자체에 대해 정의되므로, 해당 컴포넌트가 사용되는 다른 모든 콘텐츠 타입에 자동으로 적용됩니다.
:::

</TabItem>

</Tabs>

## 사용법

<br/>

### 콘텐츠 생성 및 작성

Strapi에서 콘텐츠 작성은 특정 콘텐츠(예: 텍스트, 숫자, 미디어 등)를 포함하도록 의도된 필드를 채우는 것으로 구성됩니다. 이러한 필드는 [콘텐츠 타입 빌더](/cms/features/content-type-builder)를 통해 컬렉션 또는 단일 타입에 대해 미리 구성되었습니다.

<ThemedImage
  alt="Edit view to write content"
  sources={{
    light: '/img/assets/content-manager/edit-view3.png',
    dark: '/img/assets/content-manager/edit-view3_DARK.png',
  }}
/>

콘텐츠를 작성하거나 편집하려면:

1. <Icon name="feather" /> 콘텐츠 매니저에서:
    - 새 항목을 생성하려면 선택한 컬렉션 타입의 우측 상단에 있는 **새 항목 생성** 버튼을 클릭하거나,
    - 이미 생성된 컬렉션 타입 항목 또는 단일 타입의 편집 뷰에 접근합니다.
2. 사용 가능한 필드 스키마에 따라 콘텐츠를 작성합니다. 각 필드 타입을 채우는 방법에 대한 자세한 정보와 지침은 아래 표를 참고하세요.

:::note
새 항목은 일부 콘텐츠가 작성되고 한 번 저장된 후에만 생성된 것으로 간주됩니다. 그때서야 새 항목이 목록 뷰에 나열됩니다.
:::

<!-- MAY BE REMOVED - NOT SURE ABOUT RELEVANCE

:::info
If Draft & Publish is enabled for your content-type (it's enabled by default), the fields work the same way whether you are editing the draft or published version.
:::
-->

| 필드 이름  | 지침                                                                                                                                                                                                                                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 텍스트        | 텍스트박스에 콘텐츠를 작성합니다.                                                                                                                                                                                                                                                                                                                                        |
| 리치 텍스트 (Markdown) | 에디터에서 Markdown으로 텍스트 콘텐츠를 작성합니다. 선택한 텍스트에 적용할 수 있는 기본 서식 옵션(제목, 굵게, 기울임체, 밑줄)이 에디터 상단 바에서 제공됩니다. 모드를 전환하기 위한 **미리보기 모드/Markdown 모드** 버튼도 사용할 수 있습니다. <br /><br /> 💡 하단 바의 **확장**을 클릭하여 박스를 확장할 수 있습니다. 편집할 수 있는 텍스트박스와 미리보기를 동시에 나란히 표시합니다. |
| 리치 텍스트 (Blocks) | 모든 추가/업데이트를 실시간으로 자동 렌더링하는 에디터에서 콘텐츠를 작성하고 관리합니다. Blocks 에디터에서 단락은 텍스트 블록으로 동작합니다: 단락에 마우스를 올리면 콘텐츠를 재정렬하기 위해 클릭할 수 있는 아이콘 <Icon name="dots-six-vertical" classes="ph-bold"/>이 표시됩니다. 콘텐츠를 서식화하거나 풍부하게 만드는 옵션도 에디터 상단 바에서 접근할 수 있습니다(기본 서식 옵션, 코드, 링크, 이미지 등). <!-- <br /><br /> 💡 에디터에서 `/`를 입력하면 사용 가능한 모든 옵션 목록에 접근하여 하나를 선택할 수 있습니다. --> <br /><br /> 💡 Blocks 에디터에서 텍스트 서식 키보드 단축키를 사용할 수 있습니다(예: 굵게, 기울임체, 밑줄, 링크 붙여넣기). |
| 숫자      | 텍스트박스에 숫자를 작성합니다. 박스 오른쪽에 표시되는 위아래 화살표를 사용하여 텍스트박스에 표시된 현재 숫자를 증가시키거나 감소시킬 수 있습니다.                                                                                                                                                                                                       |
| 날짜        | 1. 날짜 및/또는 시간 박스를 클릭합니다. <br /> 2. 날짜와 시간을 입력하거나 달력을 사용하여 날짜를 선택하고/또는 목록에서 시간을 선택합니다. 달력 뷰는 키보드 기반 네비게이션을 완전히 지원합니다. |
| 미디어       | 1. 미디어 영역을 클릭합니다. <br /> 2. [미디어 라이브러리](/cms/features/media-library)에서 자산을 선택하거나 생성한 [폴더](/cms/features/media-library#organizing-assets-with-folders)에서 선택하거나, **더 많은 자산 추가** 버튼을 클릭하여 미디어 라이브러리에 새 파일을 추가합니다. <br /><br /> 💡 선택한 파일을 미디어 영역에 드래그 앤 드롭할 수 있습니다.                                                                                                                                   |
| 관계    | 드롭다운 목록에서 항목을 선택합니다. 자세한 정보는 [관계형 필드](#relational-fields)를 참고하세요.                                                                                                                                                                                                          |
| 불린     | **TRUE** 또는 **FALSE**를 클릭합니다.                                                                                                                                                                                                                                                                                                                                               |
| JSON        | 코드 텍스트박스에 JSON 형식으로 콘텐츠를 작성합니다.                                                                                                                                                                                                                                                                                                                  |
| 이메일       | 완전하고 유효한 이메일 주소를 작성합니다.                                                                                                                                                                                                                                                                                                                                 |
| 비밀번호    | 비밀번호를 작성합니다. <br /><br /> 💡 박스 오른쪽에 표시된 <Icon name="eye" /> 아이콘을 클릭하여 비밀번호를 표시합니다.                                                                                                                                                                                                                                                                |
| 열거형 | 1. 드롭다운 목록을 클릭합니다. <br /> 2. 목록에서 항목을 선택합니다.                                                                                                                                                                                                                                                                                                       |
| UID         | 텍스트박스에 고유 식별자를 작성합니다. 박스 오른쪽에 표시된 "재생성" 버튼을 사용하여 콘텐츠 타입 이름을 기반으로 UID를 자동 생성할 수 있습니다.                                                                                                                                                                                                |

:::note
[커스텀 필드](/cms/features/content-type-builder#custom-fields) 작성은 필드가 처리하는 콘텐츠 타입에 따라 달라집니다. <ExternalLink to="https://market.strapi.io" text="마켓플레이스"/>에서 호스팅되는 각 커스텀 필드의 전용 문서를 참고하세요.
:::

#### 컴포넌트

컴포넌트는 편집 뷰에서 함께 그룹화된 여러 필드의 조합입니다. 컴포넌트의 콘텐츠 작성은 독립적인 필드와 정확히 동일하게 작동하지만, 컴포넌트에는 몇 가지 특수성이 있습니다.

컴포넌트에는 2가지 타입이 있습니다: 반복 불가능한 컴포넌트와 반복 가능한 컴포넌트입니다.

<Tabs groupId="Components">

<TabItem value="NonRepeatable" label="반복 불가능한 컴포넌트">

<ThemedImage
  alt="Non-repeatable component - No entry yet"
  width="80%"
  sources={{
    light: '/img/assets/content-manager/edit-view_component3.png',
    dark: '/img/assets/content-manager/edit-view_component3_DARK.png',
  }}
/>
<ThemedImage
  alt="Non-repeatable component - With entries"
  width="80%"
  sources={{
    light: '/img/assets/content-manager/edit-view_component2.png',
    dark: '/img/assets/content-manager/edit-view_component2_DARK.png',
  }}
/>

반복 불가능한 컴포넌트는 한 번만 사용할 수 있는 필드의 조합입니다.

기본적으로 필드의 조합은 편집 뷰에 직접 표시되지 않습니다:

1. 추가 버튼 <Icon name="plus-circle" />을 클릭하여 컴포넌트를 추가합니다.
2. 컴포넌트의 필드를 채웁니다.

반복 불가능한 컴포넌트를 삭제하려면 컴포넌트 영역의 우측 상단에 위치한 삭제 버튼 <Icon name="trash"/>을 클릭합니다.

</TabItem>

<TabItem value="Repeatable" label="반복 가능한 컴포넌트">

<ThemedImage
  alt="Repeatable component"
  width="80%"
  sources={{
    light: '/img/assets/content-manager/edit-view_component4.png',
    dark: '/img/assets/content-manager/edit-view_component4_DARK.png',
  }}
/>

반복 가능한 컴포넌트도 필드의 조합이지만, 모두 동일한 필드 조합을 따르는 여러 컴포넌트 항목을 생성할 수 있습니다.

새 항목을 추가하고 필드 조합을 표시하려면:

1. 추가 버튼 <Icon name="plus-circle" />을 클릭하여 컴포넌트를 추가합니다.
2. 컴포넌트의 필드를 채웁니다.
3. (선택사항) **항목 추가** 버튼을 클릭하고 필드를 다시 채웁니다.

반복 가능한 컴포넌트 항목은 항목 영역 오른쪽에 표시된 버튼을 사용하여 편집 뷰에서 직접 재정렬하거나 삭제할 수 있습니다.

- 드래그 앤 드롭 버튼 <Icon name="dots-six-vertical" classes="ph-bold" />을 사용하여 반복 가능한 컴포넌트의 항목을 재정렬합니다.
- 삭제 버튼 <Icon name="trash"/>을 사용하여 반복 가능한 컴포넌트에서 항목을 삭제합니다.

:::note
일반 필드와 달리 반복 가능한 컴포넌트 항목의 순서는 중요합니다. 최종 사용자가 콘텐츠를 읽거나 보는 방식과 정확히 일치해야 합니다.
:::

</TabItem>

</Tabs>

#### 동적 영역

동적 영역은 컴포넌트의 조합이며, 각 컴포넌트는 여러 필드로 구성됩니다. 동적 영역의 콘텐츠를 작성하려면 필드에 접근하기 위한 추가 단계가 필요합니다.

<ThemedImage
  alt="동적 영역을 위한 콘텐츠 작성"
  sources={{
    light: '/img/assets/content-manager/edit-view_dynamic-zone-1.png',
    dark: '/img/assets/content-manager/edit-view_dynamic-zone-1_DARK.png',
  }}
/>

<ThemedImage
  alt="동적 영역을 위한 콘텐츠 작성"
  sources={{
    light: '/img/assets/content-manager/edit-view_dynamic-zone-2.png',
    dark: '/img/assets/content-manager/edit-view_dynamic-zone-2_DARK.png',
  }}
/>

1. <Icon name="plus-circle" /> **[동적 영역 이름]에 컴포넌트 추가** 버튼을 클릭합니다.
2. 동적 영역에 사용 가능한 컴포넌트를 선택합니다.
3. 컴포넌트의 필드를 채웁니다.

동적 영역의 컴포넌트는 컴포넌트 영역의 우측 상단에 표시된 버튼을 사용하여 편집 뷰에서 직접 재정렬하거나 삭제할 수도 있습니다.

- 드래그 앤 드롭 버튼 <Icon name="dots-six-vertical" classes="ph-bold" />을 사용하여 동적 영역의 컴포넌트를 재정렬합니다.
- 삭제 버튼 <Icon name="trash"/>을 사용하여 동적 영역에서 컴포넌트를 삭제합니다.

:::tip
키보드를 사용하여 컴포넌트를 재정렬할 수도 있습니다: Tab을 사용하여 컴포넌트에 포커스를 맞추고, 드래그 앤 드롭 버튼 <Icon name="dots-six-vertical" classes="ph-bold" />에서 Space를 누른 다음 화살표 키를 사용하여 재정렬하고, Space를 다시 눌러 항목을 놓습니다.
:::

:::note
일반 필드와 달리 동적 필드 내부의 필드와 컴포넌트의 순서는 중요합니다. 최종 사용자가 콘텐츠를 읽거나 보는 방식과 정확히 일치해야 합니다.
:::

#### 관계형 필드

콘텐츠 타입에 추가된 관계 타입 필드는 다른 컬렉션 타입과의 관계를 설정할 수 있게 해줍니다. 이러한 필드를 "관계형 필드"라고 합니다.

관계형 필드의 콘텐츠는 해당 필드가 속한 콘텐츠 타입의 편집 뷰에서 작성됩니다. 하지만 관계형 필드는 다른 컬렉션 타입의 하나 또는 여러 항목을 가리킬 수 있기 때문에, 콘텐츠 매니저에서 콘텐츠 타입의 관계형 필드를 관리하여 어떤 항목이 관련있는지 선택할 수 있습니다.

<details>
<summary>관계형 필드 예시</summary>

내 Strapi 관리자 패널에서 2개의 컬렉션 타입을 생성했습니다:

- Restaurant, 각 항목은 레스토랑입니다
- Category, 각 항목은 레스토랑의 유형입니다

각 레스토랑에 카테고리를 할당하고 싶으므로, 2개의 컬렉션 타입 간에 관계를 설정했습니다: 레스토랑은 하나의 카테고리를 가질 수 있습니다.

콘텐츠 매니저에서 Restaurant 항목의 편집 뷰에서 Category 관계형 필드를 관리하고, 내 레스토랑에 관련된 Category 항목을 선택할 수 있습니다.
<br/>

</details>

<!-- MAY BE REMOVED - FEELS LIKE REPETITION

The relational fields of a content-type are displayed among regular fields. For each relational field is displayed a drop-down list containing all available entry titles. It allows to choose which entry the relational fields should point to. You can either choose one or several entries depending on the type of relation that was established.-->

<ThemedImage
  alt="Relational fields in the edit view"
  sources={{
    light: '/img/assets/content-manager/edit-view_relational-fields2.png',
    dark: '/img/assets/content-manager/edit-view_relational-fields2_DARK.png',
  }}
/>

<Tabs groupId="RelationalFields">

<TabItem value="OneChoice" label="단일 선택 관계형 필드">

다대일, 일대일, 단방향 관계 타입은 관계형 필드당 하나의 항목만 선택할 수 있습니다.

<ThemedImage
  alt="단일 선택 관계형 필드"
  width="40%"
  sources={{
    light: '/img/assets/content-manager/RF_one-choice2.png',
    dark: '/img/assets/content-manager/RF_one-choice2_DARK.png',
  }}
/>

유일한 관련 관계형 필드의 항목을 선택하려면:

1. 콘텐츠 타입의 편집 뷰에서 관계형 필드의 드롭다운 목록을 클릭합니다.
2. 항목 목록에서 하나를 선택합니다.

드롭다운 목록에서 선택한 항목을 제거하려면 삭제 버튼 <Icon name="x" />을 클릭합니다.

</TabItem>

<TabItem value="MultipleChoice" label="다중 선택 관계형 필드">

다대다, 일대다, 다방향 관계 타입은 관계형 필드당 여러 항목을 선택할 수 있습니다.

<ThemedImage
  alt="다중 선택 관계형 필드"
  width="40%"
  sources={{
    light: '/img/assets/content-manager/RF_multiple-choices2.png',
    dark: '/img/assets/content-manager/RF_multiple-choices2_DARK.png',
  }}
/>

관련 관계형 필드의 항목들을 선택하려면:

1. 콘텐츠 타입의 편집 뷰에서 관계형 필드의 드롭다운 목록을 클릭합니다.
2. 항목 목록에서 하나를 선택합니다.
3. 모든 관련 항목이 선택될 때까지 2단계를 반복합니다.

항목을 제거하려면 선택된 항목 목록에서 X 버튼 <Icon name="x" classes="ph-bold" />을 클릭합니다.

다중 선택 관계형 필드의 항목은 드래그 버튼 <Icon name="dots-six-vertical" classes="ph-bold" />으로 표시되어 재정렬할 수 있습니다. 항목을 이동하려면 클릭하여 드래그하고, 원하는 위치로 끌어다 놓은 다음 놓습니다.

</TabItem>

</Tabs>

:::tip
- 모든 항목이 기본적으로 나열되지는 않습니다: **더 불러오기** 버튼을 클릭하여 더 많은 항목을 표시할 수 있습니다. 또한 목록을 스크롤하여 항목을 선택하는 대신, 관계형 필드 드롭다운 목록을 클릭하고 입력하여 특정 항목을 검색할 수 있습니다.

- 항목의 이름을 클릭하면 관계형 필드의 콘텐츠 타입을 편집할 수 있는 모달이 표시됩니다. 현재는 즉석에서 관계를 편집할 수만 있고 새로 생성할 수는 없습니다.
:::

:::note
- 관계형 필드가 속한 콘텐츠 타입에 [초안 및 게시 기능](/cms/features/draft-and-publish)이 활성화된 경우, 드롭다운 목록의 항목 이름 옆에 파란색 또는 녹색 점이 표시됩니다. 이는 각각 초안 또는 게시된 콘텐츠의 항목 상태를 나타냅니다.
- 콘텐츠 타입에 [국제화(i18n) 기능](/cms/features/internationalization)이 활성화된 경우, 항목 목록이 제한되거나 로케일마다 다를 수 있습니다. 관계형 필드에 선택할 수 있는 관련 항목만 나열됩니다.
:::

<!-- Add a section "Managing entries" here with the explanations of the list view interface? Or before "Creating & Writing content"? Or maybe have 1. "Creating & managing entries" 2. "Writing content"? Or just use a Guideflow? -->

### 콘텐츠 삭제

컬렉션 타입의 항목이나 단일 타입의 기본 항목을 삭제하여 콘텐츠를 삭제할 수 있습니다.

1. 항목의 편집 뷰에서 인터페이스 우측 상단의 <Icon name="dots-three-outline" />을 클릭하고 **문서 삭제** 버튼을 클릭합니다.<br/>콘텐츠 타입에 국제화가 활성화된 경우, **로케일 삭제** 버튼을 클릭하여 현재 선택된 로케일만 삭제할 수도 있습니다.
2. 팝업되는 창에서 **확인** 버튼을 클릭하여 삭제를 확인합니다.

<ThemedImage
  alt="항목 삭제"
  sources={{
    light: '/img/assets/content-manager/deleting-entries.png',
    dark: '/img/assets/content-manager/deleting-entries_DARK.png',
  }}
/>

:::tip
컬렉션 타입의 목록 뷰에서 테이블의 항목 기록 오른쪽에 있는 <Icon name="dots-three-outline" />을 클릭한 다음 <Icon name="trash"/> **문서 삭제** 버튼을 선택하여 항목을 삭제할 수 있습니다.<br/>[국제화](/cms/features/internationalization)가 콘텐츠 타입에 활성화된 경우, **문서 삭제**는 모든 로케일을 삭제하고 **로케일 삭제**는 현재 나열된 로케일만 삭제합니다.
