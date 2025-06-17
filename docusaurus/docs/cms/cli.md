---
title: 커맨드 라인 인터페이스
displayed_sidebar: cmsSidebar
description: Strapi는 몇 초 만에 프로젝트를 스캐폴드하고 관리할 수 있는 모든 기능을 갖춘 커맨드 라인 인터페이스(CLI)를 제공합니다.
sidebar_label: Strapi CLI
tags:
  - 커맨드 라인 인터페이스 (CLI)
  - strapi develop
  - strapi start
  - strapi build
  - strapi export
  - strapi import
  - strapi transfer
  - strapi report
---

# 커맨드 라인 인터페이스 (CLI)

Strapi는 몇 초 만에 프로젝트를 스캐폴드하고 관리할 수 있는 모든 기능을 갖춘 커맨드 라인 인터페이스(CLI)를 제공합니다. CLI는 `yarn`과 `npm` 패키지 관리자 모두와 작동합니다.

:::caution
`strapi admin:create-user`와 같은 대화형 명령어는 `npm`에서 프롬프트를 표시하지 않습니다. `npm` 패키지 관리자에 대한 수정은 2023년 3월까지 예상됩니다. 그 동안은 `yarn` 패키지 관리자 사용을 고려하세요.
:::

:::note
Strapi를 로컬에서만 설치하는 것을 권장하며, 이는 다음의 모든 `strapi` 명령어 앞에 프로젝트 설정에 사용된 패키지 관리자를 접두사로 붙여야 함을 의미합니다 (예: `npm run strapi help` 또는 `yarn strapi help`) 또는 전용 노드 패키지 실행기를 사용합니다 (예: `npx strapi help`).

`npm`으로 옵션을 전달하려면 다음 구문을 사용하세요: `npm run strapi <command> -- --<option>`.

`yarn`으로 옵션을 전달하려면 다음 구문을 사용하세요: `yarn strapi <command> --<option>`
:::

<details>
<summary>ℹ️ Strapi 5에서 제거된 Strapi v4 CLI 명령어:</summary>

Strapi v4의 `strapi install`, `strapi uninstall`, `strapi new`, `strapi watch-admin` 명령어는 Strapi 5에서 제거되었습니다:

| Strapi v4 명령어          | Strapi 5 동등한 명령어                                                                                                                                                                                |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `strapi install [plugin]` | 플러그인에 해당하는 npx 명령어를 사용하세요 (마켓플레이스에서 찾을 수 있습니다. [마켓플레이스](/cms/plugins/installing-plugins-via-marketplace) 참조)                                                |
| `strapi new`              | 새로운 Strapi 프로젝트를 생성하기 위해 동등한 yarn 또는 npx 명령어를 사용하세요 ([CLI 설치 가이드](/cms/installation/cli) 참조)                                                                   |
| `strapi watch-admin`      | `yarn develop` 또는 `npm run develop`는 항상 "watch-admin" 모드로 Strapi 서버를 시작합니다. Strapi 5에서 이를 비활성화하려면 `yarn develop --no-watch-admin` 또는 `npm run develop --no-watch-admin`을 실행하세요. |

</details>

## strapi develop

**별칭**: `dev`

자동 재로딩이 활성화된 상태로 Strapi 애플리케이션을 시작합니다.

Strapi는 런타임에 파일을 수정/생성하며 새 파일이 생성될 때 재시작해야 합니다. 이를 위해 `strapi develop`는 파일 감시자를 추가하고 필요할 때 애플리케이션을 재시작합니다.

Strapi는 또한 관리자 패널을 위한 HMR(핫 모듈 교체)을 지원하는 미들웨어를 추가합니다. 이를 통해 애플리케이션을 재시작하거나 별도의 서버를 실행할 필요 없이 관리자 패널을 커스터마이징할 수 있습니다.

```shell
strapi develop
options: [--no-build |--no-watch-admin |--browser |--debug |--silent]
```

- **strapi develop --open**<br/>
  자동 재로딩이 활성화된 상태로 애플리케이션을 시작하고 관리자 패널이 실행 중인 기본 브라우저를 엽니다.
- **strapi develop --no-watch-admin**<br/>
  관리자 패널 코드에 변경사항이 있을 때 서버가 자동 재로드되는 것을 방지합니다.
- [사용 중단] **strapi develop --no-build**<br/>
  자동 재로딩이 활성화된 상태로 애플리케이션을 시작하고 관리자 패널 빌드 프로세스를 건너뜁니다.
