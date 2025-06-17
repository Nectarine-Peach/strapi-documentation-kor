---
title: TypeScript - 문서와 엔트리 조작하기
sidebar_label: 문서와 엔트리 조작하기
displayed_sidebar: cmsSidebar
description: 문서와 엔트리 조작을 시작하기 위한 TypeScript 가이드
tags:
  - 타입스크립트
  - 가이드
  - 데이터
  - 문서
  - 컴포넌트
  - uid
  - 엔트리
---

# TypeScript 기반 프로젝트에서 문서와 엔트리 조작하기

이 가이드는 Strapi v5 애플리케이션에서 문서와 엔트리를 조작하기 위한 [TypeScript](/cms/typescript) 패턴을 탐색하며, Strapi의 `UID`와 `Data` 네임스페이스를 활용하여 일반적인 엔티티 타입과 알려진 엔티티 타입 모두와 안전하게 상호작용하는 방법을 포함합니다. TypeScript 기반 Strapi 프로젝트에서 작업한다면, 이러한 접근 방식을 마스터하는 것이 타입 안전성과 코드 완성의 이점을 최대한 활용하는 데 도움이 되어, 애플리케이션의 콘텐츠와 컴포넌트와의 견고하고 오류 없는 상호작용을 보장합니다.

:::prerequisites

- **Strapi 애플리케이션:** Strapi v5 애플리케이션. 없다면 [문서](/cms/quick-start)를 따라 시작하세요.
- **TypeScript:** Strapi 프로젝트에 TypeScript가 설정되어 있는지 확인하세요. TypeScript 구성에 대한 Strapi의 [공식 가이드](/cms/typescript#getting-started-with-typescript-in-strapi)를 따를 수 있습니다.
- **생성된 타입:** 애플리케이션 타입이 [생성되었고](/cms/typescript/development#generate-typings-for-content-types-schemas) 접근 가능한지 확인하세요.
  :::

## 타입 가져오기

`UID` 네임스페이스는 애플리케이션에서 사용 가능한 리소스를 나타내는 리터럴 유니온을 포함합니다.

```typescript
import type { UID } from '@strapi/strapi';
```

- `UID.ContentType`은 애플리케이션의 모든 콘텐츠 타입 식별자의 유니온을 나타냅니다
- `UID.Component`는 애플리케이션의 모든 컴포넌트 식별자의 유니온을 나타냅니다
- `UID.Schema`는 모든 스키마(콘텐츠 타입 또는 컴포넌트) 식별자의 유니온을 나타냅니다
- 그리고 기타 등등...

---

Strapi는 엔티티 표현을 위한 여러 내장 타입을 포함하는 `Data` 네임스페이스를 제공합니다.

```typescript
import type { Data } from '@strapi/strapi';
```

- `Data.ContentType`은 Strapi 문서 객체를 나타냅니다
- `Data.Component`는 Strapi 컴포넌트 객체를 나타냅니다
- `Data.Entity`는 문서 또는 컴포넌트를 나타냅니다

:::tip
엔티티의 타입 정의와 UID 모두 애플리케이션의 생성된 스키마 타입을 기반으로 합니다.

불일치나 오류가 발생하는 경우, 언제든지 [타입을 재생성](/cms/typescript/development#generate-typings-for-content-types-schemas)할 수 있습니다.
:::

## 사용법

<br />

### 일반적인 엔티티

일반적인 데이터를 다룰 때는 `Data` 타입의 매개변수화되지 않은 형태를 사용하는 것이 권장됩니다.

#### 일반적인 문서

```typescript
async function save(name: string, document: Data.ContentType) {
  await writeCSV(name, document);
  //                    ^ {
  //                        id: Data.ID;
  //                        documentId: string;
  //                        createdAt?: DateTimeValue;
  //                        updatedAt?: DateTimeValue;
  //                        publishedAt?: DateTimeValue;
  //                        ...
  //                      }
}
```

:::warning
앞의 예제에서 `document`에 대해 해결된 속성들은 모든 콘텐츠 타입에 공통인 것들입니다.

다른 속성들은 타입 가드를 사용하여 수동으로 확인해야 합니다.

```typescript
if ('my_prop' in document) {
  return document.my_prop;
}
```

:::

#### 일반적인 컴포넌트

```typescript
function renderComponent(parent: Node, component: Data.Component) {
  const elements: Element[] = [];
  const properties = Object.entries(component);

  for (const [name, value] of properties) {
    //        ^        ^
    //        string   any
    const paragraph = document.createElement('p');

    paragraph.textContent = `Key: ${name}, Value: ${value}`;

    elements.push(paragraph);
  }

  parent.append(...elements);
}
```

### 알려진 엔티티

알려진 엔티티를 조작할 때는 더 나은 타입 안전성과 코드 완성을 위해 `Data` 타입을 매개변수화할 수 있습니다.

#### 알려진 문서

```typescript
const ALL_CATEGORIES = ['food', 'tech', 'travel'];

function validateArticle(article: Data.ContentType<'api::article.article'>) {
  const { title, category } = article;
  //       ^?         ^?
  //       string     Data.ContentType<'api::category.category'>

  if (title.length < 5) {
    throw new Error('제목이 너무 짧습니다');
  }

  if (!ALL_CATEGORIES.includes(category.name)) {
    throw new Error(`알 수 없는 카테고리 ${category.name}`);
  }
}
```

#### 알려진 컴포넌트

```typescript
function processUsageMetrics(
  id: string,
  metrics: Data.Component<'app.metrics'>
) {
  telemetry.send(id, { clicks: metrics.clicks, views: metrics.views });
}
```

### 고급 사용 사례

<br/>

#### 엔티티 하위 집합

타입의 두 번째 매개변수(`TKeys`)를 사용하여 엔티티의 하위 집합을 얻을 수 있습니다.

```typescript
type Credentials = Data.ContentType<'api::acount.acount', 'email' | 'password'>;
//   ^? { email: string; password: string }
```

```typescript
type UsageMetrics = Data.Component<'app.metrics', 'clicks' | 'views'>;
//   ^? { clicks: number; views: number }
```

#### 타입 인수 추론

다른 함수 매개변수를 기반으로 엔티티 타입을 바인딩하고 제한할 수 있습니다.

다음 예제에서 `uid` 타입은 사용 시 `T`로 추론되고 `document`의 타입 매개변수로 사용됩니다.

```typescript
import type { UID } from '@strapi/strapi';

function display<T extends UID.ContentType>(
  uid: T,
  document: Data.ContentType<T>
) {
  switch (uid) {
    case 'api::article.article': {
      return document.title;
      //              ^? string
      //     ^? Data.ContentType<'api::article.article'>
    }
    case 'api::category.category': {
      return document.name;
      //              ^? string
      //     ^? Data.ContentType<'api::category.category'>
    }
    case 'api::account.account': {
      return document.email;
      //              ^? string
      //     ^? Data.ContentType<'api::account.account'>
    }
    default: {
      throw new Error(`알 수 없는 콘텐츠 타입 uid: "${uid}"`);
    }
  }
}
```

When calling the function, the `document` type needs to match the given `uid`.

```typescript
declare const article: Data.Document<'api::article.article'>;
declare const category: Data.Document<'api::category.category'>;
declare const account: Data.Document<'api::account.account'>;

display('api::article.article', article);
display('api::category.category', category);
display('api::account.account', account);
// ^ ✅

display('api::article.article', category);
// ^ Error: "category" is not assignable to parameter of type ContentType<'api::article.article'>
```
