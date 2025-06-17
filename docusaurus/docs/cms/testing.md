---
title: 테스트
displayed_sidebar: cmsSidebar
description: Strapi 애플리케이션을 테스트하는 방법을 알아보세요.
tags:
- 인증 엔드포인트 컨트롤러
- 환경
---

# 단위 테스트

:::strapi
Strapi 블로그에서 <ExternalLink to="https://strapi.io/blog/automated-testing-for-strapi-api-with-jest-and-supertest" text="Jest와 Supertest로 API 테스트 자동화"/> 및 <ExternalLink to="https://strapi.io/blog/how-to-add-unit-tests-to-your-strapi-plugin" text="Strapi 플러그인에 단위 테스트 추가하기"/> 튜토리얼을 참고할 수 있습니다.
:::

이 가이드에서는 테스트 프레임워크를 사용해 Strapi 애플리케이션의 기본 단위 테스트를 실행하는 방법을 알아봅니다.

예시에서는 <ExternalLink to="https://jestjs.io/" text="Jest"/> 테스트 프레임워크와 <ExternalLink to="https://github.com/visionmedia/supertest" text="Supertest"/>(Node.js HTTP 서버를 테스트할 수 있는 라이브러리)를 사용합니다.

:::caution
이 가이드는 Windows에서 SQLite 데이터베이스를 사용할 경우 동작하지 않을 수 있습니다. (Windows는 SQLite 파일을 잠그는 방식 때문)
:::

## 테스트 도구 설치

`Jest`는 테스트 케이스를 만들고 설계하는 데 도움이 되는 가이드라인과 도구 모음입니다.

`Supertest`는 모든 `api` 라우트를 <ExternalLink to="https://nodejs.org/api/http.md#http_class_http_server" text="http.Server"/> 인스턴스처럼 테스트할 수 있게 해줍니다.

`sqlite3`는 테스트 간 생성/삭제되는 임시 디스크 데이터베이스를 만듭니다.

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn add --dev jest supertest sqlite3
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm install jest supertest sqlite3 --save-dev
```

</TabItem>

</Tabs>

설치 후 `package.json` 파일에 다음을 추가하세요.

`scripts` 섹션에 `test` 명령어 추가:

```json
  "scripts": {
    "develop": "strapi develop",
    "start": "strapi start",
    "build": "strapi build",
    "strapi": "strapi",
    "test": "jest --forceExit --detectOpenHandles"
  },
```

그리고 파일 하단에 다음을 추가:

```json
  "jest": {
    "testPathIgnorePatterns": [
      "/node_modules/",
      ".tmp",
      ".cache"
    ],
    "testEnvironment": "node"
  }
```

이 설정은 `Jest`가 테스트를 찾지 않아야 할 폴더를 무시하도록 합니다.

## 테스트 환경 설정

테스트 프레임워크는 유효한 테스트를 위해 깨끗한 환경이 필요하며, 실제 데이터베이스와 격리되어야 합니다.

`jest`가 실행되면 `test` [환경](/cms/configurations/environment)을 사용합니다(`NODE_ENV`가 `test`로 변경됨). 이 목적을 위해 별도의 환경 설정 파일이 필요합니다. `./config/env/test/database.js` 파일을 만들고, 아래와 같이 작성하세요.

```js title="path: ./config/env/test/database.js"
module.exports = ({ env }) => ({
  connection: {
    client: 'sqlite',
    connection: {
      filename: env('DATABASE_FILENAME', '.tmp/test.db'),
    },
    useNullAsDefault: true,
    debug: false
  },
});
```

## Strapi 인스턴스 생성

무엇이든 테스트하려면 테스트 환경에서 동작하는 Strapi 인스턴스가 필요합니다. 즉, Strapi 앱의 인스턴스를 객체로 받아와야 하며, 이는 <ExternalLink to="https://forum.strapi.io/t/how-to-use-pm2-process-manager-with-strapi/" text="프로세스 매니저"/>에서 인스턴스를 생성하는 것과 유사합니다.

이 작업을 위해 몇 개의 파일을 추가해야 합니다. 모든 테스트를 넣을 `tests` 폴더를 만들고, 그 안에 `helpers` 폴더를 만들어 주요 Strapi 헬퍼를 `strapi.js` 파일에 작성합니다.

```js title="path: ./tests/helpers/strapi.js"
const Strapi = require("@strapi/strapi");
const fs = require("fs");

