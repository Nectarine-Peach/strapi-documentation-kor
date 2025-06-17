---
title: 관리자 패널 커스터마이징
description: Strapi의 관리자 패널은 필요에 따라 커스터마이징할 수 있어, 브랜드 아이덴티티를 반영할 수 있습니다.
toc_max_heading_level: 4
tags:
- 관리자 패널
- 관리자 패널 커스터마이징
---

import HotReloading from '/docs/snippets/hot-reloading-admin-panel.md'

# 관리자 패널 커스터마이징

**Strapi의 프론트엔드** <Annotation>아래의 구분이 궁금하다면:<ul><li>Strapi 관리자 패널(프론트엔드)</li><li>Strapi 서버(백엔드)</li><li>Strapi 기반 애플리케이션의 엔드유저용 프론트엔드</li></ul> [개발 소개](/cms/customization) 문서를 참고하세요.</Annotation>는 관리자 패널이라고 부릅니다. 관리자 패널은 콘텐츠 API로 접근할 수 있는 콘텐츠를 구조화하고 관리할 수 있도록 그래픽 UI를 제공합니다. 관리자 패널 개요는 [시작하기 > 관리자 패널](/cms/features/admin-panel) 페이지를 참고하세요.

개발자 관점에서 Strapi의 관리자 패널은 React 기반의 싱글 페이지 애플리케이션이며, Strapi 애플리케이션의 모든 기능과 설치된 플러그인을 포함합니다.

관리자 패널 커스터마이징은 `src/admin/app` 파일 또는 `src/admin` 폴더 내의 다른 파일(자세한 구조는 [프로젝트 구조](/cms/project-structure) 참고)을 수정하여 진행합니다. 이를 통해 다음과 같은 작업이 가능합니다:

- 로고, 파비콘, 언어 등 브랜드 아이덴티티에 맞게 일부 UI를 커스터마이징
- 리치 텍스트 에디터, 번들러 등 일부 기능 교체
- 테마 확장 또는 UI 확장으로 새로운 기능 추가 및 기존 UI 커스터마이징

## 일반 고려사항

:::prerequisites
관리자 패널 커스터마이징을 위해 코드를 수정하기 전:

