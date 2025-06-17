---
title: 업그레이드 도구
description: Strapi 업그레이드 도구는 새로운 Strapi 버전으로 자동 업그레이드를 도와주는 CLI 명령어입니다.
displayed_sidebar: cmsSidebar
pagination_next: cms/migration/v4-to-v5/breaking-changes
sidebar_label: 업그레이드 도구 레퍼런스
tags:
- 주요 버전
- 마이너 버전
- 패치 버전
- 시맨틱 버전
- 업그레이드 도구
- 버전 종류
---

# 업그레이드 도구

업그레이드 도구는 Strapi 사용자가 애플리케이션의 의존성과 코드를 특정 버전으로 업그레이드할 수 있도록 도와줍니다.

업그레이드 도구를 실행하면 애플리케이션의 의존성이 업데이트 및 설치되고, 대상 버전까지의 브레이킹 체인지에 맞춰 **코드모드** <Codemods/>가 자동으로 코드베이스를 수정합니다.

업그레이드 도구는 Strapi 패키지로 제공되며, CLI에서 실행할 수 있습니다.

## 지원 범위

업그레이드 도구는 애플리케이션과 플러그인 업그레이드를 지원하지만, 모든 부분을 자동화하지는 않습니다.

:white_check_mark: 업그레이드 도구가 지원하는 것:
- 프로젝트 의존성 업데이트
- 기존 파일에 대한 자동 코드 변환 적용
- 프로젝트에 필요한 의존성 설치 또는 재설치

:x: 업그레이드 도구가 지원하지 않는 것:
- 파일 트리 구조 변경(파일/폴더 추가, 삭제, 이동)
- 애플리케이션 데이터 마이그레이션(이 부분은 Strapi 데이터베이스 마이그레이션이 담당)

:::warning
업그레이드 도구 실행 후, 앱이나 플러그인을 다시 실행하기 전에 변경된 내용을 반드시 검토하세요.
:::

## 버전 종류

Strapi 버전은 <ExternalLink to="https://semver.org/" text="시맨틱 버전(semver)"/> 규칙을 따릅니다:

<ThemedImage
  alt="버전 넘버 설명"
  sources={{
    light: '/img/assets/update-migration/version-numbers.png',
    dark: '/img/assets/update-migration/version-numbers_DARK.png',
  }}
/>

- 첫 번째 숫자는 **주요(major)** 버전입니다.
- 두 번째 숫자는 **마이너(minor)** 버전입니다.
- 세 번째 숫자는 **패치(patch)** 버전입니다.

업그레이드 도구는 주요, 마이너, 패치 버전 모두로 업그레이드할 수 있습니다.

업그레이드 도구의 동작은 현재 버전과 실행하는 명령어에 따라 달라집니다.

예를 들어, 최신 Strapi v4 버전이 v4.25.9라면:

| 내 Strapi 앱 현재 버전 | 실행 명령어 | 업그레이드 결과 |
|----------------------|--------------------------|----------------------------------------------------------|
| v4.25.1 | `npx @strapi/upgrade patch` | v4.25.9<br/>(해당 마이너 버전의 최신 패치로 업그레이드) |
| v4.14.1 | `npx @strapi/upgrade minor` | v4.25.9 |
| v4.14.1 | `npx @strapi/upgrade major` | 아무 일도 일어나지 않음.<br/>먼저 `npx @strapi/upgrade minor`로 v4.25.9까지 올려야 함. |
| v4.25.9 | `npx @strapi/upgrade major` | v5.0.0 |
| v4.14.1 | `npx @strapi/upgrade latest` | v5.1.2 <br/>주요 버전 업그레이드 의사 확인 프롬프트가 표시됨. |

## 새 버전으로 업그레이드하기

:::warning
업그레이드 전, 코드베이스와 데이터베이스의 백업을 반드시 생성하세요.
:::

### 주요(major) 버전 업그레이드

`major` 파라미터로 업그레이드 도구를 실행하면 프로젝트가 다음 주요 버전으로 업그레이드됩니다:

```bash
npx @strapi/upgrade major
```

업그레이드 과정에서 의존성이 업데이트 및 설치되고, 관련 코드모드가 실행됩니다.