let instance;

async function setupStrapi() {
  if (!instance) {
    await Strapi().load();
    instance = strapi;
    
    await instance.server.mount();
  }
  return instance;
}

async function cleanupStrapi() {
  const dbSettings = strapi.config.get("database.connection");

  // 서버를 닫아 db 파일을 해제
  await strapi.server.httpServer.close();

  // 데이터베이스 연결을 닫음
  await strapi.db.connection.destroy();

  // 모든 테스트가 끝난 후 임시 DB 파일 삭제
  if (dbSettings && dbSettings.connection && dbSettings.connection.filename) {
    const tmpDbFile = dbSettings.connection.filename;
    if (fs.existsSync(tmpDbFile)) {
      fs.unlinkSync(tmpDbFile);
    }
  }
}

module.exports = { setupStrapi, cleanupStrapi };
```

## Strapi 인스턴스 테스트

테스트의 메인 진입 파일이 필요합니다. 이 파일은 헬퍼 파일도 테스트합니다.

```js title="path: ./tests/app.test.js"
const fs = require('fs');
const { setupStrapi, cleanupStrapi } = require("./helpers/strapi");

beforeAll(async () => {
  await setupStrapi();
});

afterAll(async () => {
  await cleanupStrapi();
});

it("strapi is defined", () => {
  expect(strapi).toBeDefined();
});
```

이것만으로도 단위 테스트를 작성할 수 있습니다. `yarn test`를 실행하면 첫 테스트 결과를 볼 수 있습니다.

```bash
yarn run v1.13.0
$ jest
 PASS  tests/app.test.js
  ✓ strapi is defined (2 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        4.187 s
Ran all test suites.
✨  Done in 5.73s.
```

:::tip
Jest에서 타임아웃 오류가 발생한다면, `app.test.js` 파일의 `beforeAll` 바로 위에 `jest.setTimeout(15000)`을 추가하고, 필요에 따라 ms 값을 조정하세요.
:::

## 기본 엔드포인트 컨트롤러 테스트

:::tip
예시에서는 [컨트롤러](/cms/backend-customization/controllers) 섹션의 `Hello world` `/hello` 엔드포인트를 사용합니다.
:::

일부에서는 API 테스트가 단위 테스트가 아니라 제한된 통합 테스트라고 할 수 있지만, 여기서는 첫 엔드포인트 테스트를 계속 진행합니다.

엔드포인트가 제대로 동작하는지, `/hello` 라우트가 "Hello World"를 반환하는지 테스트합니다.

`supertest`를 사용해 엔드포인트가 예상대로 동작하는지 확인하는 별도 테스트 파일을 만듭니다.

```js title="path: ./tests/hello/index.js"
const request = require('supertest');

it("should return hello world", async () => {
  await request(strapi.server.httpServer)
    .get("/api/hello")
    .expect(200) // HTTP 상태 확인
    .then((data) => {
      expect(data.text).toBe("Hello World!"); // 응답 내용 확인
    });
});
```

그리고 이 코드를 `./tests/app.test.js` 파일 하단에 추가하세요.

```js
require('./hello');
```

이후 `yarn test`를 실행하면 다음과 같은 결과가 나옵니다.

```bash
➜  my-project yarn test
yarn run v1.13.0
$ jest --detectOpenHandles
 PASS  tests/app.test.js (5.742 s)
  ✓ strapi is defined (4 ms)
  ✓ should return hello world (54 ms)

[2020-05-22T14:37:38.018Z] debug GET /hello (10 ms) 200
Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        6.635 s, estimated 7 s
Ran all test suites.
✨  Done in 9.09s.
```

:::tip
`Jest has detected the following 1 open handles potentially keeping Jest from exiting` 오류가 발생한다면, jest 버전을 26.6.3으로 맞추면 문제가 해결됩니다.
:::

## 인증된 엔드포인트 컨트롤러 테스트

대부분의 API는 인증이 필요합니다. 이를 위해 JWT 토큰이 필요하며, 유효한 사용자를 만들어야 합니다.

인증 테스트를 위한 헬퍼를 만들어보겠습니다.

```js title="path: ./tests/helpers/auth.js"
const request = require('supertest');

// 유효한 사용자 생성
const createStrapiUser = async (strapi, data) => {
  // 사용자를 생성하고 저장
  const user = await strapi.plugins['users-permissions'].services.user.add({
    ...data,
    confirmed: true,
  });

  return user;
};

// JWT 토큰 생성
const createJwtToken = (strapi, user) => {
  return strapi.plugins['users-permissions'].services.jwt.issue({
    id: user.id,
  });
};

// 인증된 요청을 위한 유저와 JWT 생성
const createAuthRequest = async (strapi) => {
  // 테스트 사용자 생성
  const user = await createStrapiUser(strapi, {
    username: 'testuser',
    email: 'testuser@strapi.com',
    password: 'Password123',
  });

  // JWT 토큰 생성
  const jwt = createJwtToken(strapi, user);

  return { user, jwt };
};

module.exports = {
  createStrapiUser,
  createJwtToken,
  createAuthRequest,
};
```

이제 인증이 필요한 엔드포인트를 테스트할 수 있습니다:

```js title="path: ./tests/auth/index.js"
const request = require('supertest');
const { createAuthRequest } = require('../helpers/auth');

it("should return user profile when authenticated", async () => {
  const { jwt } = await createAuthRequest(strapi);
  
  await request(strapi.server.httpServer)
    .get("/api/users/me")
    .set('Authorization', `Bearer ${jwt}`)
    .expect(200)
    .then((data) => {
      expect(data.body).toBeDefined();
      expect(data.body.id).toBeDefined();
      expect(data.body.username).toBe('testuser');
    });
});
```

## 추가 테스트 시나리오

### 콘텐츠 타입 테스트

특정 콘텐츠 타입의 CRUD 작업을 테스트할 수 있습니다:

```js title="path: ./tests/article/index.js"
const request = require('supertest');
const { createAuthRequest } = require('../helpers/auth');

describe('Article', () => {
  let jwt;
  let user;

  beforeAll(async () => {
    const authData = await createAuthRequest(strapi);
    jwt = authData.jwt;
    user = authData.user;
  });

  it("should create article", async () => {
    const article = {
      data: {
        title: "Test Article",
        content: "This is a test article content",
        author: user.id,
      }
    };

    await request(strapi.server.httpServer)
      .post("/api/articles")
      .set('Authorization', `Bearer ${jwt}`)
      .send(article)
      .expect(200)
      .then((data) => {
        expect(data.body.data).toBeDefined();
        expect(data.body.data.attributes.title).toBe(article.data.title);
      });
  });

  it("should return articles", async () => {
    await request(strapi.server.httpServer)
      .get("/api/articles")
      .expect(200)
      .then((data) => {
        expect(Array.isArray(data.body.data)).toBe(true);
      });
  });
});
```

### 서비스 테스트

Strapi 서비스를 직접 테스트할 수도 있습니다:

```js title="path: ./tests/services/article.test.js"
describe('Article Service', () => {
  it('should create article via service', async () => {
    const articleData = {
      title: 'Service Test Article',
      content: 'Content created via service',
    };

    const article = await strapi.documents('api::article.article').create({
      data: articleData,
    });

    expect(article).toBeDefined();
    expect(article.title).toBe(articleData.title);
  });

  it('should find articles via service', async () => {
    const articles = await strapi.documents('api::article.article').findMany();
    
    expect(Array.isArray(articles)).toBe(true);
  });
});
```

이러한 테스트 패턴을 사용하여 Strapi 애플리케이션의 다양한 부분을 포괄적으로 테스트할 수 있습니다.
