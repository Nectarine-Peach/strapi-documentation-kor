---
title: 템플릿
description: 특정 목적에 맞게 설계된 Strapi의 사전 제작 애플리케이션(템플릿)을 사용하거나 직접 만들어보세요.
displayed_sidebar: cmsSidebar
tags:
- 설치
- 템플릿
- CLI
---

# 템플릿

Strapi 5의 템플릿은 특정 목적에 맞게 설계된 독립형 Strapi 애플리케이션입니다.

Strapi 5 템플릿은 일반 Strapi 애플리케이션에서 볼 수 있는 모든 파일과 폴더를 포함하는 폴더입니다(자세한 구조는 [프로젝트 구조](/cms/project-structure) 참고).

## 템플릿 사용하기

템플릿을 기반으로 새 Strapi 프로젝트를 생성하려면 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="Yarn">

```sh
yarn create strapi-app my-project --template <template-name-or-url>
```

</TabItem>

<TabItem value="npm" label="NPM">

```sh
npx create-strapi-app@latest my-project --template <template-name-or-url>
```

</TabItem>

</Tabs>

필수 `--template` 파라미터 외에도, `--template-path`와 `--template-branch` 옵션을 추가로 지정해 템플릿을 더 세밀하게 선택할 수 있습니다.

아래 표는 템플릿을 지정하는 다양한 방법을 보여줍니다:

| 문법 | 설명 |
|--------|-------------|
| `--template website` | <ExternalLink to="https://github.com/strapi/strapi/tree/develop/templates" text="Strapi 공식 템플릿"/> 중 하나를 폴더명으로 지정하여 사용 |
| `--template strapi/strapi` | 템플릿의 GitHub 저장소를 축약형으로 지정(기본 브랜치 사용) |
| `--template strapi/strapi/some/sub/path` | GitHub 저장소 축약형과 하위 경로를 함께 지정(기본 브랜치 사용) |
| `--template strapi/strapi`<br/>`--template-branch=xxx`<br/>`--template-path=some/sub/path` | 브랜치와 하위 경로를 명시적으로 지정하는 가장 상세한 방법 |
| `--template https://github.com/owner/some-template-repo` | 전체 저장소 URL을 사용(기본 브랜치 사용) |
| `--template https://github.com/owner/some-template-repo --template-branch=xxx --template-path=sub/path` | 전체 저장소 URL, 브랜치, 하위 경로를 모두 지정 |
| `--template https://github.com/strapi/strapi/tree/branch/sub/path` | 저장소, 브랜치, 하위 경로를 직접 지정<br/><br/>⚠️ _브랜치명에 `/`가 포함된 경우 동작하지 않을 수 있으니, 이럴 땐 `--template-branch`와 `--template-path`를 명시적으로 지정하세요._ |

## 템플릿 만들기

Strapi 5 템플릿을 만드는 방법은 Strapi 애플리케이션을 만드는 것과 동일합니다. [CLI 설치](/cms/installation/cli) 문서를 참고해 애플리케이션을 생성한 뒤, 해당 폴더 전체를 템플릿으로 사용할 수 있습니다. 이후 새 프로젝트 생성 시 `--template` 플래그에 해당 폴더를 지정하면 템플릿으로 활용할 수 있습니다.

예시로, <ExternalLink to="https://github.com/strapi/strapi/tree/develop/templates/website" text="Strapi 공식 `website` 템플릿"/>을 참고하세요.