:::note
현재 major의 최신 마이너/패치 버전이 아니라면, `major` 업그레이드는 차단되며, 먼저 [마이너/패치 업그레이드](#upgrade-to-a-minor-version)를 해야 합니다. 예를 들어 v4.14.4에서 v5.0.0으로 바로 올릴 수 없고, v4.16.2(최신 v4)까지 올린 뒤 major 업그레이드가 가능합니다.
:::

### 마이너(minor) 버전 업그레이드

`minor` 파라미터로 업그레이드 도구를 실행하면 프로젝트가 해당 major의 최신 마이너/패치 버전으로 업그레이드됩니다:

```bash
npx @strapi/upgrade minor
```

업그레이드 과정에서 의존성이 업데이트 및 설치되고, 관련 코드모드가 실행됩니다(있을 경우).

### 패치(patch) 버전 업그레이드

`patch` 파라미터로 업그레이드 도구를 실행하면 현재 major/minor의 최신 패치 버전으로 업그레이드됩니다:

```bash
npx @strapi/upgrade patch
```

업그레이드 과정에서 의존성이 업데이트 및 설치되고, 관련 코드모드가 실행됩니다(있을 경우).

### 최신 버전으로 업그레이드

`latest` 파라미터로 업그레이드 도구를 실행하면 현재 Strapi 버전에 상관없이 최신 버전으로 업그레이드됩니다:

```bash
npx @strapi/upgrade latest
```

업그레이드 과정에서 의존성이 업데이트 및 설치되고, 관련 코드모드가 실행됩니다(있을 경우).

:::note
`major` 버전 업그레이드가 감지되면, 업그레이드 도구가 의사 확인 프롬프트를 표시합니다.

주요 버전 업그레이드를 원하지 않는 경우, [마이너 업그레이드](#upgrade-to-a-minor-version)를 참고하세요.
:::

## 코드모드만 실행하기

`codemods` 파라미터로 업그레이드 도구를 실행하면, 실행할 코드모드를 선택할 수 있습니다. 이 명령어는 코드모드만 실행하며, 의존성은 업데이트/설치하지 않습니다.

사용 가능한 코드모드 목록을 보려면 `ls` 명령어를 사용하세요:

```bash
npx @strapi/upgrade codemods ls
```

코드모드를 선택해 실행하려면 `run` 명령어를 사용하세요:

```bash
npx @strapi/upgrade codemods run
```

특정 코드모드만 실행하려면, `ls` 명령어로 확인한 UID를 `run` 뒤에 붙여 실행하세요:

```bash
npx @strapi/upgrade codemods run 5.0.0-strapi-codemod-uid
```

## 옵션

`npx @strapi/upgrade [major|minor|patch]` 명령어는 다음 옵션을 지원합니다:

| 옵션 | 설명 | 기본값 |
|----------------|------------------------------------------------------|--------|
| [`-n, --dry`](#simulate-the-upgrade-without-updating-any-files-dry-run) | 실제 파일을 수정하지 않고 업그레이드를 시뮬레이션 | false |
| [`-d, --debug`](#get-detailed-debugging-information) | 디버그 모드로 더 많은 로그 출력 | false |
| [`-s, --silent`](#execute-the-upgrade-silently) | 아무 로그도 출력하지 않음 | false |
| [`-p, --project-path <project-path>`](#select-a-path-for-the-strapi-application-folder) | Strapi 프로젝트가 위치한 경로 지정 | - |
| [`-y, --yes`](#answer-yes-to-every-prompt) | 모든 프롬프트에 자동으로 "yes" 응답 | false |

아래 옵션들은 `npx @strapi/upgrade` 단독 또는 `[major|minor|patch]`와 함께 사용할 수 있습니다:

| 옵션 | 설명 |
|----------------|-------------------------------|
| [`-V, --version`](#get-the-current-version) | 현재 업그레이드 도구 버전 출력 |
| [`-h, --help`](#get-help) | 명령어 옵션 도움말 출력 |

### 실제 파일을 수정하지 않고 업그레이드 시뮬레이션(dry run)

`-n` 또는 `--dry` 옵션을 사용하면, 실제로 파일을 수정하지 않고 코드모드만 실행합니다. package.json도 수정되지 않고, 의존성도 재설치되지 않습니다. 이 옵션으로 코드베이스 업그레이드 결과를 미리 확인할 수 있습니다.

예시:

```bash
npx @strapi/upgrade major --dry
npx @strapi/upgrade minor --dry
npx @strapi/upgrade patch --dry
```

### 상세 디버깅 정보 출력

`-d` 또는 `--debug` 옵션을 사용하면 디버그 모드로 실행되어 더 자세한 로그가 출력됩니다.

예시:

```bash
npx @strapi/upgrade major --debug
```

### 조용히 업그레이드 실행

`-s` 또는 `--silent` 옵션을 사용하면 아무 로그도 출력하지 않고 업그레이드를 실행합니다.

예시:

```bash
npx @strapi/upgrade major --silent
```

### Strapi 애플리케이션 폴더 경로 지정

`-p` 또는 `--project-path` 옵션 뒤에 경로를 지정하면, 해당 폴더의 Strapi 애플리케이션을 대상으로 업그레이드할 수 있습니다.

예시:

```bash
npx @strapi/upgrade major -p /path/to/the/Strapi/application/folder
```

### 모든 프롬프트에 자동으로 "yes" 응답

`-y` 또는 `--yes` 옵션을 사용하면 업그레이드 과정의 모든 프롬프트에 자동으로 "yes"라고 응답합니다.

예시:

```bash
npx @strapi/upgrade major --yes
```

### 현재 버전 확인

`--version` 또는 `-V` 옵션을 사용하면, 업그레이드 도구의 현재 버전을 출력합니다.

예시:

```sh
$ npx @strapi/upgrade -V
4.15.1
```

### 도움말 보기

`-h` 또는 `--help` 옵션을 사용하면 업그레이드 도구의 사용법과 옵션을 출력합니다.

예시:

```bash
npx @strapi/upgrade --help
```
