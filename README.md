# Getting started

This repository is a sample application for users following the getting started guide at https://docs.docker.com/get-started/.

The application is based on the application from the getting started tutorial at https://github.com/docker/getting-started

# 주요 명령어

```bash
# 이미지 빌드
docker build -t <IMAGE_TAG_NAME>

# 이미지로 컨테이너 실행 (데몬)
docker run -dp <HOST>:<CONTAINER> <IMAGE_TAG_NAME>

# 이미지로 컨테이너 실행 후 컨테이너 내부 쉘로 접근
docker run -it

# 실행중인 컨테이너 목록
docker ps
docker container list

# 이미지 목록
docker images
docker image list

# 컨테이너 정지
docker stop <CONATINER_ID>

# 컨테이너 삭제
docker rm <CONTAINER_ID>

# 정지 및 삭제
docker rm -f <CONATINER_ID>

# 이미지 삭제
docker rmi <IMAGE_TAG_NAME>

# 이미지에 태그명 부여
docker tag <TAG_NAME> <USER_NAME>/<IMAGE>

# 컨테이너 로그 보기
docker logs -f <CONTAINER_ID>

# 컨테이너 내부에 쉘로 접근
docker exec -it <CONTAINER_ID> sh
```

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

# 도커 컨테이너와 호스트 머신간 파일시스템 연결

마운트 기능을 사용하면 도커 컨테이너 내부의 파일시스템과 호스트 머신의 파일시스템을 연결할 수 있다.

### 마운트 종류

-   Named volume mount
-   Bind mount

|                                    | Named volumes | Bind mounts     |
| ---------------------------------- | ------------- | --------------- |
| Host 위치                          | 도커가 선택함 | 사용자가 결정함 |
| 컨테이너 컨텐츠를 새 볼륨으로 채움 | YES           | NO              |
| 볼륨 드라이버 지원                 | YES           | NO              |

### --mount 옵션으로 사용 시

-   Named volume mount: `type=volume,src=my-volume,target=/usr/local/data`
-   Bind mount: `type=bind,src=/path/to/data,target=/usr/local/data`

예시)

```bash
# volume mount
docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-stared

# bind mount
docker run -dp 127.0.0.1:3000:3000 --mount type=bind,src="$(pwd)",target=/src getting-started
```

## 볼륨 상세히 보기

```bash
docker volume inspect <VOLUME_NAME>
```
