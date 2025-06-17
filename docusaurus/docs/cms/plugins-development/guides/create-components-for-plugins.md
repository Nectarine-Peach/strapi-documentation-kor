---
title: Strapi 플러그인용 컴포넌트 생성하는 방법
description: Strapi 플러그인용 컴포넌트를 생성하고 설정하는 방법을 알아보세요
sidebar_label: 플러그인용 컴포넌트 생성
displayed_sidebar: cmsSidebar
tags:
- 관리자 패널
- 컴포넌트
- 콘텐츠 타입
- 가이드
- 플러그인
- 플러그인 개발 가이드
---

# Strapi 플러그인용 컴포넌트 생성하는 방법

[Strapi 플러그인을 개발](/cms/plugins-development/developing-plugins)할 때 플러그인용 재사용 가능한 컴포넌트를 생성하고 싶을 수 있습니다. Strapi의 컴포넌트는 여러 콘텐츠 타입에서 사용할 수 있는 재사용 가능한 데이터 구조입니다.

Strapi 플러그인용 컴포넌트를 생성하려면 콘텐츠 타입 생성과 유사한 방법을 따라야 하지만 몇 가지 특별한 차이점이 있습니다.

## 컴포넌트 생성

플러그인용 컴포넌트는 2가지 방법으로 생성할 수 있습니다: 콘텐츠 타입 빌더 사용(권장 방법) 또는 수동으로 생성.

### 콘텐츠 타입 빌더 사용

플러그인용 컴포넌트를 생성하는 권장 방법은 관리자 패널의 콘텐츠 타입 빌더를 통하는 것입니다. 
[콘텐츠 타입 빌더 문서](/cms/features/content-type-builder#new-component)에서 이 과정에 대한 자세한 내용을 제공합니다.

### 수동으로 컴포넌트 생성

수동으로 컴포넌트를 생성하는 것을 선호한다면 다음을 수행해야 합니다:

1. 플러그인 구조에서 컴포넌트 스키마를 생성합니다.
2. 컴포넌트가 올바르게 등록되었는지 확인합니다.

플러그인용 컴포넌트는 플러그인 구조 내의 적절한 디렉토리에 배치되어야 합니다. 일반적으로 플러그인의 서버 부분 내에서 생성합니다([플러그인 구조 문서](/cms/plugins-development/plugin-structure) 참고).

Strapi의 컴포넌트에 대한 자세한 정보는 [모델 속성 문서](/cms/backend-customization/models#components-json)를 참고할 수 있습니다.

## 컴포넌트 구조 검토

Strapi의 컴포넌트는 정의에서 다음 형식을 따릅니다:

```javascript title="/my-plugin/server/components/category/component-name.json"
{
  "attributes": {
    "myComponent": {
      "type": "component",
      "repeatable": true,
      "component": "category.componentName"
    }
  }
}
```

## 관리자 패널에서 컴포넌트 표시하기

플러그인의 컴포넌트가 관리자 패널에 표시되도록 하려면 컴포넌트 스키마에서 적절한 `pluginOptions`을 설정해야 합니다:

```javascript {9-16}
{
  "kind": "collectionType",
  "collectionName": "my_plugin_components",
  "info": {
    "singularName": "my-plugin-component",
    "pluralName": "my-plugin-components",
    "displayName": "My Plugin Component"
  },
  "pluginOptions": {
    "content-manager": {
      "visible": true
    },
    "content-type-builder": {
      "visible": true
    }
  },
  "attributes": {
    "name": {
      "type": "string"
    }
  }
}
```

이 설정을 통해 컴포넌트가 콘텐츠 타입 빌더와 콘텐츠 매니저 모두에서 표시되고 편집 가능하게 됩니다.