- [사용 중단] **strapi develop --watch-admin**<br/>
  자동 재로딩이 활성화된 상태로 애플리케이션을 시작하고 프론트엔드 개발 서버를 함께 시작합니다. 관리자 패널을 커스터마이징할 수 있습니다.
- [사용 중단] **strapi develop --watch-admin --browser 'google chrome'**<br/>
  자동 재로딩이 활성화된 상태로 애플리케이션을 시작하고 프론트엔드 개발 서버를 함께 시작합니다. 관리자 패널을 커스터마이징할 수 있습니다. 기본 브라우저 대신 사용할 브라우저 이름을 제공하고, `false`는 브라우저 열기를 중단함을 의미합니다.

:::warning
프로덕션에서 Strapi 애플리케이션을 실행할 때는 이 명령어를 절대 사용하지 마세요.
:::

## strapi start

자동 재로딩이 비활성화된 상태로 Strapi 애플리케이션을 시작합니다.

이 명령어는 주로 프로덕션에서 사용하기 위해 재시작과 파일 쓰기 없이 Strapi 애플리케이션을 실행하는 데 사용됩니다.
콘텐츠 타입 빌더와 같은 특정 기능은 애플리케이션 재시작이 필요하기 때문에 `strapi start` 모드에서는 비활성화됩니다. `start` 명령어는 애플리케이션 시작을 커스터마이징하기 위해 [환경 변수](/cms/configurations/environment#strapi)를 앞에 붙일 수 있습니다.

## strapi build

관리자 패널을 빌드합니다.

```bash
strapi build
```

| 옵션                | 타입 | 설명                                                  |
| ------------------- | :--: | -------------------------------------------------------- |
| `-d, --debug`       |  -   | 상세 로그와 함께 디버깅 모드 활성화 (기본값: false) |
| `--minify`          |  -   | 출력 최소화 (기본값: true)                        |
| `--no-optimization` |  -   | [사용 중단]: 대신 minify를 사용하세요                         |
| `--silent`          |  -   | 아무것도 로그하지 않음 (기본값: false)                      |
| `--sourcemaps`      |  -   | 소스맵 생성 (기본값: false)                      |
| `--stats`           |  -   | 빌드 통계를 콘솔에 출력 (기본값: false)   |

## strapi login

Strapi Cloud에 로그인합니다 ([Cloud CLI](/cloud/cli/cloud-cli#strapi-login) 문서 참조).

## strapi logout

Strapi Cloud에서 로그아웃합니다 ([Cloud CLI](/cloud/cli/cloud-cli#strapi-logout) 문서 참조).

## strapi deploy

Strapi Cloud에 배포합니다 ([Cloud CLI](/cloud/cli/cloud-cli#strapi-deploy) 문서 참조).

## strapi export

[프로젝트 데이터를 내보냅니다](/cms/features/data-management). 기본 설정은 `gzip`으로 압축되고 `aes-128-ecb`로 암호화된 `.tar` 파일을 생성합니다.

```bash
strapi export
```

내보낸 파일은 현재 날짜와 타임스탬프를 사용하여 `export_YYYYMMDDHHMMSS` 형식으로 자동 명명됩니다. 또는 `-f` 또는 `--file` 플래그를 사용하여 파일명을 지정할 수 있습니다. 다음 표는 명령줄 플래그로 사용 가능한 모든 옵션을 제공합니다:

| 옵션              |  타입  | 설명                                                                                                                |
| ------------------- | :----: | -------------------------------------------------------------------------------------------------------------------------- |
| `--no-encrypt`      |   -    | 파일 암호화를 비활성화하고 `key` 옵션을 비활성화합니다.   |
| `--no-compress`     |   -    | 파일 압축을 비활성화합니다.      |
| `-k`, <br/>`--key`  | string | `export` 명령의 일부로 암호화 키를 전달합니다. <br/> `--key` 옵션은 `--no-encrypt`와 결합할 수 없습니다. |
| `-f`, <br/>`--file` | string | 내보내기 파일명을 지정합니다. 파일 확장자는 포함하지 마세요.                                                            |
| `--exclude`         | string | 쉼표로 구분된 데이터 타입을 사용하여 데이터를 제외합니다. 사용 가능한 타입: `content`, `files`, `config`.                  |
| `--only`            | string | 이러한 데이터만 포함합니다. 사용 가능한 타입: `content`, `files`, `config`.                                        |
| `-h`, <br/>`--help` |   -    | `strapi export` 명령에 대한 도움말을 표시합니다.                                                                             |

**예제**

```bash title="strapi export 예제:"
# 기본 옵션과 myData 파일명으로 데이터를 내보내며, myData.tar.gz.enc 파일이 생성됩니다.
strapi export -f myData

# 암호화 없이 데이터를 내보냅니다.
strapi export --no-encrypt
```

## strapi import

[Imports data](/cms/features/data-management) into your project. The imported data must originate from another Strapi application. You must pass the `--file` option to specify the filename and location for the import action.

```bash
strapi import
```

| Option         | Type   | Description                                                               |
| -------------- | ------ | ------------------------------------------------------------------------- |
| `-k,` `--key`  | string | Provide the encryption key in the command instead of a subsequent prompt. |
| `-f`, `--file` | string | Path and filename with extension for the data to be imported.             |
| `-h`, `--help` | -      | Display the `strapi import` help commands.                                |

**Examples**

```bash title="Example of strapi import:"

# import your data with the default parameters and pass an encryption key:
strapi import -f your-filepath-and-filename --key my-key
```

## strapi transfer

[Transfers data](/cms/data-management/transfer) between 2 Strapi instances. This command is primarily intended for use between a local instance and a remote instance or 2 remote instances. The `transfer` command requires a Transfer token, which is generated in the destination instance Admin panel. See the [User Guide](/cms/features/data-management#admin-panel-settings) for detailed documentation on creating Transfer tokens.

:::caution
The destination Strapi instance should be running with the `start` command and not the `develop` command.
:::

| Option                       | Description                                                                                                                                       |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--to [destinationURL]`      | Full URL of the `/admin` endpoint on the destination Strapi instance<br />(e.g. `--to https://my-beautiful-strapi-website/admin`)                 |
| `--to-token [transferToken]` | Transfer token for the remote Strapi destination                                                                                                  |
| `--from [sourceURL]`         | Full URL of the `/admin` endpoint of the remote Strapi instance to pull data from<br />(e.g., `--from https://my-beautiful-strapi-website/admin`) |
| `‑‑from‑token`               | Transfer token from the Strapi source instance.                                                                                                   |
| `--force`                    | Automatically answer "yes" to all prompts, including potentially destructive requests, and run non-interactively.                                 |
| `--exclude`                  | Exclude data using comma-separated data types. The available types are: `content`, `files`, and `config`.                                         |
| `--only`                     | Include only these data. The available types are: `content`, `files`, and `config`.                                                               |
| `-h`, `--help`               | Displays the commands for `strapi transfer`.                                                                                                      |

:::caution
Either `--to` or `--from` is required, but it's not currently allowed to enter both or neither.
:::

**Example**

```bash
strapi transfer --to http://example.com/admin --to-token my-transfer-token
```

## strapi report

Prints out debug information useful for debugging and required when reporting an issue.

| Option                 | Description                   |
| ---------------------- | ----------------------------- |
| `-u`, `--uuid`         | Includes the project UUID     |
| `-d`, `--dependencies` | Includes project dependencies |
| `--all`                | Logs all the data             |

**Examples**

To include the project UUID and dependencies in the output:

```bash
strapi report --uuid --dependencies
```

To log everything, use the `--all` option:

```bash
strapi report --all
```

## strapi configuration:dump

**Alias**: `config:dump`

Dumps configurations to a file or stdout to help you migrate to production.

The dump format will be a JSON array.

```bash title="strapi configuration:dump"

Options:
  -f, --file <file>  Output file, default output is stdout
  -p, --pretty       Format the output JSON with indentation and line breaks (default: false)
```

**Examples**

- `strapi configuration:dump -f dump.json`
- `strapi config:dump --file dump.json`
- `strapi config:dump > dump.json`

All these examples are equivalent.

:::caution
When configuring your application you often enter credentials for third party services (e.g authentication providers). Be aware that those credentials will also be dumped into the output of this command.
In case of doubt, you should avoid committing the dump file into a versioning system. Here are some methods you can explore:

- Copy the file directly to the environment you want and run the restore command there.
- Put the file in a secure location and download it at deploy time with the right credentials.
- Encrypt the file before committing and decrypt it when running the restore command.

:::

## strapi configuration:restore

**Alias**: `config:restore`

Restores a configuration dump into your application.

The input format must be a JSON array.

```bash
strapi configuration:restore

Options:
  -f, --file <file>          Input file, default input is stdin
  -s, --strategy <strategy>  Strategy name, one of: "replace", "merge", "keep". Defaults to: "replace"
```

**Examples**

- `strapi configuration:restore -f dump.json`
- `strapi config:restore --file dump.json -s replace`
- `cat dump.json | strapi config:restore`
- `strapi config:restore < dump.json`

All these examples are equivalent.

**Strategies**

When running the restore command, you can choose from three different strategies:

- **replace**: Will create missing keys and replace existing ones.
- **merge**: Will create missing keys and merge existing keys with their new value.
- **keep**: Will create missing keys and keep existing keys as is.

## strapi admin:create-user

**Alias** `admin:create`

Creates an administrator.
Administrator's first name, last name, email, and password can be:

- passed as options
- or set interactively if you call the command without passing any option.

**Example**

```bash

strapi admin:create-user --firstname=Kai --lastname=Doe --email=chef@strapi.io --password=Gourmet1234
```

**Options**

| Option          | Type   | Description                        | Required |
| --------------- | ------ | ---------------------------------- | -------- |
| -f, --firstname | string | The administrator's first name     | Yes      |
| -l, --lastname  | string | The administrator's last name      | No       |
| -e, --email     | string | The administrator's email          | Yes      |
| -p, --password  | string | New password for the administrator | No       |
| -h, --help      |        | display help for command           |          |

## strapi admin:reset-user-password

**Alias** `admin:reset-password`

Reset an admin user's password.
You can pass the email and new password as options or set them interactively if you call the command without passing the options.

**Example**

```bash

strapi admin:reset-user-password --email=chef@strapi.io --password=Gourmet1234
```

**Options**

| Option         | Type   | Description               |
| -------------- | ------ | ------------------------- |
| -e, --email    | string | The user email            |
| -p, --password | string | New password for the user |
| -h, --help     |        | display help for command  |

## strapi generate

Run a fully interactive CLI to generate APIs, [controllers](/cms/backend-customization/controllers), [content-types](/cms/backend-customization/models), [plugins](/cms/plugins-development/create-a-plugin), [policies](/cms/backend-customization/policies), [middlewares](/cms/backend-customization/middlewares) and [services](/cms/backend-customization/services), and [migrations](/cms/database-migrations).

```bash
strapi generate
```

## strapi templates:generate

Create a template from the current Strapi project.

```bash
strapi templates:generate <path>
```

- **strapi templates:generate &#60;path&#62;**<br/>
  Generates a Strapi template at `<path>`

  Example: `strapi templates:generate ../strapi-template-name` will copy the required files and folders to a `template` directory inside `../strapi-template-name`

## strapi ts:generate-types

Generate [TypeScript](/cms/typescript) typings for the project schemas.

```bash
strapi ts:generate-types
```

- **strapi ts:generate-types --debug**<br />
  Generate typings with the debug mode enabled, displaying a detailed table of the generated schemas.
- **strapi ts:generate-types --silent** or **strapi ts:generate-types -s**<br/>
  Generate typings with the silent mode enabled, completely removing all the logs in the terminal. Cannot be combined with `debug`
- **strapi ts:generate-types --out-dir &#60;path&#62;** or **strapi ts:generate-types -o &#60;path&#62;**<br/>
  Generate typings specifying the output directory in which the file will be created.

:::caution
Strapi requires the project types to be generated in the `types` directory for them to work. The `--out-dir` option should not be used for most cases. However, it can be useful for cases such as generating a second copy to compare the difference between your existing and updated types after changing your content structure.
:::

## strapi routes:list

Display a list of all the available [routes](/cms/backend-customization/routes).

```bash
strapi routes:list
```

## strapi policies:list

Display a list of all the registered [policies](/cms/backend-customization/policies).

```bash
strapi policies:list
```

## strapi middlewares:list

Display a list of all the registered [middlewares](/cms/backend-customization/middlewares).

```bash
strapi middlewares:list
```

## strapi content-types:list

Display a list of all the existing [content-types](/cms/backend-customization/models).

```bash
strapi content-types:list
```

## strapi hooks:list

Display a list of all the available hooks.

```bash
strapi hooks:list
```

## strapi controllers:list

Display a list of all the registered [controllers](/cms/backend-customization/controllers).

```bash
strapi controllers:list
```

## strapi services:list

Display a list of all the registered [services](/cms/backend-customization/services).

```bash
strapi services:list
```

## strapi telemetry:disable

Disable data collection for the project (see [Usage Information](/cms/usage-information)).

```bash
strapi telemetry:disable
```

## strapi telemetry:enable

Re-enable data collection for the project after it was disabled (see [Usage Information](/cms/usage-information)).

```bash
strapi telemetry:enable
```

## strapi console

Start the server and eval commands in your application in real time.

```bash
strapi console
```

## strapi version

Print the currently installed Strapi version.
It will output the current globally installed version if this command is strapi is installed globally, or the current version of Strapi within a Strapi project if the command is run from a given folder containing a Strapi project.

```bash
strapi version
```

## strapi help

List CLI commands.

```bash
strapi help
```
