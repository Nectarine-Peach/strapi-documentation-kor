---
title: CLI
displayed_sidebar: cmsSidebar
description: 1분 안에 컴퓨터에서 Strapi를 실행하기 위한 빠른 로컬 설치.
pagination_prev: cms/installation
pagination_next: cms/installation/docker
tags:
- 설치
- Command Line Interface (CLI)
- 데이터베이스
- MySQL
- PostgreSQL
---

import InstallPrerequisites from '/docs/snippets/installation-prerequisites.md'
import SupportedDatabases from '/docs/snippets/supported-databases.md'

# CLI로 설치하기

Strapi CLI(Command Line Interface) 설치 스크립트는 Strapi를 로컬에서 실행하는 가장 빠른 방법입니다. 다음 가이드는 Strapi에서 가장 권장하는 설치 옵션입니다.

## 설치 준비

<InstallPrerequisites components={props.components} />
모든 Strapi 프로젝트에는 지원되는 데이터베이스도 필요합니다:
<SupportedDatabases components={props.components} />

## Strapi 프로젝트 생성

새로운 Strapi 프로젝트를 만들려면 아래 단계를 따르되, 설치된 패키지 매니저에 맞는 적절한 명령어를 사용하세요:

1. 터미널에서 다음 명령어를 실행합니다:

    <Tabs groupId="yarn-npm">

    <TabItem value="npm" label="NPM">

    ```bash
    npx create-strapi@latest
    ```

    <details>
    <summary>명령어에 대한 추가 설명:</summary>

    * `npx`는 npm 패키지에서 명령어를 실행합니다
    * `create-strapi`는 Strapi 패키지입니다
    * `@latest`는 최신 버전의 Strapi가 사용됨을 나타냅니다
    
    <br/>

    npx 대신 기존 npm 명령어도 `npm create strapi@latest`로 사용할 수 있습니다.

    npx 사용 시 create와 strapi 사이의 추가 대시에 주의하세요: `npx create-strapi` vs. `npm create strapi`.
    </details>
    
    </TabItem>

    <TabItem value="yarn" label="Yarn">

    ```bash
    yarn create strapi
   
    ```

    :::note
    Yarn은 npm과 달리 `@latest`와 같은 버전 태그 전달을 지원하지 않습니다. yarn을 사용했는데 예상과 다른 결과가 나오고 최신 버전의 Strapi가 설치되지 않았다면, <ExternalLink to="https://yarnpkg.com/cli/cache/clean" text="`yarn cache clean` 명령어를 실행"/>하여 Yarn 캐시를 정리해야 할 수 있습니다.
    :::

    </TabItem>

    <TabItem value="pnpm" label="pnpm">

    :::caution
    Strapi Cloud에서 pnpm으로 생성된 프로젝트에 문제가 있을 수 있습니다. Strapi Cloud는 아직 pnpm을 지원하지 않으므로, 결국 Strapi Cloud에서 프로젝트를 호스팅할 계획이라면 yarn이나 npm을 사용하는 것이 좋습니다.
    :::

    ```bash
    pnpm create strapi
    ```
    
    </TabItem>

    </Tabs>

2. 터미널에서 `Login/Signup` 또는 `Skip` 중 어느 것을 선택할지 묻습니다. 화살표 키를 사용하고 `Enter`를 눌러 선택하세요. 로그인을 선택하면 생성된 프로젝트에 자동으로 적용되는 <GrowthBadge /> 플랜의 30일 체험판을 받게 됩니다. 이 단계를 건너뛰면 프로젝트는 CMS Free 플랜으로 되돌아갑니다.

