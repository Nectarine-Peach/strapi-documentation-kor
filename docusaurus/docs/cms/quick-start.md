---
sidebar_label: '빠른 시작 가이드'
displayed_sidebar: cmsSidebar
sidebar_position: 2
title: 빠른 시작 가이드 - Strapi 개발자 문서
description: 오픈소스 헤드리스 CMS인 Strapi를 3분 이내에 설치하고 실행하는 방법을 안내합니다.
tags:
 - 가이드
 - 콘텐츠 타입 빌더
 - 컬렉션 타입
 - 콘텐츠 매니저
 - Strapi Cloud
---

import InstallPrerequisites from '/docs/snippets/installation-prerequisites.md'

# 빠른 시작 가이드

Strapi는 매우 유연합니다. 빠르게 결과를 보고 싶거나, 제품을 깊이 있게 탐구하고 싶을 때 모두 적합합니다. 이 튜토리얼에서는 직접 프로젝트와 콘텐츠 구조를 처음부터 만들어보고, 이후 Strapi Cloud에 배포하여 데이터를 추가하는 과정을 안내합니다.

*예상 소요 시간: 5~10분*

:::prerequisites
<InstallPrerequisites components={props.components} />

또한, 프로젝트를 Strapi Cloud에 배포하려면 <ExternalLink to="https://github.com/git-guides/install-git" text="`git` 설치"/>와 <ExternalLink to="https://github.com" text="GitHub 계정"/>이 필요합니다.
:::

## <Icon name="rocket-launch"/> A단계: Strapi로 새 프로젝트 생성하기

먼저 터미널에서 명령어를 실행해 Strapi 프로젝트를 생성하고, 첫 번째 로컬 관리자 계정을 등록합니다.

아래 단계별로 토글을 클릭해 자세한 설명을 확인하세요.

<details open>
<summary>1단계: 설치 스크립트 실행 및 Strapi Cloud 계정 생성</summary>

### 1단계: 설치 스크립트 실행 및 Strapi Cloud 계정 생성

1. 터미널에서 다음 명령어를 실행하세요:

    <TabItem value="npm" label="NPM">

    ```bash
    npx create-strapi@latest my-strapi-project
    ```

    </TabItem>

2. 터미널에서 로그인 또는 회원가입을 하라는 메시지가 표시됩니다. 진행하면 30일 무료 <GrowthBadge tooltip="CMS Growth 플랜에는 라이브 프리뷰, 릴리즈, 콘텐츠 히스토리 기능이 포함됩니다."/> 플랜이 자동 적용됩니다. 터미널에서 `Login/Sign up`이 선택되어 있는지 확인하고, Enter를 누르세요.

3. 새로 열린 브라우저 탭에서 터미널에 표시된 인증 코드와 동일한지 확인 후 **Confirm**을 클릭하세요.

4. 브라우저 탭에서 **Continue with GitHub**을 클릭합니다. 이미 GitHub에 로그인되어 있지 않다면 로그인 페이지로 이동할 수 있습니다.

5. 로그인 후 "Congratulations, you're all set!" 메시지가 나오면 브라우저 탭을 닫고 터미널로 돌아오세요.

    <ThemedImage
      alt="로그인 GIF"
      sources={{
        light: '/img/assets/quick-start-guide/qsg-cloud-login.gif',
        dark: '/img/assets/quick-start-guide/qsg-cloud-login.gif',
      }}
    />

6. 터미널에서 몇 가지 질문이 나옵니다. 모두 기본값(Enter)을 입력해도 됩니다.

    ![터미널 질문과 답변](/img/assets/quick-start-guide/qsg-questions-answers-terminal.png)

터미널에서 프로젝트가 로컬에 생성되고 있음을 확인할 수 있습니다.

:::info

* 프로젝트 폴더에는 로컬 Strapi 프로젝트와 Strapi 서버를 연결하는 `.strapi-cloud.json` 파일이 포함됩니다.
* 더 다양한 설치 옵션은 [설치 문서](/cms/installation)에서 확인할 수 있습니다.
:::

