# Getting started

This repository is a sample application for users following the getting started guide at https://docs.docker.com/get-started/.

The application is based on the application from the getting started tutorial at https://github.com/docker/getting-started

# 어플리케이션 도커 이미지 빌드

빌드를 위해 Dockerfile이 필요함.

```text
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

-   `FROM node:18-alpine`: node:18-alpine 이미지를 베이스로하는 이미지를 빌드함. 빌드 시 해당 이미지가 없으면 도커가 해당 이미지를 먼저 다운로드 받음.
-   `WORKDIR`: 컨테이너 안에 작업 디렉토리를 생성함.
-   `COPY . .`: 빌드 컨텍스트의 파일들을 컨테이너 내부의 작업 디렉토리로 복사함.
-   그 후 `RUN yarn install` 명령어에 따라 어플리케이션을 설치함.
-   `CMD` 명령어는 이 이미지로 실행될 컨테이너에서 시작할 기본 명령어.
-   `-t` : —tag 옵션은 이 도커 이미지의 이름을 지정한다.

## 도커 이미지 빌드

```bash
docker build -t <IMAGE_TAG_NAME>
docker build -t getting-started
```

-   마지막에 `.` 명령어는 도커가 해당 경로에서 Dockerfile을 찾아서 사용하라고 알려주는 path 이다.

## 도커 이미지로 컨테이너 실행

```bash
docker run -dp <HOST>:<CONTAINER> <IMAGE_TAG_NAME>
docker run -dp 127.0.0.0:3000:3000 getting-started
```

-   `-d` : —detach 명령어로써, 도커 컨테이너를 백그라운드에서 실행시킨다.
    -   실행된 컨테이너는 Docker Desktop > Containers > 대시보드에서 확인할 수 있다.
    -   또는 `docker ps` 명령어로 확인함 (= `docker container list`)
-   `-p`: —publish 명령어로써, 호스트와 컨테이너 간에 포트매핑 옵션이다.
    -   `HOST:CONTAINER` 형식의 문자로 지정한다.
    -   HOST는 호스트의 주소이고 CONTAINER는 컨테이너의 포트이다.
    -   127.0.0.1:3000 으로 접속하면 컨테이너 3000 포트로 연결할 수 있다.
    -   포트매핑을 하지 않으면 컨테이너로 실행한 어플리케이션에 접근할 수 없다.

# 어플리케이션 업데이트

## 소스코드 변경 후 도커 이미지 재빌드

```
docker build -t getting-started
```

## 기존 컨테이너 정지 및 삭제

```
docker stop getting-started
docker rm getting-started
```

## 새 컨테이너 실행

```
docker run -dp 127.0.0.1:3000:3000 getting-started
```
