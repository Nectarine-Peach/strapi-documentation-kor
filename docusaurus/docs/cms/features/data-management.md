---
title: 데이터 관리
sidebar_position: 1
description: 데이터 관리 기능을 사용해 서로 다른 Strapi 인스턴스 간 데이터 가져오기, 내보내기, 전송을 수행하는 방법을 알아보세요.
toc_max_heading_level: 5
tags:
- 관리자 패널
- 기능
- 데이터 관리
- 데이터 가져오기
- 데이터 내보내기
- 데이터 전송
---

# 데이터 관리

데이터 관리 기능은 데이터를 가져오거나 내보내거나, 서로 다른 Strapi 인스턴스 간에 전송할 때 사용할 수 있습니다. 데이터 관리는 CLI 기반으로 동작하지만, 일부 설정은 관리자 패널에서 진행합니다.

<IdentityCard>
  <IdentityCardItem icon="credit-card" title="플랜">무료 기능</IdentityCardItem>
  <IdentityCardItem icon="user" title="역할 및 권한">Roles > Settings - Transfer tokens에서 최소 "Access the transfer tokens settings page" 권한 필요</IdentityCardItem>
  <IdentityCardItem icon="toggle-right" title="활성화">transfer salt가 정의된 경우 사용 가능</IdentityCardItem>
  <IdentityCardItem icon="desktop" title="환경">개발 및 프로덕션 환경 모두에서 사용 가능</IdentityCardItem>
</IdentityCard>

## 설정

데이터 관리 기능의 일부 설정은 관리자 패널에서, 일부는 프로젝트 코드에서 관리할 수 있습니다.

### 관리자 패널 설정

:::prerequisites
`config/admin` 설정 파일에 `transfer.token.salt`가 정의되어 있어야 합니다([코드 기반 설정](#code-based-configuration) 참고).
:::

**기능 설정 경로:** <Icon name="gear-six" /> *설정 > 글로벌 설정 > Transfer Tokens*

Transfer token을 사용하면 `strapi transfer` CLI 명령어를 인증할 수 있습니다([데이터 전송](/cms/data-management/transfer) 문서 참고).

<ThemedImage
  alt="Transfer tokens"
  sources={{
    light: '/img/assets/settings/settings_transfer-token.png',
    dark: '/img/assets/settings/settings_transfer-token_DARK.png',
  }}
/>

The *Transfer Tokens* interface displays a table listing all of the created Transfer tokens. More specifically, it displays each Transfer token's name, description, date of creation, and date of last use.

From there, administrators can also:

- Click on the <Icon name="pencil-simple" /> to edit a transfer token's name, description, or type, or [regenerate the token](#regenerating-a-transfer-token).
- Click on the <Icon name="trash" /> to delete a Transfer token.

#### Creating a new transfer token

1. Click on the **Create new Transfer Token** button.
2. In the Transfer token edition interface, configure the new Transfer token:
    | Setting name   | Instructions                                                                  |
    | -------------- | ----------------------------------------------------------------------------- |
    | Name           | Write the name of the Transfer token.                                         |
    | Description    | (optional) Write a description for the Transfer token.                        |
    | Token duration | Choose a token duration: *7 days*, *30 days*, *90 days*, or *Unlimited*.      |
    | Token type | Choose a token type:<ul><li>*Push* to allow transfers from local to remote instances only,</li><li>*Pull* to allow transfers from remote to local instances only,</li><li>or *Full Access* to allow both types of transfer.</li></ul>      |
3. Click on the **Save** button. The new Transfer token will be displayed at the top of the interface, along with a copy button <Icon name="copy" />.

<ThemedImage
  alt="Custom Transfer Token"
  sources={{
    light: '/img/assets/settings/settings_create-transfer-token.png',
    dark: '/img/assets/settings/settings_create-transfer-token_DARK.png',
  }}
/>

:::caution
For security reasons, Transfer tokens are only shown right after they have been created. When refreshing the page or navigating elsewhere in the admin panel, the newly created Transfer token will be hidden and will not be displayed again.
:::

#### Regenerating a Transfer token

1. Click on the Transfer token's edit button.
2. Click on the **Regenerate** button.
3. Click on the **Regenerate** button to confirm in the dialog.
4. Copy the new Transfer token displayed at the top of the interface.

### Code-based configuration

A `transfer.token.salt` value must be defined in [the `config/admin` file](/cms/configurations/admin-panel) so that transfer tokens can be properly generated. If no value is defined, the feature will be disabled. The salt can be any long string, and for increased security, it's best to add it to the [environment variables](/cms/configurations/environment) and import it using the `env()` utility as in the following example:

<Tabs groupId="js-ts">

<TabItem value="javascript" label="JavaScript">

```js title="/config/admin.js"
module.exports = ({ env }) => ({
  // …
  transfer: { 
    token: { 
      salt: env('TRANSFER_TOKEN_SALT', 'anotherRandomLongString'),
    } 
  },
});

```

</TabItem>

<TabItem value="typescript" label="TypeScript">

```ts title="/config/admin.ts"
export default ({ env }) => ({
  // …
  transfer: { 
    token: { 
      salt: env('TRANSFER_TOKEN_SALT', 'anotherRandomLongString'),
    } 
  },
});
```

</TabItem>

</Tabs>

## Usage

The Data Management system is CLI-based only, meaning any import, export, or transfer command must be executed from the terminal. Exhaustive documentation for each command is accessible from the following pages:

<CustomDocCardsWrapper>
<CustomDocCard icon="terminal" title="Import" description="Learn how to import data into a Strapi instance." link="/cms/data-management/import"/>
<CustomDocCard icon="terminal" title="Export" description="Learn how to export data from a Strapi instance." link="/cms/data-management/export"/>
<CustomDocCard icon="terminal" title="Transfer" description="Learn how to transfer data from a Strapi instance to another one." link="/cms/data-management/transfer"/>
</CustomDocCardsWrapper>