- 기본 `app.example.tsx|js` 파일을 `app.ts|js`로 이름 변경
- `/src/admin/`에 `extensions` 폴더 생성
- 개발 중 변경 사항을 실시간으로 확인하려면, 관리자 패널 서버가 실행 중이어야 합니다(기본 [호스트, 포트, 경로](/cms/configurations/admin-panel#admin-panel-server)를 변경하지 않았다면 `yarn develop` 또는 `npm run develop` 명령어로 실행)
:::

가장 기본적인 커스터마이징은 `/src/admin/app` 파일에서 `config` 객체를 수정하여 진행합니다.

`config` 객체에서 사용하는 파일(예: 커스텀 로고)은 `/src/admin/extensions/` 폴더에 두고, `/src/admin/app.js`에서 import해야 합니다.

<HotReloading />

배포 전에는 프로젝트 루트에서 다음 명령어로 관리자 패널을 빌드해야 합니다:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```sh
yarn build
```

</TabItem>

<TabItem value="npm" label="npm">

```sh
npm run build
```

</TabItem>

</Tabs>

이 명령어는 `./build` 폴더의 내용을 대체합니다. <ExternalLink to="http://localhost:1337/admin" text="http://localhost:1337/admin"/>에서 커스터마이징이 적용되었는지 확인하세요.

:::note 참고: 관리자 패널 확장 vs. 플러그인 확장
Strapi 프로젝트에는 기본적으로 `/src`에 또 다른 `extensions` 폴더가 있는데, 이는 플러그인 확장용입니다([플러그인 확장](/cms/plugins-development/plugins-extension) 참고).
:::

## 커스터마이징 가능한 항목

`/src/admin/app`의 `config` 객체는 다음과 같은 파라미터를 지원합니다:

| 파라미터                      | 타입             | 설명                                                                                                           |
| ------------------------------ | ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| `auth`                         | Object           | 로그인 화면의 기본 Strapi 로고를 교체할 때 `logo` 키 사용                                     |
| `head`                         | Object           | 기본 Strapi 파비콘을 교체할 때 `favicon` 키 사용                                             |
| `locales`                      | 문자열 배열      | 지원할 로케일 정의 |
| `translations`                 | Object           | 번역 확장                                                                   |
| `menu`                         | Object           | 메인 네비게이션의 로고를 변경할 때 `logo` 키 사용                                            |
| `theme.light` 및 `theme.dark`  | Object           | 라이트/다크 모드 테마 속성 덮어쓰기                                               |
| `tutorials`                    | Boolean          | 비디오 튜토리얼 표시 여부
| `notifications`                | Object           | 새 릴리즈 알림 표시 여부(`releases` 키, Boolean) |

아래 카드 중 하나를 클릭하면 각 주제별 상세 문서로 이동합니다:

<CustomDocCardsWrapper>
<CustomDocCard icon="image" title="로고" description="관리자 패널에 표시되는 로고를 브랜드에 맞게 변경하세요." link="/cms/admin-panel-customization/logos" />
<CustomDocCard icon="image" title="파비콘" description="파비콘을 브랜드에 맞게 변경하세요." link="/cms/admin-panel-customization/favicon" />
<CustomDocCard icon="globe" title="로케일 & 번역" description="관리자 패널에서 사용할 로케일 정의 및 번역 확장." link="/cms/admin-panel-customization/locales-translations" />
<CustomDocCard icon="swap" title="리치 텍스트 에디터" description="내장 리치 텍스트 에디터 교체 전략 알아보기." link="/cms/admin-panel-customization/wysiwyg-editor" />
<CustomDocCard icon="package" title="번들러" description="Vite와 webpack 번들러 선택 및 설정 방법." link="/cms/admin-panel-customization/bundlers" />
<CustomDocCard icon="palette" title="테마 확장" description="내장 테마 확장 기본 방법 알아보기." link="/cms/admin-panel-customization/theme-extension" />
<CustomDocCard icon="paint-bucket" title="관리자 패널 확장" description="관리자 패널 확장 기본 방법 알아보기." link="/cms/admin-panel-customization/extension" />
</CustomDocCardsWrapper>

## 기본 예시

아래는 관리자 패널을 간단히 커스터마이징하는 예시입니다:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/src/admin/app.js"
import AuthLogo from "./extensions/my-logo.png";
import MenuLogo from "./extensions/logo.png";
import favicon from "./extensions/favicon.png";

export default {
  config: {
    // 로그인 화면의 Strapi 로고 교체
    auth: {
      logo: AuthLogo,
    },
    // 파비콘 교체
    head: {
      favicon: favicon,
    },
    // 'en' 외에 새로운 로케일 추가
    locales: ["fr", "de"],
    // 메인 네비게이션의 Strapi 로고 교체
    menu: {
      logo: MenuLogo,
    },
    // 테마 덮어쓰기 또는 확장
    theme: {
      // 라이트 테마 속성 덮어쓰기
      light: {
        colors: {
          primary100: "#f6ecfc",
          primary200: "#e0c1f4",
          primary500: "#ac73e6",
          primary600: "#9736e8",
          primary700: "#8312d1",
          danger700: "#b72b1a",
        },
      },

      // 다크 테마 속성 덮어쓰기
      dark: {
        // ...
      },
    },
    // 번역 확장
    translations: {
      fr: {
        "Auth.form.email.label": "test",
        Users: "Utilisateurs",
        City: "CITY (FRENCH)",
        // Content Manager 테이블의 라벨 커스터마이징
        Id: "ID french",
      },
    },
    // 비디오 튜토리얼 비활성화
    tutorials: false,
    // 새 Strapi 릴리즈 알림 비활성화
    notifications: { releases: false },
  },

  bootstrap() {},
};
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```jsx title="/src/admin/app.ts"
// 참고: 일부 ts 오류가 보일 수 있으나 무시해도 됩니다
import AuthLogo from "./extensions/my-logo.png";
import MenuLogo from "./extensions/logo.png";
import favicon from "./extensions/favicon.png";

export default {
  config: {
    // 로그인 화면의 Strapi 로고 교체
    auth: {
      logo: AuthLogo,
    },
    // 파비콘 교체
    head: {
      // 이 설정이 동작하지 않으면 프로젝트 루트의 favicon.png 파일을 변경해보세요.
      favicon: favicon, 
    },
    // 'en' 외에 새로운 로케일 추가
    locales: ["fr", "de"],
    // 메인 네비게이션의 Strapi 로고 교체
    menu: {
      logo: MenuLogo,
    },
    // 테마 덮어쓰기 또는 확장
    theme: {
	    dark:{
	      colors: {
			  alternative100: '#f6ecfc',
			  alternative200: '#e0c1f4',
			  alternative500: '#ac73e6',
			  alternative600: '#9736e8',
			  alternative700: '#8312d1',
			  buttonNeutral0: '#ffffff',
			  buttonPrimary500: '#7b79ff',
			  // 기타 색상은 아래 링크 참고
			  },
		},
		light:{
			// 라이트 테마 색상도 아래 링크에서 확인 가능 https://github.com/strapi/design-system/blob/main/packages/design-system/src/themes/lightTheme/light-colors.ts
		},
  },
    },
    // 번역 확장
    // 번역 키 전체는 https://github.com/strapi/strapi/blob/develop/packages/core/admin/admin/src/translations 참고
    translations: {
      fr: {
        "Auth.form.email.label": "test",
        Users: "Utilisateurs",
        City: "CITY (FRENCH)",
        // Content Manager 테이블의 라벨 커스터마이징
        Id: "ID french",
      },
    },
    // 비디오 튜토리얼 비활성화
    tutorials: false,
    // 새 Strapi 릴리즈 알림 비활성화
    notifications: { releases: false },
  },

  bootstrap() {},
};
```

</TabItem>
</Tabs>

:::strapi 코드베이스의 상세 예시

* 전체 번역 키(예: 환영 메시지 변경)는 [GitHub](https://github.com/strapi/strapi/blob/develop/packages/core/admin/admin/src/translations)에서 확인할 수 있습니다.
* 라이트/다크 테마 색상도 [GitHub](https://github.com/strapi/design-system/tree/main/packages/design-system/src/themes)에서 확인할 수 있습니다.
:::
