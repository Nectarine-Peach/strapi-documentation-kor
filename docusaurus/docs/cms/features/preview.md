---
title: 미리보기
description: 미리보기 기능을 통해 Content Manager에서 프론트엔드 결과를 바로 확인할 수 있습니다.
displayedSidebar: userSidebar
toc_max_heading_level: 4
tags:
- 콘텐츠 매니저
- 미리보기
- 기능
---

# 미리보기

미리보기 기능을 사용하면 Strapi 관리자 패널에서 프론트엔드 애플리케이션을 직접 미리 볼 수 있습니다. 이 기능은 Content Manager의 편집 뷰에서 콘텐츠를 수정할 때, 실제 결과가 어떻게 보일지 바로 확인하는 데 유용합니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능<br/>Live Preview는 CMS Growth 및 Enterprise 플랜에서만 제공</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">Roles > Plugins - Users & Permissions에서 읽기 권한 필요</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">`config/admin` 파일에서 설정 필요</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

<ThemedImage
  alt="콘텐츠 미리보기 화면"
  sources={{
    light: '/img/assets/content-manager/previewing-content.png',
    dark: '/img/assets/content-manager/previewing-content.png',
  }}
/>

## 설정

:::note 참고
* 아래 환경 변수들을 `.env` 파일에 정의해야 하며, 예시 값을 실제 환경에 맞게 변경하세요:

  ```bash
  CLIENT_URL=https://your-frontend-app.com
  PREVIEW_SECRET=your-secret-key
  ```

  The `PREVIEW_SECRET` key is optional but required with Next.js draft mode.

* A front-end application for your Strapi project should be already created and set up.
:::

### 구성 요소

미리보기 기능 구성은 [`config/admin` 파일](/cms/configurations/admin-panel)의 `preview` 객체에 저장되며 3가지 주요 구성 요소로 구성됩니다:

#### 활성화 플래그

미리보기 기능을 활성화하거나 비활성화합니다:
```javascript title="config/admin.ts|js" {3}
// …
preview: {
  enabled: true,
    // …
}
// …
```

#### 허용된 원본

미리보기에 접근할 수 있는 도메인을 제어합니다:

```javascript title="config/admin.ts|js" {5}
// …
preview: {
  enabled: true,
  config: {
    allowedOrigins: env("CLIENT_URL"),  // 일반적으로 프론트엔드 애플리케이션 URL
    // …
  }
}
// …
```

#### 미리보기 핸들러

미리보기 로직과 URL 생성을 관리합니다. 다음은 `uid`가 콘텐츠 타입 식별자(예: `api::article.article` 또는 `plugin::my-api.my-content-type`)인 기본 예시입니다:

```jsx title="config/admin.ts|js" {6-11}
// …
preview: {
  enabled: true,
  config: {
    // …
    async handler(uid, { documentId, locale, status }) {
      const document = await strapi.documents(uid).findOne({ documentId });
      const pathname = getPreviewPathname(uid, { locale, document });

      return `${env('PREVIEW_URL')}${pathname}`
    },
  }
}
// …
```

