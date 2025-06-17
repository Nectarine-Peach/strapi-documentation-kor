---
title: 데이터베이스 마이그레이션
description: Strapi 데이터베이스 마이그레이션은 데이터베이스를 수정하는 방법입니다
---

# 데이터베이스 마이그레이션

데이터베이스 마이그레이션은 데이터베이스에 대해 일회성 쿼리를 실행하는 기능으로, 주로 Strapi 애플리케이션을 업그레이드할 때 테이블 구조나 데이터를 수정하는 데 사용됩니다. 이러한 마이그레이션은 애플리케이션이 시작될 때 자동으로 실행되며, Strapi가 부팅 시 수행하는 자동 스키마 마이그레이션보다 먼저 실행됩니다.

:::callout 🚧  실험적 기능
데이터베이스 마이그레이션은 실험적 기능입니다. 이 기능은 아직 개발 중이며 지속적으로 업데이트되고 개선될 예정입니다. 그동안 도움이 필요하시면 <ExternalLink to="https://forum.strapi.io/" text="포럼"/>이나 커뮤니티 <ExternalLink to="https://discord.strapi.io" text="Discord"/>에서 문의해 주세요.
:::

## 데이터베이스 마이그레이션 파일 이해하기

마이그레이션은 `./database/migrations`에 저장된 JavaScript 마이그레이션 파일을 사용하여 실행됩니다.

Strapi는 마이그레이션 파일을 자동으로 감지하고 다음 시작 시 알파벳 순서로 한 번씩 실행합니다. 모든 새 파일은 한 번만 실행됩니다. 마이그레이션은 데이터베이스 테이블이 콘텐츠 타입 스키마와 동기화되기 전에 실행됩니다.

:::warning
* 현재 Strapi는 다운 마이그레이션을 지원하지 않습니다. 즉, 마이그레이션을 되돌려야 하는 경우 수동으로 처리해야 합니다. 다운 마이그레이션 구현이 계획되어 있지만 구체적인 일정은 미정입니다.

* Strapi는 알 수 없는 테이블을 경고 없이 삭제합니다. 즉, 데이터베이스 마이그레이션은 Strapi 스키마를 변경할 때 데이터를 유지하는 용도로만 사용할 수 있습니다. `forceMigration`과 `runMigrations` [데이터베이스 설정 매개변수](/cms/configurations/database#settings-configuration-object)를 사용하여 데이터베이스 마이그레이션 동작을 세밀하게 조정할 수 있습니다.
:::

마이그레이션 파일은 업그레이드 시 사용되는 `up()` 함수를 내보내야 합니다(예: 새 테이블 `my_new_table` 추가).

`up()` 함수는 데이터베이스 트랜잭션 내에서 실행되므로, 마이그레이션 중 쿼리가 실패하면 전체 마이그레이션이 취소되고 데이터베이스에 변경사항이 적용되지 않습니다. 마이그레이션 함수 내에서 다른 트랜잭션이 생성되면 중첩 트랜잭션으로 작동합니다.

:::note
데이터베이스 마이그레이션을 수동으로 실행하는 CLI는 없습니다.
:::

## 마이그레이션 파일 생성하기

마이그레이션 파일을 생성하려면:

1. `./database/migrations` 폴더에서 날짜와 마이그레이션 이름으로 새 파일을 생성합니다(예: `2022.05.10T00.00.00.name-of-my-migration.js`). 파일 이름의 알파벳 순서가 마이그레이션 실행 순서를 결정하므로, 이 명명 패턴을 따르는지 확인하세요.

2. 이전에 생성한 파일에 다음 템플릿을 복사하여 붙여넣습니다:

```jsx
'use strict'

async function up(knex) {}

module.exports = { up };
```

3. `up()` 함수 내에 실제 마이그레이션 코드를 추가하여 템플릿을 완성합니다.
`up()`은 이미 트랜잭션 상태인 <ExternalLink to="https://knexjs.org/" text="Knex 인스턴스"/>를 받아서 데이터베이스 쿼리를 실행하는 데 사용할 수 있습니다.

<details>
<summary>마이그레이션 파일 예시</summary>

```jsx title="./database/migrations/2022.05.10T00.00.00.name-of-my-migration.js"

module.exports = {
  async up(knex) {
    // 이미 초기화된 데이터베이스 연결로 Knex.js API에 완전히 액세스할 수 있습니다

    // 예시: 테이블 이름 변경
    await knex.schema.renameTable('oldName', 'newName');

    // 예시: 컬럼 이름 변경
    await knex.schema.table('someTable', table => {
      table.renameColumn('oldName', 'newName');
    });

    // 예시: 데이터 업데이트
    await knex.from('someTable').update({ columnName: 'newValue' }).where({ columnName: 'oldValue' });
  },
};
```

</details>

### 마이그레이션에서 Strapi 인스턴스 사용하기

:::danger
사용자가 마이그레이션에서 Knex를 직접 사용하지 않고 Strapi 인스턴스를 활용하는 경우, 마이그레이션 코드를 `strapi.db.transaction()`으로 감싸는 것이 중요합니다. 그렇지 않으면 오류 발생 시 마이그레이션이 롤백되지 않을 수 있습니다.
:::

<details>
<summary>Strapi 인스턴스를 사용한 마이그레이션 파일 예시</summary>

```jsx title="./database/migrations/2022.05.10T00.00.00.name-of-my-migration.js"
module.exports = {
  async up() {
    await strapi.db.transaction(async () => {
      // 여기에 마이그레이션 코드 작성

      // 예시: 새 엔트리 생성
      await strapi.entityService.create('api::article.article', {
        data: {
          title: 'My Article',
        },
      });

      // 예시: 커스텀 서비스 메소드
      await strapi.service('api::article.article').updateRelatedArticles();
    });
  },
};
```

</details>

## TypeScript 코드로 마이그레이션 처리하기

기본적으로 Strapi는 TypeScript를 사용할 때 빌드 디렉토리가 아닌 소스 디렉토리에서 마이그레이션 파일을 찾습니다. 즉, Strapi가 올바른 위치를 찾도록 설정하지 않으면 TypeScript 마이그레이션이 제대로 찾아지거나 실행되지 않습니다.

Strapi에서 TypeScript 마이그레이션을 활성화하려면 데이터베이스 설정에서 `useTypescriptMigrations` 매개변수를 true로 설정해야 합니다. 이 설정은 Strapi가 소스 디렉토리 대신 빌드 디렉토리에서 마이그레이션을 찾도록 지시합니다.

데이터베이스 설정에서 구성하는 방법은 다음과 같습니다:

<Tabs groupId="js-ts">
<TabItem value="js" label="JavaScript">

```jsx title="/config/database.js"
module.exports = ({ env }) => ({
  connection: {
    // 데이터베이스 연결 설정
  },
  settings: {
    useTypescriptMigrations: true
  }
});
```

</TabItem>

<TabItem value="ts" label="TypeScript">

```tsx title="/config/database.ts"
export default ({ env }) => ({
  connection: {
    // 데이터베이스 연결 설정
  },
  settings: {
    useTypescriptMigrations: true
  }
});
```

</TabItem>
</Tabs>

또한 기존 JavaScript 마이그레이션을 TypeScript 마이그레이션과 함께 계속 사용하려면, [데이터베이스 설정 문서](/cms/configurations/database#settings-configuration-object)에서 언급된 대로 `tsconfig.json` 파일의 컴파일러 옵션에서 `allowJs: true`를 설정할 수 있습니다.
