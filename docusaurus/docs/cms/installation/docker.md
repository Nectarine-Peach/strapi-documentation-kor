---
title: Docker
displayed_sidebar: cmsSidebar
pagination_prev: cms/installation/cli
pagination_next: cms/features/admin-panel
description: 로컬 프로젝트에서 빠르게 Docker 컨테이너를 생성하세요.
tags:
- 설치
- 환경 
- MySQL
---

import DockerEnvTable from '/docs/snippets/docker-env-table.md'

# Docker 컨테이너에서 Strapi 실행하기

:::caution
Strapi는 공식 컨테이너 이미지를 빌드하지 않습니다. 다음 지침은 커뮤니티에 대한 예의로 제공됩니다. 질문이 있으시면 <ExternalLink to="https://discord.strapi.io" text="Discord"/>로 문의하세요.
:::

:::danger
 Strapi 애플리케이션은 Strapi 애플리케이션에서 생성되지 않은 기존 데이터베이스나 Strapi v3 데이터베이스에 연결하도록 설계되지 않았습니다. Strapi 팀은 이러한 시도를 지원하지 않습니다. 지원되지 않는 데이터베이스에 연결을 시도하면 테이블 삭제와 같은 데이터 손실이 발생할 가능성이 높습니다.
:::

다음 문서는 기존 Strapi 프로젝트로 커스텀 <ExternalLink to="https://www.docker.com/" text="Docker"/> 컨테이너를 빌드하는 방법을 안내합니다.

Docker는 컨테이너(즉, 라이브러리 및 종속성과 같이 애플리케이션이 작동하는 데 필요한 모든 부분을 포함하는 패키지)를 사용하여 애플리케이션을 개발, 배송 및 실행할 수 있는 오픈 플랫폼입니다. 컨테이너는 서로 격리되어 있으며 자체 소프트웨어, 라이브러리 및 구성 파일을 번들로 제공합니다. 잘 정의된 채널을 통해 서로 통신할 수 있습니다.

:::prerequisites