[URL 생성 로직](#2-add-url-generation)의 예시는 다음 기본 구현 가이드에서 제공됩니다.

##### 초안 항목 미리보기

프론트엔드 애플리케이션이 초안 또는 게시된 콘텐츠를 쿼리하는 전략은 프레임워크별로 다릅니다. 최소 3가지 전략이 존재합니다:

- `/your-path?preview=true`와 같은 쿼리 매개변수 사용 (예를 들어, <ExternalLink to="https://nuxt.com/docs/api/composables/use-preview-mode" text="Nuxt"/>가 작동하는 방식)
- `/preview?path=your-path`와 같은 전용 미리보기 경로로 리디렉션 (예를 들어, <ExternalLink to="https://nextjs.org/docs/app/building-your-application/configuring/draft-mode" text="Next의 draft mode"/>가 작동하는 방식)
- 또는 `preview.mysite.com/your-path`와 같은 미리보기용 다른 도메인 사용

콘텐츠 타입에 [초안 및 게시](/cms/features/draft-and-publish)가 활성화된 경우, 다음과 같은 일반적인 접근 방식을 사용하여 미리보기 핸들러 내에서 Strapi의 `status` 매개변수를 직접 활용할 수 있습니다:

```javascript
async handler(uid, { documentId, locale, status }) {
   const document = await strapi.documents(uid).findOne({ documentId });
   const pathname = getPreviewPathname(uid, { locale, document });
   if (status === 'published')  { 
      // 게시된 버전 반환
   }
   // 초안 버전 반환
},
```

Next.js의 draft mode를 사용하는 더 자세한 예시는 [기본 구현 가이드](#3-add-handler)에서 제공됩니다.

### 기본 구현 가이드

콘텐츠 타입에 미리보기 기능을 추가하려면 다음 단계를 따르세요.

#### 1. [Strapi] 미리보기 구성 생성 {#1-create-config}

다음 기본 구조로 새 파일 `/config/admin.ts`를 생성하거나 (이미 존재하는 경우 업데이트) 하세요:

```javascript title="config/admin.ts"
export default ({ env }) => ({
  // 기타 관리자 관련 구성이 여기에 들어갑니다
  // (docs.strapi.io/cms/configurations/admin-panel 참고)
  preview: {
    enabled: true,
    config: {
      allowedOrigins: env('CLIENT_URL'),
      async handler (uid, { documentId, locale, status }) => {
        // 핸들러 구현은 3단계에서 진행
      },
    },
  },
});
```

#### 2. [Strapi] URL 생성 로직 추가 {#2-add-url-generation}

`getPreviewPathname` 함수로 URL 생성 로직을 추가합니다. 다음 예시는 <ExternalLink to="https://github.com/strapi/LaunchPad/tree/feat/preview" text="Launchpad"/> Strapi 데모 애플리케이션에서 가져온 것입니다:

```typescript title="config/admin.ts"
// 콘텐츠 타입과 문서를 기반으로 미리보기 경로명을 생성하는 함수
const getPreviewPathname = (uid, { locale, document }): string => {
  const { slug } = document;
  
  // 특정 URL 패턴을 가진 다양한 콘텐츠 타입 처리
  switch (uid) {
    // 미리 정의된 경로를 가진 페이지 처리
    case "api::page.page":
      switch (slug) {
        case "homepage":
          return `/${locale}`; // 지역화된 홈페이지
        case "pricing":
          return "/pricing"; // 가격 페이지
        case "contact":
          return "/contact"; // 연락처 페이지
        case "faq":
          return "/faq"; // FAQ 페이지
      }
    // 제품 페이지 처리
    case "api::product.product": {
      if (!slug) {
        return "/products"; // 제품 목록 페이지
      }
      return `/products/${slug}`; // 개별 제품 페이지
    }
    // 블로그 기사 처리
    case "api::article.article": {
      if (!slug) {
        return "/blog"; // 블로그 목록 페이지
      }
      return `/blog/${slug}`; // 개별 기사 페이지
    }
    default: {
      return null;
    }
  }
};

// … 메인 export (3단계 참고)
```

:::note
일부 콘텐츠 타입은 의미가 없다면 미리보기가 필요하지 않으므로, 기본 케이스에서 `null`을 반환합니다. 예를 들어, 일부 사이트 메타데이터를 가진 Global 단일 타입은 일치하는 프론트엔드 페이지가 없을 것입니다. 이러한 경우 핸들러 함수는 `null`을 반환해야 하며, 관리자 패널에서 미리보기 UI가 표시되지 않습니다. 이것이 콘텐츠 타입별로 미리보기를 활성화하거나 비활성화하는 방법입니다.
:::

#### 3. [Strapi] 핸들러 로직 추가 {#3-add-handler}

Create the complete configuration, expanding the basic configuration created in step 1. with the URL generation logic created in step 2., adding an appropriate handler logic:

```typescript title="config/admin.ts" {8-9,18-35}
const getPreviewPathname = (uid, { locale, document }): string => {
  // … as defined in step 2
};

// Main configuration export
export default ({ env }) => {
  // Get environment variables
  const clientUrl = env("CLIENT_URL"); // Frontend application URL
  const previewSecret = env("PREVIEW_SECRET"); // Secret key for preview authentication

  return {
    // Other admin-related configurations go here
    // (see docs.strapi.io/cms/configurations/admin-panel)
    preview: {
      enabled: true, // Enable preview functionality
      config: {
        allowedOrigins: clientUrl, // Restrict preview access to specific domain
        async handler(uid, { documentId, locale, status }) {
          // Fetch the complete document from Strapi
          const document = await strapi.documents(uid).findOne({ documentId });
          
          // Generate the preview pathname based on content type and document
          const pathname = getPreviewPathname(uid, { locale, document });

          // Disable preview if the pathname is not found
          if (!pathname) {
            return null;
          }

          // Use Next.js draft mode passing it a secret key and the content-type status
          const urlSearchParams = new URLSearchParams({
            url: pathname,
            secret: previewSecret,
            status,
          });
          return `${clientUrl}/api/preview?${urlSearchParams}`;
        },
      },
    },
  };
};

```
#### 4. [Front end] Set up the front-end preview route {#4-setup-frontend-route}

Setting up the front-end preview route is highly dependent on the framework used for your front-end application.

For instance, <ExternalLink to="https://nextjs.org/docs/app/building-your-application/configuring/draft-mode" text="Next.js draft mode"/> and
<ExternalLink to="https://nuxt.com/docs/api/composables/use-preview-mode" text="Nuxt preview mode"/> provide additional documentation on how to implement the front-end part in their respective documentations.

If using Next.js, a basic implementation could be like in the following example taken from the <ExternalLink to="https://github.com/strapi/LaunchPad/tree/feat/preview" text="Launchpad"/> Strapi demo application:

```typescript title="/next/api/preview/route.ts"
import { draftMode } from "next/headers";
import { redirect } from "next/navigation";

export async function GET(request: Request) {
  // Parse query string parameters
  const { searchParams } = new URL(request.url);
  const secret = searchParams.get("secret");
  const url = searchParams.get("url");
  const status = searchParams.get("status");

  // Check the secret and next parameters
  // This secret should only be known to this route handler and the CMS
  if (secret !== process.env.PREVIEW_SECRET) {
    return new Response("Invalid token", { status: 401 });
  }

  // Enable Draft Mode by setting the cookie
  if (status === "published") {
    draftMode().disable();
  } else {
    draftMode().enable();
  }

  // Redirect to the path from the fetched post
  // We don't redirect to searchParams.slug as that might lead to open redirect vulnerabilities
  redirect(url || "/");
}
```

#### 5. [Front end] Allow the front-end to be embedded {#5-allow-frontend-embed}

On the Strapi side, [the `allowedOrigins` configuration parameter](#allowed-origins) allows the admin panel to load the front-end window in an iframe. But allowing the embedding works both ways, so on the front-end side, you also need to allow the window to be embedded in Strapi's admin panel.

This requires the front-end application to have its own header directive, the CSP `frame-ancestors` directive. Setting this directive up depends on how your website is built. For instance, setting this up in Next.js requires a middleware configuration (see [Next.js docs](https://nextjs.org/docs/app/building-your-application/configuring/content-security-policy)).

#### 6. [Front end] Detect changes in Strapi and refresh the front-end {#6-refresh-frontend}

Strapi emits a `strapiUpdate` message to inform the front end that data has changed. 

To track this, within your front-end application, add an event listener to listen to events posted through [the `postMessage()` API](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) on the `window` object. The listener needs to filter through messages and react only to Strapi-initiated messages, then refresh the iframe content.

With Next.js, the recommended way to refresh the iframe content is with <ExternalLink to="https://nextjs.org/docs/app/building-your-application/caching#routerrefresh" text="the `router.refresh()` method" />.

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript" >

```tsx title="next/app/path/to/your/front/end/logic.jsx" {6-17}
export default function MyClientComponent({...props}) {
  // …
  const router = useRouter();

  useEffect(() => {
    const handleMessage = async (message) => {
      if (
        // Filters events emitted through the postMessage() API
        message.origin === process.env.NEXT_PUBLIC_API_URL &&
        message.data.type === "strapiUpdate"
      ) { // Recommended way to refresh with Next.js
        router.refresh();
      }
    };

    // Add the event listener
    window.addEventListener("message", handleMessage);

    // Cleanup the event listener on unmount
    return () => {
      window.removeEventListener("message", handleMessage);
    };
  }, [router]);

  // ...
}
```

</TabItem>
<TabItem value="ts" label="TypeScript" >

```tsx title="next/app/path/to/your/front/end/logic.tsx" {6-17}
export default function MyClientComponent({
  //…
  const router = useRouter();

  useEffect(() => {
    const handleMessage = async (message: MessageEvent<any>) => {
      if (
        // Filters events emitted through the postMessage() API
        message.origin === process.env.NEXT_PUBLIC_API_URL &&
        message.data.type === "strapiUpdate"
      ) { // Recommended way to refresh with Next.js
        router.refresh();
      }
    };

    // Add the event listener
    window.addEventListener("message", handleMessage);

    // Cleanup the event listener on unmount
    return () => {
      window.removeEventListener("message", handleMessage);
    };
  }, [router]);

  // …
})
```

</TabItem>

</Tabs>

<details>
<summary>Caching in Next.js:</summary>

In Next.js, [cache persistence](https://nextjs.org/docs/app/building-your-application/caching) may require additional steps. You might need to invalidate the cache by making an API call from the client side to the server, where the revalidation logic will be handled. Please refer to Next.js documentation for details, for instance with the [revalidatePath() method](https://nextjs.org/docs/app/building-your-application/caching#revalidatepath).
<br/>

</details>

#### [Front end] Next steps

Once the preview system is set up, you need to adapt your data fetching logic to handle draft content appropriately. This involves the following steps:

1. Create or adapt your data fetching utility to check if draft mode is enabled
2. Update your API calls to include the draft status parameter when appropriate

The following, taken from the <ExternalLink to="https://github.com/strapi/LaunchPad/tree/feat/preview" text="Launchpad" /> Strapi demo application, is an example of how to implement draft-aware data fetching in your Next.js front-end application:

```typescript {8-18}
import { draftMode } from "next/headers";
import qs from "qs";

export default async function fetchContentType(
  contentType: string,
  params: Record = {}
): Promise {
  // Check if Next.js draft mode is enabled
  const { isEnabled: isDraftMode } = draftMode();
  
  try {
    const queryParams = { ...params };
    // Add status=draft parameter when draft mode is enabled
    if (isDraftMode) {
      queryParams.status = "draft";
    }
    
    const url = `${baseURL}/${contentType}?${qs.stringify(queryParams)}`;
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(
        `Failed to fetch data from Strapi (url=${url}, status=${response.status})`
      );
    }
    return await response.json();
  } catch (error) {
    console.error("Error fetching content:", error);
    throw error;
  }
}
```

This utility method can then be used in your page components to fetch either draft or published content based on the preview state:

```typescript
// In your page component:
const pageData = await fetchContentType('api::page.page', {
  // Your other query parameters
});
```

## 사용법

**기능 사용 경로:** <Icon name="feather" /> 콘텐츠 매니저, 콘텐츠 타입의 편집 뷰

:::strapi 미리보기 vs. 라이브 미리보기
CMS 요금제에 따라 미리보기 경험이 다릅니다:
- 무료 요금제에서는 미리보기가 전체 화면으로만 제공됩니다.
- <GrowthBadge /> 및 <EnterpriseBadge /> 요금제에서는 라이브 미리보기에 접근할 수 있습니다. 라이브 미리보기를 사용하면 콘텐츠 매니저의 편집 뷰와 함께 미리보기를 볼 수 있어, 콘텐츠를 편집하면서 동시에 미리보기할 수 있습니다.
:::

미리보기 기능이 올바르게 설정되면 [콘텐츠 매니저의 편집 뷰](/cms/features/content-manager#overview) 오른쪽에 **미리보기 열기** 버튼이 표시됩니다. 이를 클릭하면 프론트엔드 애플리케이션에서 나타날 콘텐츠의 미리보기가 Strapi 관리자 패널 내에서 직접 표시됩니다.

<!-- TODO: add a dark mode GIF -->
<ThemedImage
  alt="콘텐츠 미리보기"
  sources={{
    light: '/img/assets/content-manager/previewing-content2.gif',
    dark: '/img/assets/content-manager/previewing-content2.gif',
  }}
/>

미리보기가 열리면 다음을 할 수 있습니다:

- 왼쪽 상단의 닫기 버튼 <Icon name="x" classes="ph-bold" />을 클릭하여 콘텐츠 매니저의 편집 뷰로 돌아갑니다,
- 초안과 게시된 버전 간의 미리보기를 전환합니다 (콘텐츠 타입에 [초안 및 게시](/cms/features/draft-and-publish)가 활성화된 경우),
- 오른쪽 상단의 링크 아이콘 <Icon name="link" classes="ph-bold"/>을 클릭하여 미리보기 링크를 복사합니다. 현재 보고 있는 미리보기 탭에 따라 초안 또는 게시된 버전의 미리보기 링크가 복사됩니다.

또한 라이브 미리보기에서는 다음을 할 수 있습니다:
- <GrowthBadge /> 및 <EnterpriseBadge /> 요금제에서 <Icon name="arrow-line-left" classes="ph-bold" /> 버튼을 클릭하여 사이드 패널을 확장하고 미리보기를 전체 화면으로 만들 수 있습니다,
- <EnterpriseBadge /> 요금제에서는 활성화된 경우 [검토 워크플로 기능](/cms/features/review-workflows)의 담당자 및 단계를 정의하기 위해 에디터 오른쪽 상단의 버튼을 사용할 수 있습니다.

:::note
콘텐츠 매니저의 편집 뷰에서 저장되지 않은 변경사항이 있으면 미리보기 열기 버튼이 비활성화됩니다. 최신 변경사항을 저장하면 다시 콘텐츠를 미리보기할 수 있습니다.
:::
