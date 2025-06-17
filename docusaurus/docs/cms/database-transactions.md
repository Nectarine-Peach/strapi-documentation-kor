---
title: 데이터베이스 트랜잭션
description: Strapi의 트랜잭션에 대한 개념 가이드
tags:
  - database
  - experimental
---

# 데이터베이스 트랜잭션

:::caution
이것은 실험적 기능이며 향후 버전에서 변경될 수 있습니다.
:::

Strapi 5는 데이터의 무결성을 보장하는 트랜잭션으로 일련의 작업을 래핑하는 API를 제공합니다.

트랜잭션은 단일 단위로 함께 실행되는 일련의 작업입니다. 작업 중 하나라도 실패하면 전체 트랜잭션이 실패하고 데이터가 이전 상태로 롤백됩니다. 모든 작업이 성공하면 트랜잭션이 커밋되고 데이터가 데이터베이스에 영구적으로 저장됩니다.

## 사용법

트랜잭션은 핸들러 함수를 `strapi.db.transaction`에 전달하여 처리됩니다:

```js
await strapi.db.transaction(async ({ trx, rollback, commit, onCommit, onRollback }) => {
  // 암시적으로 트랜잭션을 사용합니다
  await strapi.entityService.create();
  await strapi.entityService.create();
});
```

트랜잭션 핸들러가 실행된 후, 모든 작업이 성공하면 트랜잭션이 커밋됩니다. 작업 중 하나라도 예외를 발생시키면 트랜잭션이 롤백되고 데이터가 이전 상태로 복원됩니다.

:::note
트랜잭션 블록에서 수행되는 모든 `strapi.entityService` 또는 `strapi.db.query` 작업은 암시적으로 트랜잭션을 사용합니다.
:::

### 트랜잭션 핸들러 속성

핸들러 함수는 다음 속성을 가진 객체를 받습니다:

| 속성         | 설명                                                                                        |
| ------------ | ------------------------------------------------------------------------------------------- |
| `trx`        | 트랜잭션 객체입니다. 트랜잭션 내에서 knex 쿼리를 수행하는 데 사용할 수 있습니다.              |
| `commit`     | 트랜잭션을 커밋하는 함수입니다.                                                              |
| `rollback`   | 트랜잭션을 롤백하는 함수입니다.                                                              |
| `onCommit`   | 트랜잭션이 커밋된 후 실행될 콜백을 등록하는 함수입니다.                                       |
| `onRollback` | 트랜잭션이 롤백된 후 실행될 콜백을 등록하는 함수입니다.                                       |

### 중첩 트랜잭션

트랜잭션은 중첩될 수 있습니다. 트랜잭션이 중첩되면 외부 트랜잭션이 커밋되거나 롤백될 때 내부 트랜잭션도 커밋되거나 롤백됩니다.

```js
await strapi.db.transaction(async () => {
  // 암시적으로 트랜잭션을 사용합니다
  await strapi.entityService.create();

  // 중첩 트랜잭션은 암시적으로 외부 트랜잭션을 사용합니다
  await strapi.db.transaction(async ({}) => {
    await strapi.entityService.create();
  });
});
```

### onCommit과 onRollback

`onCommit`과 `onRollback` 훅은 트랜잭션이 커밋되거나 롤백된 후 코드를 실행하는 데 사용할 수 있습니다.

```js
await strapi.db.transaction(async ({ onCommit, onRollback }) => {
  // 암시적으로 트랜잭션을 사용합니다
  await strapi.entityService.create();
  await strapi.entityService.create();

  onCommit(() => {
    // 트랜잭션이 커밋된 후 실행됩니다
  });

  onRollback(() => {
    // 트랜잭션이 롤백된 후 실행됩니다
  });
});
```

### knex 쿼리 사용하기

트랜잭션은 knex 쿼리와도 함께 사용할 수 있지만, 이 경우 `.transacting(trx)`를 명시적으로 호출해야 합니다.

```js
await strapi.db.transaction(async ({ trx, rollback, commit }) => {
  await knex('users').where('id', 1).update({ name: 'foo' }).transacting(trx);
});
```

## 언제 트랜잭션을 사용해야 하는가

트랜잭션은 여러 작업이 함께 실행되어야 하고 서로 의존적인 경우에 사용해야 합니다. 예를 들어, 새 사용자를 생성할 때 사용자가 데이터베이스에 생성되고 환영 이메일이 사용자에게 전송되어야 합니다. 이메일 전송이 실패하면 사용자가 데이터베이스에 생성되지 않아야 합니다.

## 언제 트랜잭션을 사용하지 말아야 하는가

서로 의존적이지 않은 작업에는 트랜잭션을 사용하지 않아야 합니다. 성능 저하를 초래할 수 있기 때문입니다.

## 트랜잭션의 잠재적 문제점

트랜잭션 내에서 여러 작업을 수행하면 잠금이 발생할 수 있으며, 이는 원래 트랜잭션이 완료될 때까지 다른 프로세스의 트랜잭션 실행을 차단할 수 있습니다.

또한 트랜잭션이 적절히 커밋되거나 롤백되지 않으면 교착 상태가 될 수 있습니다.

예를 들어, 트랜잭션이 열렸지만 코드에서 이를 닫지 않는 경로가 있다면, 트랜잭션이 무기한 열려 있게 되고 서버가 재시작되어 연결이 강제로 닫힐 때까지 불안정성을 야기할 수 있습니다. 이러한 문제는 디버그하기 어려울 수 있으므로, 필요한 경우에만 신중하게 트랜잭션을 사용하세요.