3. 터미널에서 몇 가지 질문을 합니다. 각 질문에 대해 입력하는 대신 `Enter`를 누르면 기본 답변(Yes)이 사용됩니다:

  ![설치 시 터미널 프롬프트](/img/assets/installation/prompts.png)

  :::tip
  설치 명령어에 전달되는 다양한 옵션을 사용하여 이러한 질문들을 건너뛸 수 있습니다. 사용 가능한 모든 옵션의 전체 목록은 [표](#cli-installation-options)를 참조하세요.
  :::

4. _(선택사항)_ 기본(SQLite) 데이터베이스 질문에 "no"인 `n`으로 답했다면, CLI가 데이터베이스에 대한 추가 질문을 합니다:

    * 화살표 키를 사용하여 원하는 데이터베이스 타입을 선택한 다음 `Enter`를 누르세요.
    * 데이터베이스 이름을 지정하고, 데이터베이스 호스트 주소와 포트를 정의하고, 데이터베이스 관리자 사용자명과 비밀번호를 정의하고, 데이터베이스가 SSL 연결을 사용할지 정의하세요.<br/>이러한 질문 중 어느 것에서든 아무것도 입력하지 않고 `Enter`를 누르면, 기본값(터미널 출력에서 괄호 안에 표시됨)이 사용됩니다.

모든 질문에 답하면 스크립트가 Strapi 프로젝트 생성을 시작합니다.

### CLI 설치 옵션

위의 설치 가이드는 CLI를 사용한 기본 설치 옵션만 다룹니다. 새로운 Strapi 프로젝트를 생성할 때 사용할 수 있는 다른 옵션들이 있습니다. 예를 들어:

| 옵션                              | 설명                                                                                                                                                                                                                                     |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--no-run`                          | 생성 후 애플리케이션을 시작하지 않습니다                                                                                                                                                                                                                |
| `--ts`<br/>`--typescript`           | TypeScript로 프로젝트를 초기화합니다 (기본값)                                                                                                                                                                                                |
| `--js`<br/>`--javascript`           | JavaScript로 프로젝트를 초기화합니다                                                                                                                                                                                                          |
| `--use-npm`                         | <ExternalLink to="https://www.npmjs.com/" text="npm"/>을 프로젝트 패키지 매니저로 강제 사용합니다                                                                                                                                        |
| `--use-yarn`                        | <ExternalLink to="https://yarnpkg.com/" text="yarn"/>을 프로젝트 패키지 매니저로 강제 사용합니다                                                                                                                                         |
| `--use-pnpm`                        | <ExternalLink to="https://pnpm.io/" text="pnpm"/>을 프로젝트 패키지 매니저로 강제 사용합니다                                                                                                                                             |
| `--install`                         | 모든 종속성을 설치하고, 관련 CLI 프롬프트를 건너뜁니다                                                                                                                                                                                       |
| `--no-install`                      | 모든 종속성을 설치하지 않고, 관련 CLI 프롬프트를 건너뜁니다                                                                                                                                                                                |
| `--git-init`                        | git 저장소를 초기화하고, 관련 CLI 프롬프트를 건너뜁니다                                                                                                                                                                                    |
| `--no-git-init`                     | git 저장소를 초기화하지 않고, 관련 CLI 프롬프트를 건너뜁니다                                                                                                                                                                             |
| `--example`                         | 예제 데이터를 추가하고, 관련 CLI 프롬프트를 건너뜁니다                                                                                                                                                                                               |
| `--no-example`                      | 예제 데이터를 추가하지 않고, 관련 CLI 프롬프트를 건너뜁니다                                                                                                                                                                                        |
| `--skip-cloud`                      | [Strapi 로그인 및 프로젝트 생성 단계](#skipping-the-strapi-login-step)를 건너뜁니다                                                                                                                                                     |
| `--skip-db`                         | 모든 데이터베이스 관련 프롬프트를 건너뛰고 기본(SQLite) 데이터베이스로 프로젝트를 생성합니다                                                                                                                                                       |
| `--template <template-name-or-url>` | 주어진 템플릿을 기반으로 애플리케이션을 생성합니다.<br/>템플릿에 대한 추가 옵션이 있습니다. 자세한 내용은 [템플릿 문서](/cms/templates)를 참조하세요.                                                                            |
| `--dbclient <dbclient>`             | 명령어에서 `<dbclient>`를 다음 값 중 하나로 바꿔서 사용할 데이터베이스 클라이언트를 정의합니다:<ul><li>`sql` (SQLite 데이터베이스, 기본값)</li><li>`postgres` (PostgreSQL 데이터베이스)</li><li>`mysql` (MySQL 데이터베이스)</li></ul> |
| `--dbhost <dbhost>`                 | 명령어에서 `<dbhost>`를 원하는 값으로 바꿔서 사용할 데이터베이스 호스트를 정의합니다                                                                                                                                              |
| `--dbport <dbport>`                 | 명령어에서 `<dbport>`를 원하는 값으로 바꿔서 사용할 데이터베이스 포트를 정의합니다                                                                                                                                              |
| `--dbname <dbname>`                 | 명령어에서 `<dbname>`을 원하는 값으로 바꿔서 사용할 데이터베이스 이름을 정의합니다                                                                                                                                              |
| `--dbusername <dbusername>`         | 명령어에서 `<dbusername>`을 원하는 값으로 바꿔서 사용할 데이터베이스 사용자명을 정의합니다                                                                                                                                      |
| `--dbpassword <dbpassword>`         | 명령어에서 `<dbpassword>`를 원하는 값으로 바꿔서 사용할 데이터베이스 비밀번호를 정의합니다                                                                                                                                      |
| `--dbssl <dbssl>`                   | `--dbssl=true`를 전달하여 데이터베이스에서 SSL이 사용됨을 정의합니다 (기본적으로 SSL 없음)                                                                                                                                                        |
| `--dbfile <dbfile>`                 | SQLite 데이터베이스의 경우, 명령어에서 `<dbclient>`를 원하는 값으로 바꿔서 사용할 데이터베이스 파일 경로를 정의합니다                                                                                                                 |
| `--quickstart`                      | (**Strapi 5에서 폐기됨**)<br/>quickstart 모드에서 직접 프로젝트를 생성합니다.                                                                                                                                                                |

:::note 참고사항
* `--use-yarn|npm|pnpm` 옵션을 전달하지 않으면, 설치 스크립트는 create 명령어에 사용된 패키지 매니저를 사용하여 모든 종속성을 설치합니다(예: `npm create strapi`는 npm으로 프로젝트의 모든 종속성을 설치합니다).
* 데이터베이스 구성에 대한 추가 정보는 [데이터베이스 구성 문서](/cms/configurations/database)를 참조하세요.
* 실험적 Strapi 버전은 매주 화요일부터 토요일까지 GMT 자정에 릴리스됩니다. `npx create-strapi@experimental`을 사용하여 최신 실험적 릴리스를 기반으로 새로운 Strapi 애플리케이션을 생성할 수 있습니다. 이러한 실험적 빌드는 본인의 책임하에 사용하세요. 프로덕션에서 사용하는 것은 권장되지 않습니다.
:::

### Strapi 로그인 단계 건너뛰기

설치 스크립트가 실행되면 터미널에서 먼저 로그인/가입 여부를 묻습니다. `Login/signup`을 선택하면 생성된 프로젝트에 자동으로 적용되는 <GrowthBadge tooltip="CMS Growth 플랜에는 Live Preview, Releases, Content History 기능이 포함됩니다."/>의 30일 체험판을 제공받습니다. 이를 통해 고급 CMS 기능에 접근할 수 있습니다.

이 Strapi 로그인 부분을 건너뛰고 싶다면 화살표 키를 사용하여 `Skip`을 선택하세요. 스크립트가 계속되어 CMS Free 플랜을 사용하는 로컬 프로젝트를 생성합니다.

나중에 [가격 페이지](https://strapi.io/pricing-self-hosted)를 확인하여 CMS 라이선스를 구매할 수 있습니다.

### 프로젝트 호스팅

무료 [Strapi Cloud](/cloud/intro) 프로젝트를 생성할 수 있습니다. 이 프로젝트를 배포하고 온라인으로 호스팅하려면 다음을 선택할 수 있습니다:

- [배포 가이드](/cms/deployment)를 따르기 전에 프로젝트 코드를 저장소(예: GitHub)에 푸시하여 직접 호스팅하거나,
- [Cloud CLI](/cloud/cli/cloud-cli#) 명령어를 사용하여 Strapi Cloud에 로그인하고 무료로 프로젝트를 배포합니다.

프로젝트를 직접 호스팅하고 싶지만 GitHub에 익숙하지 않다면, 다음 토글 가능한 콘텐츠가 시작하는 데 도움이 될 것입니다👇.

<details>
<summary>Strapi 프로젝트 코드를 GitHub에 푸시하는 데 필요한 단계:</summary>

1. 터미널에서 생성한 Strapi 프로젝트를 호스팅하는 폴더에 있는지 확인하세요.
2. `git init` 명령어를 실행하여 이 폴더에 대해 git을 초기화하세요.
3. `git add .` 명령어를 실행하여 수정된 모든 파일을 git 인덱스에 추가하세요.
4. `git commit -m "Initial commit"` 명령어를 실행하여 추가된 모든 변경사항으로 커밋을 생성하세요.
5. GitHub 계정에 로그인하고 <ExternalLink to="https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories" text="새 저장소를 생성"/>하세요. 새 저장소에 이름을 지정합니다(예: `my-first-strapi-project`). 이 이름을 기억하세요.
6. 터미널로 돌아가서 로컬 저장소를 GitHub에 푸시하세요:

  a. 다음과 유사한 명령어를 실행하세요: `git remote add origin git@github.com:yourname/my-first-strapi-project.git`. `yourname`을 본인의 GitHub 프로필 이름으로, `my-first-strapi-project`를 4단계에서 사용한 실제 이름으로 바꿔주세요.

  b. `git push --set-upstream origin main` 명령어를 실행하여 마지막으로 커밋을 GitHub 저장소에 푸시하세요.

명령줄 인터페이스로 git을 사용하는 것에 대한 추가 정보는 <ExternalLink to="https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github#adding-a-local-repository-to-github-using-git" text="공식 GitHub 문서"/>에서 찾을 수 있습니다.

</details>

## Strapi 실행

Strapi 애플리케이션을 시작하려면 프로젝트 폴더에서 다음 명령어를 실행하세요:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="Yarn">

```bash
yarn develop
```

</TabItem>

<TabItem value="npm" label="NPM">

```bash
npm run develop
```

</TabItem>

</Tabs>

:::info 내 콘텐츠는 어디에 있나요?
자체 호스팅 Strapi 프로젝트의 경우, 모든 콘텐츠는 프로젝트 폴더의 `.tmp` 하위 폴더에 있는 데이터베이스 파일(기본적으로 SQLite)에 저장됩니다. 따라서 Strapi 프로젝트를 생성한 폴더에서 Strapi 애플리케이션을 시작할 때마다 콘텐츠를 사용할 수 있습니다([데이터베이스 구성](/cms/configurations/database)에서 추가 정보 참고).
:::
