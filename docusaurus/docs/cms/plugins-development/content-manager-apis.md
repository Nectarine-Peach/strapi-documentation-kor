---
title: 콘텐츠 매니저 API
description: 콘텐츠 매니저 API 참조는 플러그인이 콘텐츠 매니저 목록 보기 및 편집 보기에 액션과 옵션을 추가하는 데 사용할 수 있는 API를 나열합니다.
displayed_sidebar: cmsSidebar
toc_max_heading_level: 4
tags:
- 관리자 패널 API
- 플러그인 개발
- 플러그인
---

# 콘텐츠 매니저 API

콘텐츠 매니저 API는 [관리자 패널 API](/cms/plugins-development/admin-panel-api)의 일부입니다. 이는 플러그인에서 [콘텐츠 매니저](/cms/features/content-manager)에 콘텐츠나 옵션을 추가하는 방법으로, [Injection Zone](/cms/plugins-development/admin-panel-api#injection-zones-api)으로 할 수 있는 것처럼 자신의 플러그인에서 기능을 추가하여 콘텐츠 매니저를 확장할 수 있습니다.

Strapi 5는 4가지 콘텐츠 매니저 API를 제공하며, 모두 `app.getPlugin('content-manager').apis`를 통해 접근할 수 있습니다:

- [`addEditViewSidePanel`](#addeditviewsidepanel),
- [`addDocumentAction`](#adddocumentaction),
- [`addDocumentHeaderAction`](#adddocumentheaderaction),
- [`addBulkAction`](#addbulkaction).

## 일반 정보

모든 콘텐츠 매니저 API는 동일한 API 형태를 공유하며 컴포넌트를 사용해야 합니다.

### API 형태

모든 콘텐츠 매니저 API는 동일한 방식으로 작동합니다: 사용하려면 플러그인의 [bootstrap](/cms/plugins-development/admin-panel-api#bootstrap) 함수에서 다음 2가지 방법 중 하나로 호출합니다:

- 추가하려는 것이 포함된 배열을 전달하는 방법. 예를 들어, 다음 코드는 ReleasesPanel을 현재 EditViewSidePanel의 끝에 추가합니다:
  
  ```jsx
  app.getPlugin('content-manager').apis.addEditViewSidePanel([ReleasesPanel])
  ```

- 현재 요소들을 받아서 새로운 요소들을 반환하는 함수를 전달하는 방법. 이는 예를 들어 목록의 특정 위치에 무언가를 추가하려는 경우 유용합니다:

  ```jsx
  app.getPlugin('content-manager').apis.addEditViewSidePanel(
    (panels) => [SuperImportantPanel, ...panels]
  )
  ```

### 컴포넌트

콘텐츠 매니저에 요소를 추가하려면 API에 컴포넌트를 전달해야 합니다. 이러한 컴포넌트는 기본적으로 일부 속성을 받아서 특정 형태의 객체를 반환하는 함수입니다(사용하는 함수에 따라 다름). 각 컴포넌트의 반환 객체는 사용하는 함수에 따라 다르지만, ListView 또는 EditView API를 사용하는지에 따라 유사한 속성을 받습니다. 이러한 속성에는 보고 있거나 편집하고 있는 문서에 대한 중요한 정보가 포함됩니다.

#### ListViewContext

```jsx
interface ListViewContext {
  /**
   * 'single-types' | 'collection-types' 중 하나
   */
  collectionType: string;
  /**
   * 테이블에서 현재 선택된 문서들
   */
  documents: Document[];
  /**
   * 현재 콘텐츠 타입의 모델
   */
  model: string;
}
```

#### EditViewContext

```jsx
interface EditViewContext {
  /**
   * 콘텐츠 타입에 초안 및 발행이 활성화되지 않은 경우에만 null
   */
  activeTab: 'draft' | 'published' | null;
  /**
   * 'single-types' | 'collection-types' 중 하나
   */
  collectionType: string;
  /**
   * 누군가가 엔트리를 생성하는 중이면 undefined
   */
  document?: Document;
  /**
   * 누군가가 엔트리를 생성하는 중이면 undefined
   */
  documentId?: string;
  /**
   * 누군가가 엔트리를 생성하는 중이면 undefined
   */
  meta?: DocumentMetadata;
  /**
   * 현재 콘텐츠 타입의 모델
   */
  model: string;
}
```

:::tip
타입과 API에 대한 자세한 정보는 <ExternalLink to="https://github.com/strapi/strapi/blob/develop/packages/core/content-manager/admin/src/content-manager.ts" text="Strapi 코드베이스의 `/admin/src/content-manager.ts` 파일"/>에서 찾을 수 있습니다.
:::

**예시:**

사이드바에 패널을 추가하는 방법:

```jsx title="my-plugin/components/my-panel.ts"
import type { PanelComponent, PanelComponentProps } from '@strapi/content-manager/strapi-admin';

const Panel: PanelComponent = ({ 
  activeTab, 
  collectionType, 
  document, 
  documentId, 
  meta, 
  model 
}: PanelComponentProps) => {
  return {
    title: 'My Panel',
    content: <p>I'm on {activeTab}</p>
  }
}
```

## 사용 가능한 API

<br/>

### `addEditViewSidePanel`

편집 보기 사이드바에 새 패널을 추가하는 데 사용합니다. 다음 예시와 같이 릴리즈 패널에 무언가를 추가하는 경우:

![addEditViewSidePanel](/img/assets/content-manager-apis/add-edit-view-side-panel.png)

```jsx
addEditViewSidePanel(panels: DescriptionReducer<PanelComponent> | PanelComponent[])
```

#### PanelDescription

API의 인터페이스는 다음 2가지 속성만 받습니다:

```jsx
{
  title: string;
  content: React.ReactNode;	
}
```

### `addDocumentAction`

이 API를 사용하여 콘텐츠 매니저의 편집 보기 또는 목록 보기에 더 많은 액션을 추가합니다. 3가지 위치를 사용할 수 있습니다:

- 편집 보기의 `header`:

    ![Header of the Edit view](/img/assets/content-manager-apis/add-document-action-header.png)
- 편집 보기의 `panel`:

    ![Panel of the Edit View](/img/assets/content-manager-apis/add-document-action-panel.png)
- 목록 보기의 `table-row`:

    ![Table-row in the List View](/img/assets/content-manager-apis/add-document-action-tablerow.png)

```jsx
addDocumentAction(actions: DescriptionReducer<DocumentActionComponent> | DocumentActionComponent[])
```

#### DocumentActionDescription

API의 인터페이스와 속성은 다음과 같습니다: 

```jsx
interface DocumentActionDescription {
    label: string;
    onClick?: (event: React.SyntheticEvent) => Promise<boolean | void> | boolean | void;
    icon?: React.ReactNode;
    /**
     * @default false
     */
    disabled?: boolean;
    /**
     * @default 'panel'
     * @description 액션이 렌더링될 위치
     */
    position?: DocumentActionPosition | DocumentActionPosition[];
    dialog?: DialogOptions | NotificationOptions | ModalOptions;
    /**
     * @default 'secondary'
     */
    variant?: ButtonProps['variant'];
}

type DocumentActionPosition = 'panel' | 'header' | 'table-row';

interface DialogOptions {
    type: 'dialog';
    title: string;
    content?: React.ReactNode;
    variant?: ButtonProps['variant'];
    onConfirm: () => Promise<boolean | void> | boolean | void;
    /**
     * @default 'Confirm'
     */
    confirmButtonLabel?: string;
}

interface NotificationOptions {
    type: 'notification';
    title: string;
    /**
     * @default 'success'
     */
    variant?: 'success' | 'info' | 'danger' | 'warning';
    /**
     * @default 5000
     */
    timeout?: number;
    link?: {
        label: string;
        url: string;
        target?: string;
    }
}

interface ModalOptions {
    type: 'modal';
    title: string;
    content: React.ReactNode;
    footer?: React.ReactNode;
    /**
     * @default 'L'
     */
    size?: 'S' | 'M' | 'L';
}
```

### `addDocumentHeaderAction`

:::warning 사용 중단됨
`addDocumentHeaderAction`은 Strapi 5에서 사용 중단되었습니다. 대신 [`addDocumentAction`](#adddocumentaction)을 사용하고 `position: 'header'`를 설정하세요.
:::

이는 편집 보기 헤더에 새 액션을 추가하는 데 사용됩니다. [`addDocumentAction`](#adddocumentaction)과 동일한 방식으로 작동하지만 `position` 옵션은 무시되고 항상 헤더에 추가됩니다.

```jsx
addDocumentHeaderAction(actions: DescriptionReducer<DocumentActionComponent> | DocumentActionComponent[])
```

### `addBulkAction`

이는 콘텐츠 매니저의 목록 보기에 새로운 대량 액션을 추가하는 데 사용됩니다:

![addBulkAction](/img/assets/content-manager-apis/add-bulk-action.png)

```jsx
addBulkAction(actions: DescriptionReducer<BulkActionComponent> | BulkActionComponent[])
```

#### BulkActionDescription

API의 인터페이스는 다음과 같습니다:

```jsx
interface BulkActionDescription {
    label: string;
    onClick: (
        event: React.SyntheticEvent,
        documents: Document[]
    ) => Promise<boolean | void> | boolean | void;
    icon?: React.ReactNode;
    /**
     * @default false
     */
    disabled?: boolean;
    dialog?: DialogOptions | NotificationOptions | ModalOptions;
    /**
     * @default 'secondary'
     */
    variant?: ButtonProps['variant'];
}
```