- 머신에 <ExternalLink to="https://www.docker.com/" text="Docker"/> 설치
- [지원되는 Node.js 버전](/cms/installation/cli#preparing-the-installation)
- **기존 Strapi 5 프로젝트** 또는 [빠른 시작 가이드](/cms/quick-start.md)로 생성한 새 프로젝트
- _(선택사항)_ 머신에 <ExternalLink to="https://yarnpkg.com/" text="Yarn"/> 설치
- _(선택사항)_ 머신에 <ExternalLink to="https://docs.docker.com/compose/" text="Docker Compose"/> 설치

:::

## 개발 및/또는 스테이징 환경

호스트 머신에서 Strapi와 로컬로 작업하려면 <ExternalLink to="https://docs.docker.com/engine/reference/builder/" text="Dockerfile"/>을 사용할 수 있으며, 필요한 경우 <ExternalLink to="https://docs.docker.com/compose/compose-file/" text="docker-compose.yml"/>을 사용하여 데이터베이스 컨테이너를 시작할 수도 있습니다.

두 방법 모두 기존 Strapi 프로젝트가 있거나 새로 생성된 프로젝트가 필요합니다([빠른 시작 가이드](/cms/quick-start.md) 참고).

### 개발용 Dockerfile

다음 `Dockerfile`은 Strapi 프로젝트의 비프로덕션 Docker 이미지를 빌드하는 데 사용할 수 있습니다.

:::note

`docker-compose`를 사용하는 경우 환경 변수를 수동으로 설정할 필요가 없습니다. `docker-compose.yml` 파일이나 `.env` 파일에서 설정됩니다.

:::

<DockerEnvTable components={props.components} />

`Dockerfile` 및 그 명령어에 대한 자세한 정보는 <ExternalLink to="https://docs.docker.com/engine/reference/commandline/cli/" text="공식 Docker 문서"/>를 참조하세요.

샘플 `Dockerfile`:

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```dockerfile title="./Dockerfile"
FROM node:22-alpine
# Sharp 호환성을 위한 libvips-dev 설치
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev nasm bash vips-dev git
ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}

WORKDIR /opt/
COPY package.json yarn.lock ./
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install
ENV PATH /opt/node_modules/.bin:$PATH

WORKDIR /opt/app
COPY . .
RUN chown -R node:node /opt/app
USER node
RUN ["yarn", "build"]
EXPOSE 1337
CMD ["yarn", "develop"]
```

</TabItem>

<TabItem value="npm" label="npm">

```dockerfile title="./Dockerfile"
FROM node:22-alpine
# Sharp 호환성을 위한 libvips-dev 설치
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev nasm bash vips-dev git
ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}

WORKDIR /opt/
COPY package.json package-lock.json ./
RUN npm install -g node-gyp
RUN npm config set fetch-retry-maxtimeout 600000 -g && npm install
ENV PATH=/opt/node_modules/.bin:$PATH

WORKDIR /opt/app
COPY . .
RUN chown -R node:node /opt/app
USER node
RUN ["npm", "run", "build"]
EXPOSE 1337
CMD ["npm", "run", "develop"]

```

</TabItem>

</Tabs>

### (선택사항) Docker Compose

다음 `docker-compose.yml`은 데이터베이스 컨테이너와 Strapi 컨테이너, 그리고 두 컨테이너 간 통신을 위한 공유 네트워크를 시작하는 데 사용할 수 있습니다.

:::note
Docker compose 실행 및 그 명령어에 대한 자세한 정보는 <ExternalLink to="https://docs.docker.com/compose/" text="Docker Compose 문서"/>를 참조하세요.
:::

샘플 `docker-compose.yml`:

<Tabs groupId="databases">

<TabItem value="mysql" label="MySQL">

```yml title="./docker-compose.yml"
services:
  strapi:
    container_name: strapi
    build: .
    image: strapi:latest
    restart: unless-stopped
    env_file: .env
    environment:
      DATABASE_CLIENT: ${DATABASE_CLIENT}
      DATABASE_HOST: strapiDB
      DATABASE_PORT: ${DATABASE_PORT}
      DATABASE_NAME: ${DATABASE_NAME}
      DATABASE_USERNAME: ${DATABASE_USERNAME}
      DATABASE_PASSWORD: ${DATABASE_PASSWORD}
      JWT_SECRET: ${JWT_SECRET}
      ADMIN_JWT_SECRET: ${ADMIN_JWT_SECRET}
      APP_KEYS: ${APP_KEYS}
      NODE_ENV: ${NODE_ENV}
    volumes:
      - ./config:/opt/app/config
      - ./src:/opt/app/src
      - ./package.json:/opt/package.json
      - ./yarn.lock:/opt/yarn.lock
      - ./.env:/opt/app/.env
      - ./public/uploads:/opt/app/public/uploads
    ports:
      - "1337:1337"
    networks:
      - strapi
    depends_on:
      - strapiDB

  strapiDB:
    container_name: strapiDB
    platform: linux/amd64 #Apple M1 칩의 플랫폼 오류용
    restart: unless-stopped
    env_file: .env
    image: mysql:8.0
    command: --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_USER: ${DATABASE_USERNAME}
      MYSQL_ROOT_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_DATABASE: ${DATABASE_NAME}
    volumes:
      - strapi-data:/var/lib/mysql
      #- ./data:/var/lib/mysql # 바인드 폴더를 사용하려는 경우
    ports:
      - "3306:3306"
    networks:
      - strapi

volumes:
  strapi-data:

networks:
  strapi:
    name: Strapi
    driver: bridge
```

</TabItem>

<TabItem value="mariadb" label="MariaDB">

```yml title="./docker-compose.yml"
version: "3"
services:
  strapi:
    container_name: strapi
    build: .
    image: strapi:latest
    restart: unless-stopped
    env_file: .env
    environment:
      DATABASE_CLIENT: ${DATABASE_CLIENT}
      DATABASE_HOST: strapiDB
      DATABASE_PORT: ${DATABASE_PORT}
      DATABASE_NAME: ${DATABASE_NAME}
      DATABASE_USERNAME: ${DATABASE_USERNAME}
      DATABASE_PASSWORD: ${DATABASE_PASSWORD}
      JWT_SECRET: ${JWT_SECRET}
      ADMIN_JWT_SECRET: ${ADMIN_JWT_SECRET}
      APP_KEYS: ${APP_KEYS}
      NODE_ENV: ${NODE_ENV}
    volumes:
      - ./config:/opt/app/config
      - ./src:/opt/app/src
      - ./package.json:/opt/package.json
      - ./yarn.lock:/opt/yarn.lock
      - ./.env:/opt/app/.env
      - ./public/uploads:/opt/app/public/uploads
    ports:
      - "1337:1337"
    networks:
      - strapi
    depends_on:
      - strapiDB

  strapiDB:
    container_name: strapiDB
    platform: linux/amd64 #for platform error on Apple M1 chips
    restart: unless-stopped
    env_file: .env
    image: mariadb:latest
    environment:
      MYSQL_USER: ${DATABASE_USERNAME}
      MYSQL_ROOT_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_DATABASE: ${DATABASE_NAME}
    volumes:
      - strapi-data:/var/lib/mysql
      #- ./data:/var/lib/mysql # if you want to use a bind folder
    ports:
      - "3306:3306"
    networks:
      - strapi

volumes:
  strapi-data:

networks:
  strapi:
    name: Strapi
    driver: bridge
```

</TabItem>

<TabItem value="postgresql" label="PostgreSQL">

```yml title="./docker-compose.yml"
version: "3"
services:
  strapi:
    container_name: strapi
    build: .
    image: strapi:latest
    restart: unless-stopped
    env_file: .env
    environment:
      DATABASE_CLIENT: ${DATABASE_CLIENT}
      DATABASE_HOST: strapiDB
      DATABASE_PORT: ${DATABASE_PORT}
      DATABASE_NAME: ${DATABASE_NAME}
      DATABASE_USERNAME: ${DATABASE_USERNAME}
      DATABASE_PASSWORD: ${DATABASE_PASSWORD}
      JWT_SECRET: ${JWT_SECRET}
      ADMIN_JWT_SECRET: ${ADMIN_JWT_SECRET}
      APP_KEYS: ${APP_KEYS}
      NODE_ENV: ${NODE_ENV}
    volumes:
      - ./config:/opt/app/config
      - ./src:/opt/app/src
      - ./package.json:/opt/package.json
      - ./yarn.lock:/opt/yarn.lock
      - ./.env:/opt/app/.env
      - ./public/uploads:/opt/app/public/uploads
    ports:
      - "1337:1337"
    networks:
      - strapi
    depends_on:
      - strapiDB

  strapiDB:
    container_name: strapiDB
    platform: linux/amd64 #for platform error on Apple M1 chips
    restart: unless-stopped
    env_file: .env
    image: postgres:16.0-alpine
    environment:
      POSTGRES_USER: ${DATABASE_USERNAME}
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD}
      POSTGRES_DB: ${DATABASE_NAME}
    volumes:
      - strapi-data:/var/lib/postgresql/data/ #using a volume
      #- ./data:/var/lib/postgresql/data/ # if you want to use a bind folder

    ports:
      - "5432:5432"
    networks:
      - strapi

volumes:
  strapi-data:

networks:
  strapi:
    name: Strapi
    driver: bridge
```

</TabItem>

</Tabs>

## Production Environments

The Docker image in production is different from the one used in development/staging environments because of the differences in the admin build process in addition to the command used to run the application. Typical production environments will use a reverse proxy to serve the application and the admin panel. The Docker image is built with the production build of the admin panel and the command used to run the application is `strapi start`.

Once the [Dockerfile](#production-dockerfile) is created, the [production container](#building-the-production-container) can be built. Optionally, the container can be published to a [registry](#optional-publishing-the-container-to-a-registry) to make it available to the community. [Community tools](#community-tools) can help you
in the process of building a production Docker image and deploying it to a production environment.

### Production Dockerfile

The following `Dockerfile` can be used to build a production Docker image for a Strapi project.

<Tabs groupId="yarn-npm">

<TabItem value="yarn" label="yarn">

```dockerfile title="./Dockerfile.prod"
# Creating multi-stage build for production
FROM node:22-alpine AS build
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev vips-dev git > /dev/null 2>&1
ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /opt/
COPY package.json yarn.lock ./
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install --production
ENV PATH=/opt/node_modules/.bin:$PATH
WORKDIR /opt/app
COPY . .
RUN yarn build

# Creating final production image
FROM node:22-alpine
RUN apk add --no-cache vips-dev
ENV NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}
WORKDIR /opt/
COPY --from=build /opt/node_modules ./node_modules
WORKDIR /opt/app
COPY --from=build /opt/app ./
ENV PATH=/opt/node_modules/.bin:$PATH

RUN chown -R node:node /opt/app
USER node
EXPOSE 1337
CMD ["yarn", "start"]
```

</TabItem>

<TabItem value="npm" label="npm">

```dockerfile title="./Dockerfile.prod"
# Creating multi-stage build for production
FROM node:22-alpine AS build
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev vips-dev git > /dev/null 2>&1
ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /opt/
COPY package.json package-lock.json ./
RUN npm install -g node-gyp
RUN npm config set fetch-retry-maxtimeout 600000 -g && npm install --only=production
ENV PATH=/opt/node_modules/.bin:$PATH
WORKDIR /opt/app
COPY . .
RUN npm run build

# Creating final production image
FROM node:22-alpine
RUN apk add --no-cache vips-dev
ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}
WORKDIR /opt/
COPY --from=build /opt/node_modules ./node_modules
WORKDIR /opt/app
COPY --from=build /opt/app ./
ENV PATH=/opt/node_modules/.bin:$PATH

RUN chown -R node:node /opt/app
USER node
EXPOSE 1337
CMD ["npm", "run", "start"]

```

</TabItem>

</Tabs>

### Building the production container

Building production Docker images can have several options. The following example uses the `docker build` command to build a production Docker image for a Strapi project. However, it is recommended you review the <ExternalLink to="https://docs.docker.com/engine/reference/commandline/build/" text="Docker documentation"/> for more information on building Docker images with more advanced options.

To build a production Docker image for a Strapi project, run the following command:

```bash
docker build \
  --build-arg NODE_ENV=production \
  # --build-arg STRAPI_URL=https://api.example.com \ # Uncomment to set the Strapi Server URL
  -t mystrapiapp:latest \ # Replace with your image name
  -f Dockerfile.prod .
```

### (Optional) Publishing the container to a registry

After you have built a production Docker image for a Strapi project, you can publish the image to a Docker registry. Ideally for production usage this should be a private registry as your Docker image will contain sensitive information.

Depending on your hosting provider you may need to use a different command to publish your image. It is recommended you review the <ExternalLink to="https://docs.docker.com/engine/reference/commandline/push/" text="Docker documentation"/> for more information on publishing Docker images with more advanced options.

Some popular hosting providers are:

- <ExternalLink to="https://aws.amazon.com/ecr/" text="AWS ECR"/>
- <ExternalLink to="https://azure.microsoft.com/en-us/services/container-registry/" text="Azure Container Registry"/>
- <ExternalLink to="https://cloud.google.com/container-registry" text="GCP Container Registry"/>
- <ExternalLink to="https://www.digitalocean.com/products/container-registry/" text="Digital Ocean Container Registry"/>
- <ExternalLink to="https://www.ibm.com/cloud/container-registry" text="IBM Cloud Container Registry"/>
- <ExternalLink to="https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry" text="GitHub Container Registry"/>
- <ExternalLink to="https://docs.gitlab.com/ee/user/packages/container_registry/" text="Gitlab Container Registry"/>

## Community tools

Several community tools are available to assist you in deploying Strapi to various cloud providers and setting up Docker in a development or production environment.

We strongly support our community efforts and encourage you to check out the following tools, please help support them by contributing to their development.

If you would like to add your tool to this list, please open a pull request on the <ExternalLink to="https://github.com/strapi/documentation" text="Strapi documentation repository"/>.

### @strapi-community/dockerize

The `@strapi-community/dockerize` package is a CLI tool that can be used to generate a `Dockerfile` and `docker-compose.yml` file for a Strapi project.

To get started run `npx @strapi-community/dockerize@latest` within an existing Strapi project folder and follow the CLI prompts.

For more information please see the official <ExternalLink to="https://github.com/strapi-community/strapi-tool-dockerize" text="GitHub repository"/> or the <ExternalLink to="https://www.npmjs.com/package/@strapi-community/dockerize" text="npm package"/>.

### @strapi-community/deployify

The `@strapi-community/deployify` package is a CLI tool that can be used to deploy your application to various cloud providers and hosting services. Several of these also support deploying a Strapi project with a Docker container and will call on the `@strapi-community/dockerize` package to generate the required files if they don't already exist.

To get started run `npx @strapi-community/deployify@latest` within an existing Strapi project folder and follow the CLI prompts.

For more information please see the official <ExternalLink to="https://github.com/strapi-community/strapi-tool-deployify" text="GitHub repository"/> or the <ExternalLink to="https://www.npmjs.com/package/@strapi-community/deployify" text="npm package"/>.

## Docker FAQ

### Why doesn't Strapi provide official Docker images?

Strapi is a framework that can be used to build many different types of applications. As such, it is not possible to provide a single Docker image that can be used for all use cases.

### Why do we have different Dockerfiles for development and production?

The primary reason for various Docker images is due to the way our Admin panel is built. The Admin panel is built using React and is bundled into the Strapi application during the build process. This means that the Strapi backend is acting as a web server to serve the Admin panel and thus certain environment variables are statically compiled into the built Admin panel.

It is generally considered a best practice with Strapi to build different Docker images for development and production environments. This is because the development environment is not optimized for performance and is not intended to be exposed to the public internet.
