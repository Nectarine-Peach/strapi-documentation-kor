---
title: TypeScript 지원 추가하기
description: 기존 Strapi 프로젝트에 TypeScript 지원을 추가하는 방법을 알아보세요.
displayed_sidebar: cmsSidebar
pagination_previous: cms/typescript/development
tags:
- allowJs 플래그
- 타입스크립트
- tsconfig.json 파일
- 프로젝트 구조
---

# 기존 Strapi 프로젝트에 TypeScript 지원 추가하기

기존 프로젝트에 [TypeScript](/cms/typescript) 지원을 추가하려면 2개의 `tsconfig.json` 파일을 추가하고 관리자 패널을 재빌드해야 합니다. 추가로 `eslintrc`와 `eslintignore` 파일은 선택적으로 제거할 수 있습니다.

TypeScript 플래그 `allowJs`는 기존 JavaScript 프로젝트에 점진적으로 TypeScript 파일을 추가하기 위해 루트 `tsconfig.json` 파일에서 `true`로 설정해야 합니다. `allowJs` 플래그는 `.ts`와 `.tsx` 파일이 JavaScript 파일과 공존할 수 있게 해줍니다.

다음 절차를 사용하여 기존 Strapi 프로젝트에 TypeScript 지원을 추가할 수 있습니다:

1. 프로젝트 루트에 `tsconfig.json` 파일을 추가하고 다음 코드를 `allowJs` 플래그와 함께 파일에 복사하세요:

  ```json title="./tsconfig.json"

  {
      "extends": "@strapi/typescript-utils/tsconfigs/server",
      "compilerOptions": {
        "outDir": "dist",
        "rootDir": ".",
        "allowJs": true //ts 파일 없이도 빌드 가능하게 함
      },
      "include": [
        "./",
        "src/**/*.json"
      ],
      "exclude": [
        "node_modules/",
        "build/",
        "dist/",
        ".cache/",
        ".tmp/",
        "src/admin/",
        "**/*.test.ts",
        "src/plugins/**"
      ]
    
    }
    
  ```

2. `./src/admin/` 디렉터리에 `tsconfig.json` 파일을 추가하고 다음 코드를 파일에 복사하세요:

  ```json title="./src/admin/tsconfig.json"

  {
      "extends": "@strapi/typescript-utils/tsconfigs/admin",
      "include": [
        "../plugins/**/admin/src/**/*",
        "./"
      ],
      "exclude": [
        "node_modules/",
        "build/",
        "dist/",
        "**/*.test.ts"
      ]
    }
    
  ```

3. _(선택사항)_ 프로젝트 루트에서 `.eslintrc`와 `.eslintignore` 파일을 삭제하세요.
4. `database.ts` 구성 파일의 `filename` 속성에 추가 `'..'`를 추가하세요 (SQLite 데이터베이스에만 필요):

  ```js title="./config/database.ts"

  const path = require('path');

  module.exports = ({ env }) => ({
    connection: {
      client: 'sqlite',
      connection: {
        filename: path.join(
          __dirname,
          "..",
          "..",
          env("DATABASE_FILENAME", ".tmp/data.db")
        ),
      },
      useNullAsDefault: true,
    },
  });

  ```

5. 관리자 패널을 재빌드하고 개발 서버를 시작하세요:

  <Tabs groupId="yarn-npm">

  <TabItem value='yarn' label="Yarn">

    ```sh
    yarn build
    yarn develop
    ```

  </TabItem>

  <TabItem value='npm' label="NPM">

  ```sh
  npm run build
  npm run develop
  ```

  </TabItem>

  </Tabs>

`dist` 디렉터리가 프로젝트 루트에 추가되고([프로젝트 구조](/cms/project-structure) 참고), 프로젝트가 새로운 TypeScript를 지원하는 Strapi 프로젝트와 동일한 TypeScript 기능에 접근할 수 있게 됩니다.