</details>

<details>
<summary>2단계: 첫 번째 로컬 관리자 계정 등록</summary>

### 2단계: 첫 번째 로컬 관리자 계정 등록

설치가 완료되면 서버를 시작해야 합니다. 터미널에서 `cd my-strapi-project && yarn develop`을 입력하면 브라우저가 자동으로 새 탭을 엽니다.

:::tip
`my-strapi-project` 폴더 내에서 작업할 때는 언제든 `yarn develop` 명령어로 Strapi 서버를 다시 시작할 수 있습니다.
:::

폼을 작성하면 본인 계정이 생성되며, 이 Strapi 애플리케이션의 첫 번째 관리자 사용자가 됩니다. 환영합니다!

이제 <ExternalLink to="http://localhost:1337/admin" text="관리자 패널"/>에 접근할 수 있습니다:

<ThemedImage
alt="관리자 패널 스크린샷: 대시보드"
sources={{
    light: '/img/assets/quick-start-guide/qsg-handson-part1-01-admin_panel-v5.png',
    dark: '/img/assets/quick-start-guide/qsg-handson-part1-01-admin_panel-v5_DARK.png',
}}
/> 

</details>

:::callout <Icon name="confetti" /> 축하합니다!
새로운 Strapi 프로젝트가 생성되었습니다! 이제 Strapi를 직접 사용해보거나, 아래 B단계로 진행해보세요.
:::

## <Icon name="wrench" /> B단계: 콘텐츠 타입 빌더로 콘텐츠 구조 만들기

