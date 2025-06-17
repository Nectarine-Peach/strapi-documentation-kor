---
title: 데이터 전송
description: Strapi CLI를 사용하여 데이터를 전송하세요
displayed_sidebar: cmsSidebar
canonicalUrl: https://docs.strapi.io/cms/data-management/transfer.html
tags:
- 데이터 관리 시스템
- 데이터 전송
- strapi transfer
- 환경
---

# 데이터 전송

`strapi transfer` 명령은 [데이터 관리 기능](/cms/features/data-management)의 일부로, 한 Strapi 인스턴스에서 다른 Strapi 인스턴스로 데이터를 스트리밍합니다. `transfer` 명령은 엄격한 스키마 매칭을 사용하므로, 두 Strapi 인스턴스는 포함된 데이터를 제외하고는 서로 정확한 복사본이어야 합니다. 기본 `transfer` 명령은 콘텐츠(엔티티 및 관계), 파일(에셋), 프로젝트 구성 및 스키마를 전송합니다. 이 명령을 사용하면 다음과 같이 데이터를 전송할 수 있습니다:

- 로컬 Strapi 인스턴스에서 원격 Strapi 인스턴스로
- 원격 Strapi 인스턴스에서 로컬 Strapi 인스턴스로

다음 문서는 데이터 전송을 사용자 정의하는 데 사용할 수 있는 옵션을 자세히 설명합니다. transfer 명령과 모든 사용 가능한 옵션은 [Strapi CLI](/cms/cli#strapi-transfer)를 사용하여 실행됩니다.

:::caution

* 대상 인스턴스에서 SQLite 데이터베이스를 사용하는 경우 `transfer` 작업이 실행되는 동안 다른 데이터베이스 연결이 차단됩니다.
* 관리자 사용자와 API 토큰은 전송되지 않습니다.
* 프로젝트에서 웹소켓이나 Socket.io를 사용하는 경우 transfer 명령이 실패합니다. transfer 명령을 사용하려면 **웹소켓이나 Socket.io를 일시적으로 비활성화**하거나 웹소켓 서버가 Strapi 서버와 다른 포트 또는 Strapi 내의 특정 경로에서 실행되도록 해야 합니다.

:::

CLI 명령은 다음 인수들로 구성됩니다:

| 옵션         | 설명                                                                                                                                  |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `--to`         | 대상 Strapi 인스턴스의 `/admin` 엔드포인트 전체 URL<br />(예: `--to https://my-beautiful-strapi-website/admin`)            |
| `‑‑to‑token`   | Strapi 대상 인스턴스의 전송 토큰.                                                                                         |
| `--from`       | 데이터를 가져올 원격 Strapi 인스턴스의 `/admin` 엔드포인트 전체 URL (예: `--from https://my-beautiful-strapi-website/admin`) |
| `‑‑from‑token` | Strapi 소스 인스턴스의 전송 토큰.                                                                                              |
| `--force`      | 잠재적으로 파괴적인 요청을 포함하여 모든 프롬프트에 자동으로 "yes"로 답하고 비대화형으로 실행합니다.                            |
| `--exclude`    | 쉼표로 구분된 데이터 타입을 사용하여 데이터를 제외합니다. 사용 가능한 타입: `content`, `files`, `config`.                                    |
| `--only`       | 이 데이터만 포함합니다. 사용 가능한 타입: `content`, `files`, `config`.                                                          |
| `--throttle` | 전송 중 "청크" 사이에 인위적인 지연을 주입하는 시간(밀리초). |
| `--verbose` | 자세한 로그를 활성화합니다. |

:::caution
`--to` 또는 `--from` 중 하나는 필수입니다.
:::

:::tip 팁
* 데이터 전송은 [관리자 패널에서 관리되는](/cms/features/data-management#admin-panel-settings) 전송 토큰으로 승인됩니다. 관리자 패널에서 `view`, `create`, `read`, `regenerate`, `delete`를 포함한 토큰의 역할 기반 권한을 관리할 수 있습니다.
* 복사/붙여넣기를 피하기 위해 전송 토큰을 [환경 변수](/cms/configurations/environment)에 저장하는 것이 편리할 수 있습니다. 단지 이러한 토큰이 공개 저장소에 푸시되지 않도록 주의하세요.
:::

:::warning
nginx와 localhost로 요청을 프록시하는 서버를 사용할 때 문제가 발생할 수 있습니다. 이를 방지하려면 `/etc/nginx/sites-available/yourdomain`의 구성 파일을 다음과 같이 변경하여 모든 헤더가 올바르게 전달되도록 하세요:

```
server {
    listen 80;
    server_name <yourdomain>;
    location / {
        proxy_pass http://localhost:1337;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
        include proxy_params;
    }
}
```

:::

## 전송 토큰 생성

:::prerequisites
[관리자 패널 구성](/cms/configurations/admin-panel) 파일에 salt 전송 토큰이 정의되어야 합니다.
:::

`strapi transfer` 명령은 대상 인스턴스에서 발급한 전송 토큰이 필요합니다. 관리자 패널에서 전송 토큰을 생성하려면 [사용자 가이드](/cms/features/data-management#admin-panel-settings)의 지침을 사용하세요.

## 데이터 전송 설정 및 실행

데이터 전송 시작은 원격 인스턴스로 데이터를 푸시하려는지 원격에서 데이터를 풀하려는지에 따라 달라집니다:

<Tabs>

<TabItem value="push" label="원격으로 데이터 푸시">

  1. 대상 인스턴스의 Strapi 서버를 시작합니다.
  2. 새 터미널 창에서 소스 인스턴스의 루트 디렉토리로 이동합니다.
  3. 다음 최소 명령을 실행하여 전송을 시작하세요. `destinationURL`이 관리자 패널의 전체 URL(즉, URL에 `/admin` 부분이 포함됨)인지 확인하세요:

    <Tabs groupId="yarn-npm">

    <TabItem value="yarn" label="yarn">

    ```bash
    yarn strapi transfer --to destinationURL
    ```

    </TabItem>

    <TabItem value="npm" label="npm">

    ```bash
    npm run strapi transfer -- --to destinationURL
    ```

    </TabItem>

    </Tabs>
  
  4. 프롬프트가 나타나면 전송 토큰을 추가하세요.
  5. CLI 프롬프트에 **Yes** 또는 **No**로 답하세요: "전송하면 모든 원격 Strapi 에셋과 데이터베이스가 삭제됩니다. 계속 진행하시겠습니까?"

</TabItem>

<TabItem value="pull" label="원격에서 데이터 풀">

1. 소스 인스턴스의 Strapi 서버를 시작합니다.
2. 새 터미널 창에서 대상 인스턴스의 루트 디렉토리로 이동합니다.
  3. 다음 최소 명령을 실행하여 전송을 시작하세요. `remoteURL`이 관리자 패널의 전체 URL(즉, URL에 `/admin` 부분이 포함됨)인지 확인하세요:

  <Tabs groupId="yarn-npm">

  <TabItem value="yarn" label="yarn">

  ```bash
  yarn strapi transfer --from remoteURL
  ```

  </TabItem>

  <TabItem value="npm" label="npm">

  ```bash
  npm run strapi transfer -- --from remoteURL
  ```

  </TabItem>

  </Tabs>

4. 프롬프트가 나타나면 전송 토큰을 추가하세요.
5. CLI 프롬프트에 **Yes** 또는 **No**로 답하세요: "전송하면 모든 로컬 Strapi 에셋과 데이터베이스가 삭제됩니다. 계속 진행하시겠습니까?".

</TabItem>
</Tabs>

## 모든 `transfer` 명령줄 프롬프트 우회

`strapi transfer` 명령을 사용할 때 전송이 기존 데이터베이스 내용을 삭제한다는 것을 확인해야 합니다. `--force` 플래그를 사용하면 이 프롬프트를 우회할 수 있습니다. 이 옵션은 `strapi transfer`를 프로그래밍 방식으로 구현할 때 유용합니다. `--force` 옵션을 사용하는 경우 전송 토큰과 함께 `to-token` 옵션을 전달해야 합니다.

:::caution
`--force` 옵션은 콘텐츠 삭제에 대한 모든 경고를 우회합니다.
:::

### 예시: `--force`로 `transfer` 명령줄 프롬프트 우회

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn strapi transfer --to https://example.com/admin --to-token my-transfer-token --force
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm run strapi transfer -- --to https://example.com/admin --to-token my-transfer-token --force
```

</TabItem>

</Tabs>

## 전송 중 지정된 데이터 타입만 포함

기본 `strapi transfer` 명령은 콘텐츠(엔티티 및 관계), 파일(에셋), 프로젝트 구성 및 스키마를 전송합니다. `--only` 옵션을 사용하면 타입 사이에 공백 없이 쉼표로 구분된 문자열을 전달하여 나열된 항목만 전송할 수 있습니다. 사용 가능한 값은 `content`, `files`, `config`입니다. `strapi transfer`에 스키마 매칭이 사용되므로 스키마는 항상 전송됩니다.

### 예시: 파일만 전송

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn strapi transfer --to https://example.com/admin --only files
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm run strapi transfer -- --to https://example.com/admin --only files
```

</TabItem>

</Tabs>

## Exclude data types during transfer

The default `strapi transfer` command transfers your content (entities and relations), files (assets), project configuration, and schemas. The `--exclude` option allows you to exclude content, files, and the project configuration by passing these items in a comma-separated string with no spaces between the types. You can't exclude the schemas, as schema matching is used for `strapi transfer`.

### Example: exclude files from transfer

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn strapi transfer --to https://example.com/admin --exclude files
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm run strapi transfer -- --to https://example.com/admin --exclude files
```

</TabItem>

</Tabs>

:::warning
Any types excluded from the transfer will be deleted in your destination instance. For example, if you exclude `config` the project configuration in your destination instance will be deleted.
:::

## Manage data transfer with environment variables

The environment variable `STRAPI_DISABLE_REMOTE_DATA_TRANSFER` is available to disable remote data transfer. In addition to the [RBAC permissions](/cms/features/rbac#plugins-and-settings) in the admin panel this can help you secure your Strapi application. To use `STRAPI_DISABLE_REMOTE_DATA_TRANSFER` you can add it to your `.env` file or preface the `start` script. See the following example:

```bash
STRAPI_DISABLE_REMOTE_DATA_TRANSFER=true yarn start
```

Additional details on using environment variables in Strapi are available in the [Environment configurations documentation](/cms/configurations/environment).

## Test the transfer command locally

The `transfer` command is not intended for transferring data between two local instances. The [`export`](/cms/data-management/export) and [`import`](/cms/data-management/import) commands were designed for this purpose. However, you might want to test `transfer` locally on test instances to better understand the functionality before using it with a remote instance. The following documentation provides a fully-worked example of the `transfer` process.

### Create and clone a new Strapi project

1. Create a new Strapi project using the installation command:

   ```bash
   npx create-strapi-app@latest <project-name> --quickstart
   ```

2. Create at least 1 content type in the project. See the [Quick Start Guide](/cms/quick-start) if you need instructions on creating your first content type.

   :::caution
   Do not add any data to your project at this step.
   :::

3. Commit the project to a git repository:

   ```bash
   git init
   git add .
   git commit -m "first commit"
   ```

4. Clone the project repository:

   ```bash
   cd .. # move to the parent directory
   git clone <path to created git repository>.git/ <new-instance-name>
   ```

### Add data to the first Strapi instance

1. Return to the first Strapi instance and add data to the content type.
2. Stop the server on the first instance.

### Create a transfer token

1. Navigate to the second Strapi instance and run the `build` and `start` commands in the root directory:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn build && yarn start
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm run build && npm run start
```

</TabItem>

</Tabs>

2. Register an admin user.
3. [Create and copy a transfer token](/cms/features/data-management#admin-panel-settings).
4. Leave the server running.

### Transfer your data

1. Return the the first Strapi instance.
2. In the terminal run the `strapi transfer` command:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```bash
yarn strapi transfer --to http://localhost:1337/admin
```

</TabItem>

<TabItem value="npm" label="npm">

```bash
npm run strapi transfer -- --to http://localhost:1337/admin
```

</TabItem>

</Tabs>

3. When prompted, apply the transfer token.
4. When the transfer is complete you can return to the second Strapi instance and see that the content is successfully transferred.

:::tip
In some cases you might receive a connection refused error targeting `localhost`. Try changing the address to <ExternalLink to="http://127.0.0.1:1337/admin" text="http://127.0.0.1:1337/admin"/>.
:::

<FeedbackPlaceholder />
