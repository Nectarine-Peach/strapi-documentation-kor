---
title: 마켓플레이스를 통한 플러그인 설치
displayed_sidebar: cmsSidebar
sidebar_position: 2
tags:
- 플러그인
- 제공업체
- 마켓플레이스
- 업로드 플러그인
---

# 마켓플레이스 사용하기

Strapi는 [문서화](/cms/plugins/documentation), [GraphQL](/cms/plugins/graphql), [Sentry](/cms/plugins/sentry)와 같은 내장 플러그인이 함께 제공됩니다. 마켓플레이스는 사용자가 Strapi 애플리케이션을 커스터마이징할 수 있는 추가 플러그인과 플러그인을 확장할 수 있는 추가 제공업체를 찾을 수 있는 곳입니다. 마켓플레이스는 관리자 패널에서 <Icon name="shopping-cart" /> _마켓플레이스_로 표시됩니다. 마켓플레이스에서 사용자는 플러그인과 제공업체를 찾아보거나 검색하고, 각각에 대한 자세한 설명으로 이동하며, 새로운 플러그인과 제공업체를 제출할 수 있습니다.

:::note strapi 인앱 마켓플레이스 vs 마켓 웹사이트
관리자 패널의 마켓플레이스는 Strapi 버전에 관계없이 모든 기존 플러그인을 표시합니다. 모든 플러그인은 <ExternalLink to="https://market.strapi.io" text="Strapi Market"/> 웹사이트를 통해서도 찾을 수 있습니다.

하지만 v4와 v5 플러그인은 호환되지 않으며, 제공업체는 v4와 v5 플러그인 모두와 호환된다는 점을 유의하세요.
:::

<ThemedImage
  alt="마켓플레이스 인터페이스"
  sources={{
    light: '/img/assets/plugins/marketplace-plugins.png',
    dark: '/img/assets/plugins/marketplace-plugins_DARK.png',
  }}
/>

플러그인 및 제공업체 탭은 각 플러그인/제공업체를 다음 내용을 포함한 개별 카드로 표시합니다:

- 이름, 때로는 다음 배지 중 하나가 뒤따릅니다:
  - <img alt="Strapi에서 관리하는 아이콘" src="/img/strapi-logo.png" width="14px" style={{position: "relative", bottom:"2px", marginRight:"2px"}} /> Strapi에서 제작했음을 나타냄,
  - <Icon name="seal-check" color="rgb(58,115,66)" /> Strapi에서 검증했음을 나타냄.
- GitHub에서 플러그인/제공업체가 받은 스타 수와 다운로드 수
- 설명
- 추가 정보를 위해 마켓 웹사이트로 리디렉션되는 **더보기** <Icon name="arrow-square-out" /> 버튼(플러그인이 지원하는 Strapi 버전과 구현 지침 포함)

마켓플레이스 오른쪽 상단의 **플러그인 제출** 버튼은 자신만의 플러그인과 제공업체를 제출할 수 있는 Strapi Market으로 리디렉션됩니다.

:::tip 팁

- 검색창은 플러그인/제공업체 이름과 설명을 기반으로 점진적 검색 결과를 표시합니다.
- "정렬 기준" 버튼을 사용하거나 필터를 설정하여 플러그인을 더 쉽게 찾을 수 있습니다.

:::

## 마켓플레이스 플러그인 및 제공업체 설치

마켓플레이스를 통해 새 플러그인이나 제공업체를 설치하려면:

1. <Icon name="shopping-cart" /> *마켓플레이스*로 이동합니다.
2. **플러그인** 탭을 선택하여 사용 가능한 플러그인을 찾아보거나 **제공업체** 탭을 선택하여 사용 가능한 제공업체를 찾아봅니다.
3. 원하는 플러그인/제공업체를 선택하고 **더보기** <Icon name="arrow-square-out" /> 버튼을 클릭합니다.
4. Strapi Market 웹사이트로 리디렉션되면, 플러그인/제공업체별 구현 지침을 따릅니다.

:::strapi Strapi 플러그인 개발하기
사용 사례에 맞는 플러그인을 찾을 수 없나요? [직접 만들어보세요](/cms/plugins-development/developing-plugins)!
:::