설치 스크립트로 빈 프로젝트가 생성되었습니다. 이제 [FoodAdvisor](https://github.com/strapi/foodadvisor) 예제 앱을 참고해 음식점 디렉터리를 만들어보겠습니다.

로컬 Strapi 프로젝트의 관리자 패널은 <ExternalLink to="http://localhost:1337/admin" text="http://localhost:1337/admin"/>에서 실행됩니다. 이곳에서 대부분의 콘텐츠 생성 및 수정 작업을 하게 됩니다.

먼저 콘텐츠 구조를 만들어봅니다. 이 작업은 개발 모드(로컬 프로젝트의 기본 모드)에서만 가능합니다.

:::tip
서버가 실행 중이 아니라면, 터미널에서 `my-strapi-project` 폴더로 이동 후 `npm run develop`(또는 `yarn develop`)을 실행하세요.
:::

콘텐츠 타입 빌더는 콘텐츠 구조를 만드는 데 도움을 줍니다. Strapi로 빈 프로젝트를 만들었다면, 여기서부터 시작하세요!

<details >

<summary>1단계: "Restaurant" 컬렉션 타입 생성</summary>

### 1단계: "Restaurant" 컬렉션 타입 생성

음식점 디렉터리에는 여러 음식점이 들어가야 하므로, "Restaurant" 컬렉션 타입을 만듭니다. 이후 음식점 추가 시 표시할 필드를 정의합니다:

1. **Create your first Content type** 버튼을 클릭하세요.<br />버튼이 보이지 않으면, 좌측 메뉴에서 <Icon name="layout" /> <ExternalLink to="http://localhost:1337/admin/plugins/content-type-builder" text="Content-Type Builder"/>로 이동하세요.
2. **Create new collection type**을 클릭합니다.
3. _Display name_에 `Restaurant`을 입력하고 **Continue**를 클릭합니다.  
4. Text 필드를 클릭합니다.
5. _Name_ 필드에 `Name`을 입력합니다.
6. _Advanced Settings_ 탭으로 이동해 **Required field**와 **Unique field**를 체크합니다.
7. **Add another field**를 클릭합니다.
8. 목록에서 Rich text (Blocks) 필드를 선택합니다.
9. _Name_ 필드에 `Description`을 입력한 후 **Finish**를 클릭합니다.
10. 마지막으로 **Save**를 클릭하고, Strapi가 재시작될 때까지 기다립니다.

<ThemedImage
alt="GIF: Content-type Builder에서 Restaurant 컬렉션 타입 생성"
sources={{
    light: '/img/assets/quick-start-guide/qsg-handson-restaurant-v5.gif',
    dark: '/img/assets/quick-start-guide/qsg-handson-restaurant-v5_DARK.gif',
}}
/>

Strapi가 재시작되면, 좌측 메뉴의 <Icon name="feather" /> _Content Manager > Collection types_에 "Restaurant"가 표시됩니다. 첫 번째 콘텐츠 타입 생성 완료! 이제 바로 또 하나 만들어볼까요?

</details>

<details>
<summary>2단계: "Category" 컬렉션 타입 생성</summary>

### 2단계: "Category" 컬렉션 타입 생성

음식점 디렉터리를 더 체계적으로 관리하려면 카테고리가 필요합니다. "Category" 컬렉션 타입을 만들어봅시다:

1. 좌측 메뉴에서 <Icon name="layout" /> <ExternalLink to="http://localhost:1337/admin/plugins/content-type-builder" text="Content-type Builder"/>로 이동합니다.
2. **Create new collection type**을 클릭합니다.
3. _Display name_에 `Category`를 입력하고 **Continue**를 클릭합니다.
4. Text 필드를 클릭합니다.
5. _Name_ 필드에 `Name`을 입력합니다.
6. _Advanced Settings_ 탭으로 이동해 **Required field**와 **Unique field**를 체크합니다.
7. **Add another field**를 클릭합니다.
8. Relation 필드를 선택합니다.
9. 가운데에서 "many-to-many"를 나타내는 아이콘을 선택합니다. ![many-to-many 아이콘](/img/assets/icons/v5/ctb_relation_manytomany.svg) 텍스트는 `Categories has and belongs to many Restaurants`로 표시됩니다.

<ThemedImage
alt="관리자 패널 스크린샷: 관계 설정"
sources={{
  light: '/img/assets/quick-start-guide/qsg-handson-part2-02-collection_ct-v5.png',
  dark: '/img/assets/quick-start-guide/qsg-handson-part2-02-collection_ct-v5_DARK.png',
}}
/>

11. 마지막으로 **Finish**를 클릭한 후 **Save** 버튼을 눌러 Strapi가 재시작될 때까지 기다립니다.

</details>

:::callout <Icon name="confetti" /> 축하합니다!
Strapi 프로젝트의 기본 콘텐츠 구조가 완성되었습니다! [콘텐츠 타입 빌더](/cms/features/content-type-builder)로 더 실험해보거나, 아래 C, D단계로 진행해보세요.
:::

## <Icon name="cloud" />️ C단계: Strapi Cloud에 배포하기

이제 멋진 Strapi 프로젝트가 로컬에서 동작하니, 전 세계에 공개해볼 차례입니다! Strapi Cloud를 이용하면 한 번의 명령어로 프로젝트를 배포할 수 있습니다. 🚀

Strapi Cloud에 무료로 배포하려면 터미널에서 다음을 실행하세요:

1. 로컬 Strapi 프로젝트 서버가 실행 중이라면, `Ctrl-C`로 서버를 중지하세요.
2. Strapi 프로젝트 폴더에 있는지 확인하고(필요하다면 `cd my-strapi-project`로 이동), 아래 명령어를 실행하세요:

    <Tabs groupId="yarn-npm">

    <TabItem value="yarn" label="Yarn">

      ```sh
      yarn strapi deploy
      ```

    </TabItem>

    <TabItem value="npm" label="NPM">

      ```sh
      npm run strapi deploy
      ```

    </TabItem>

    </Tabs>

3. 터미널에서 프로젝트 이름(Enter로 기본값 사용 가능), 권장 NodeJS 버전, 가까운 리전을 선택하세요:

    ![Strapi Cloud 터미널 질문과 답변](/img/assets/quick-start-guide/qsg-strapi-cloud-terminal-questions.png)

잠시 후, 로컬 프로젝트가 Strapi Cloud에 배포됩니다. 🚀 

완료되면 터미널에 `https://cloud.strapi.io/projects`로 시작하는 링크가 표시됩니다. 클릭하거나 복사해 브라우저에 입력하면, Strapi Cloud 대시보드에서 방금 만든 `my-strapi-project`를 확인할 수 있습니다. 우측 상단 **Visit app** 버튼을 클릭해 배포된 프로젝트에 접속하세요.

<ThemedImage
alt="Strapi Cloud 앱 방문 GIF"
sources={{
  light: '/img/assets/quick-start-guide/qsg-visit-cloud-app.gif',
  dark: '/img/assets/quick-start-guide/qsg-visit-cloud-app_DARK.gif',
}}
/>

:::callout <Icon name="confetti" /> 축하합니다!
이제 프로젝트가 Strapi Cloud에 배포되어 온라인에서 접근할 수 있습니다. [Cloud 전용 문서](/cloud/intro)도 참고하거나, D단계로 넘어가 온라인 프로젝트에 데이터를 추가해보세요.
:::

:::tip
콘텐츠 타입 빌더로 더 많은 필드를 추가하거나 새로운 콘텐츠 타입을 만들어보세요. 변경 사항이 있을 때마다 `deploy` 명령어로 다시 배포하면, 몇 분 내에 온라인 프로젝트가 업데이트됩니다. 정말 마법 같죠? 🪄
:::

## <Icon name="note-pencil" /> D단계: 콘텐츠 매니저로 Strapi Cloud 프로젝트에 콘텐츠 추가하기

이제 "Restaurant"와 "Category" 두 컬렉션 타입으로 기본 구조를 만들고, 프로젝트를 Strapi Cloud에 배포했습니다. 이제 실제로 데이터를 추가해보겠습니다.

<details>
<summary>1단계: Strapi Cloud 프로젝트의 관리자 패널 로그인</summary>

### 1단계: Strapi Cloud 프로젝트의 관리자 패널 로그인

Strapi Cloud 프로젝트가 생성되었으니, 로그인해봅시다:

1. <ExternalLink to="https://cloud.strapi.io/projects" text="Strapi Cloud 대시보드"/>에서 `my-strapi-project`를 클릭하세요.
2. **Visit app** 버튼을 클릭합니다.
3. 새로 열린 페이지에서 첫 번째 관리자 계정을 생성하세요.

로그인 후, 이제 Strapi Cloud 프로젝트에 데이터를 추가할 수 있습니다.

<ThemedImage
alt=""
sources={{
  light: '/img/assets/quick-start-guide/qsg-first-login-cloud.gif',
  dark: '/img/assets/quick-start-guide/qsg-first-login-cloud_DARK.gif'
}}
/>

<details>
<summary><Icon name="info" /> 사용자 및 Strapi Cloud 프로젝트 관련 추가 정보와 팁</summary>

:::note 참고: 로컬 사용자와 Strapi Cloud 사용자는 다릅니다
Strapi Cloud 프로젝트와 로컬 프로젝트의 데이터베이스는 서로 다릅니다. 즉, 로컬에서 만든 데이터(사용자 포함)는 자동으로 Cloud로 이전되지 않습니다. 처음 Cloud 프로젝트에 로그인할 때 새 관리자 계정을 만드는 이유입니다.
:::

:::tip 팁: Strapi Cloud 프로젝트의 관리자 패널 바로 접근하기
Strapi Cloud에 배포된 프로젝트는 `https://my-strapi-project-name.strapiapp.com`과 같은 고유 URL로 접근할 수 있습니다. 온라인 프로젝트의 관리자 패널은 URL 뒤에 `/admin`을 붙이면 됩니다(예: `https://my-strapi-project-name.strapiapp.com/admin`). URL은 Cloud 대시보드에서 확인할 수 있으며, 프로젝트 이름 클릭 후 **Visit app** 버튼으로도 바로 이동할 수 있습니다.
:::

</details>

</details>

<details>
<summary>2단계: "Restaurant" 컬렉션 타입에 엔트리 추가</summary>

### 2단계: "Restaurant" 컬렉션 타입에 엔트리 추가

1. 좌측 메뉴에서 <Icon name="feather" /> _Content Manager > Collection types - Restaurant_로 이동합니다.
2. **Create new entry**를 클릭합니다.
3. _Name_ 필드에 좋아하는 음식점 이름을 입력하세요. 예: `Biscotte Restaurant`.
4. _Description_ 필드에 간단한 소개를 작성하세요. 예시: `Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers.`
5. **Save**를 클릭합니다.

<ThemedImage
alt="스크린샷: Biscotte Restaurant 등록"
sources={{
  light: '/img/assets/quick-start-guide/qsg-handson-part2-03-restaurant-v5.png',
  dark: '/img/assets/quick-start-guide/qsg-handson-part2-03-restaurant-v5_DARK.png',
}}
/>

이제 _Collection types - Restaurant_ 뷰에 음식점이 등록됩니다.

</details>

<details>
<summary>3단계: 카테고리 추가</summary>

#### 3단계: 카테고리 추가

<Icon name="feather" /> _Content Manager > Collection types - Category_로 이동해 2개의 카테고리를 만들어봅시다:

1. **Create new entry**를 클릭합니다.
2. _Name_ 필드에 `French Food`를 입력합니다.
3. **Save**를 클릭합니다.
4. _Collection types - Category_로 돌아가 **Create new entry**를 다시 클릭합니다.  
5. _Name_ 필드에 `Brunch`를 입력하고 **Save**를 클릭합니다.

<ThemedImage
alt="GIF: 카테고리 추가"
sources={{
  light: '/img/assets/quick-start-guide/qsg-handson-categories-v5.gif',
  dark: '/img/assets/quick-start-guide/qsg-handson-categories-v5_DARK.gif',
}}/>

"French Food"와 "Brunch" 카테고리가 _Collection types - Category_ 뷰에 등록됩니다.

이제 음식점에 카테고리를 추가해봅시다:

1. <Icon name="feather" /> _Content Manager > Collection types - Restaurant_로 이동해 "Biscotte Restaurant"를 클릭합니다.
2. 페이지 하단의 **Categories** 드롭다운에서 "French Food"를 선택합니다. 페이지 상단으로 돌아가 **Save**를 클릭하세요.

</details>

<details>
<summary>4단계: 역할 및 권한 설정</summary>

### 4단계: 역할 및 권한 설정

음식점과 카테고리를 추가했으니, 이제 API를 통해 외부에서 접근할 수 있도록 공개 권한을 설정해야 합니다:

1. 좌측 하단 _<Icon name="gear-six" /> Settings_를 클릭합니다.
2. _Users & Permissions Plugin_에서 _Roles_를 선택합니다.
3. **Public** 역할을 클릭합니다.
4. _Permissions_ 아래로 스크롤합니다.
5. _Permissions_ 탭에서 _Restaurant_를 찾아 클릭합니다.
6. **find**와 **findOne** 체크박스를 선택합니다.
7. _Category_도 동일하게 **find**와 **findOne**을 체크합니다.
8. 마지막으로 **Save**를 클릭합니다.

<ThemedImage
alt="스크린샷: Users & Permissions 플러그인에서 Public 역할"
sources={{
  light: '/img/assets/quick-start-guide/qsg-handson-part2-04-roles-v5.png',
  dark: '/img/assets/quick-start-guide/qsg-handson-part2-04-roles-v5_DARK.png'
}}/>

</details>

<details>
<summary>5단계: 콘텐츠 발행</summary>

### 5단계: 콘텐츠 발행

기본적으로 생성된 콘텐츠는 초안 상태로 저장됩니다. 카테고리와 음식점을 발행해봅시다.

먼저 <Icon name="feather" /> _Content Manager > Collection types - Category_로 이동해:

1. "Brunch" 항목을 클릭합니다.
2. 다음 화면에서 **Publish**를 클릭합니다.
3. _Confirmation_ 창에서 **Yes, publish**를 클릭합니다.  

다시 카테고리 목록으로 돌아가 "French Food"도 동일하게 발행하세요.

마지막으로, 음식점도 발행하려면 <Icon name="feather" /> _Content Manager > Collection types - Restaurant_에서 "Biscotte Restaurant"를 클릭한 후 **Publish**를 클릭하세요.

<ThemedImage
alt="GIF: 콘텐츠 발행"
sources={{
  light: '/img/assets/quick-start-guide/qsg-handson-publish-v5.gif',
  dark: '/img/assets/quick-start-guide/qsg-handson-publish-v5_DARK.gif',
}}
/>

</details>

<details>
<summary>6단계: API 사용하기</summary>

### 6단계: API 사용하기

이제 콘텐츠를 생성하고, API를 통해 접근할 수 있게 되었습니다. 수고하셨습니다! 이제 결과를 직접 확인해보세요.

예시: Strapi Cloud 프로젝트 URL의 `/api/restaurants` 경로로 접속하면 음식점 목록을 확인할 수 있습니다(예: `https://beautiful-first-strapi-project.strapiapp.com/api/restaurants`).

아래는 API 응답 예시입니다 👇.

<details>
<summary>API 응답 예시 보기</summary>

```json
{
  "data": [
    {
      "id": 3,
      "documentId": "wf7m1n3g8g22yr5k50hsryhk",
      "Name": "Biscotte Restaurant",
      "Description": [
        {
          "type": "paragraph",
          "children": [
            {
              "type": "text",
              "text": "Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers."
            }
          ]
        }
      ],
      "createdAt": "2024-09-10T12:49:32.350Z",
      "updatedAt": "2024-09-10T13:14:18.275Z",
      "publishedAt": "2024-09-10T13:14:18.280Z",
      "locale": null
    }
  ],
  "meta": {
    "pagination": {
      "page": 1,
      "pageSize": 25,
      "pageCount": 1,
      "total": 1
    }
  }
}
```

</details>

</details>

:::callout <Icon name="confetti"/> 축하합니다!
이제 콘텐츠가 생성·발행되었고, API를 통해 접근할 수 있습니다.
멋진 콘텐츠를 계속 만들어보세요!
:::

:::tip 팁: 로컬과 Strapi Cloud 프로젝트 간 데이터 전송
Strapi Cloud와 로컬 프로젝트의 데이터베이스는 서로 다릅니다. 데이터가 자동으로 동기화되지 않으니, [데이터 관리 시스템](/cms/features/data-management)을 활용해 프로젝트 간 데이터를 전송할 수 있습니다.
:::

## <Icon name="fast-forward"/> 다음 단계는?

이제 Strapi로 콘텐츠를 생성·발행하는 기본 과정을 익혔으니, 아래 기능도 탐구해보세요:

<Icon name="arrow-fat-right"/> Strapi의 [REST](/cms/api/rest) API로 콘텐츠 쿼리하기<br/>
<Icon name="arrow-fat-right"/> <Icon name="backpack" /> **기능** 카테고리에서 다양한 Strapi 기능 살펴보기<br/>
<Icon name="arrow-fat-right"/> [Cloud 전용 문서](/cloud/intro)에서 Strapi Cloud 프로젝트 더 알아보기<br/>
<Icon name="arrow-fat-right"/> [백엔드 커스터마이징](/cms/backend-customization) 및 [관리자 패널 커스터마이징](/cms/admin-panel-customization)으로 고급 활용하기<br/>
