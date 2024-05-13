# Docker Command

Docker Command 정리

## Docker image

### docker pull: 도커 이미지 pull 받기

```shell
docker pull [OPTIONS] NAME[:TAG|@DIGEST]

-a, --all-tags: 이미지의 모든 태그를 다운로드합니다.
--disable-content-trust: 이미지의 content trust를 비활성화합니다.
--platform: 특정 플랫폼의 이미지를 다운로드합니다.

docker pull ubuntu
docker pull ubuntu:20.04
docker pull --platform linux/arm64 ubuntu
```

### docker images: 도커 이미지 리스트 보기

```shell
docker images
```

### docker build: 도커 이미지 만들기

```shell
docker build [OPTIONS] PATH | URL | -

-f, --file:
  - 사용할 Dockerfile의 경로를 지정. 기본적으로 현재 디렉토리에 있는 Dockerfil 사용

--build-arg: 빌드 시에 사용할 빌드 인수를 설정

-t, --tag: 생성되는 이미지에 태그를 지정(<이미지_이름>:<태그> 형식으로 지정)

--target: 멀티스테이지 빌드에서 목표 빌드 스테이지를 지정

--network: 빌드 컨텍스트를 가져오는 데 사용되는 네트워크를 지정

--compress: 빌드 컨텍스트를 전송할 때 압축 사용

--progress: 빌드 진행 상황을 표시하는 방법 지정

--no-cache: 캐시를 사용하지 않고 빌드를 진행

--pull: 빌드 전에 베이스 이미지를 항상 업데이트

docker build -t node-local:0.1 -f ./docker/Dockerfile.local .
```

### docker rmi: 도커 이미지 삭제(컨테이너 중지 상태에서)

```shell
docker rmi <image id or name>
```

## Docker container

### docker run: 컨테이너 생성 및 실행

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

-d, --detach: 컨테이너 백그라운드 실행
--name: 컨테이너 이름 지정
-p, --publish: 호스트와 컨테이너 간 포트 매핑을 설정(포트포워딩)
-v, --volume: 호스트와 컨테이너 간 볼륨 마운트
-e, --env: 환경 변수 설정
--network: 컨테이너 특정 네트워크에 연결
--rm: 컨테이너가 종료되면 자동으로 삭제
-it: 대화형 터미널 사용하여 컨테이너를 실행
--restart: 컨테이너 재시작 정책 설정
--privileged: 컨테이너에 모든 권한 부여

docker run -it --name backend-local -p 3000:3000 node-local:0.1
docker run -d --name api-local -p 3000:3000 node-local
```

### docker rm: 컨테이너 삭제

```shell
docker rm <container id or name>
```

### docker 컨테이너 실행/중지

```shell
docker start <container id or name>
docker stop <container id or name>
```

### docker 컨테이너 리스트 확인

```shell
docker ps
docker ps -a
docker ps -q
docker ps -a -q
```

### docker exec: 실행 중인 컨테이너 내부에서 추가적인 작업 또는 디버깅

```shell
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

-d, --detach: 명령을 백그라운드에서 실행
-i, --interactive: 명령의 표준 입력을 연결
-t, --tty: 의사 TTY를 할당하고, 명령을 상호작용적으로 실행할 수 있도록
--user: 명령을 실행할 사용자를 지정
--privileged: 컨테이너를 특권 모드로 실행
--workdir: 명령을 실행할 작업 디렉토리를 지정

# 컨테이너 내부에서 Bash 셸 실행하기
docker exec -it my-container bash

# 백그라운드에서 명령 실행하기
docker exec -d my-container ls -l

# 특정 사용자로 명령 실행하기
docker exec --user myuser my-container whoami

# 특정 작업 디렉토리에서 명령 실행하기
docker exec --workdir /app my-container ls -l

# 자주 사용하는 명령어
docker exec -it node-local bash
docker exec -it node-local /bin/sh
```

## Docker network

```shell
docker network ls
docker network create my-network
docker network inspect my-network
docker networkd rm my-network
```

## Docker [image/container/volume] prue

```shell
docker container prune
docker image prune
docker volume prune
docker network prune
docker system prune
docker system prune -a
```

## Dockerfile

### FROM

- 베이스 이미지 지정
- 기존에 빌드된 이미지를 기반으로 생성

### RUN

- 쉘 명령어를 실행하여 이미지에 패키지를 설치하거나 소프트웨어를 설정

### COPY

- 호스트 머신의 파일을 컨테이너 내부로 복사

### ADD

- COPY와 유사하지만 추가적인 기능을 제공
- 로컬 파일이나 URL을 복사할 수 있으며, 압축 파일을 자동으로 해제하고 원격 파일을 다운로드 가능

### CMD

- 컨테이너가 시작될 때 실행하는 명령을 설정
- 도커 파일에서 한 번만 사용해야 함
- 동시에 여러 개의 CMD 지시문을 사용할 수 있지만, 마지막 CMD만 적용됨

### ENTRYPOINT

- 컨테이너가 시작될 때 실행되는 실행 파일 설정
- CMD와 유사하지만 ENTRYPOINT는 항상 실행됨
- CMD는 ENTRYPOINT의 인수로 사용됨

### WORKDIR

- 명령이 실행될 작업 디렉토리 설정
- 해당 디렉토리가 없다면 생성됨

### ENV

- 환경 변수 설정

### EXPOSE

- 컨테이너가 노출할 포트 설정

### VOLUME

- 호스트 머신과 컨테이너 간에 볼륨을 공유할 수 있도록 설정

### USER

- 컨테이너가 실행될 사용자를 설정

### HEALTHCHECK

- 컨테이너의 상태를 확인하는 방법을 설정

```Dockerfile
FROM node:20.11-alpine

RUN apk update && apk add bash sudo vim

WORKDIR /app

COPY . .

RUN npm cache clean --force && rm -rf node_modules && npm install

EXPOSE 3000

CMD ["npm", "run", "start"]
```

## docker compose

```shell
docker-compose up [options] [SERVICE...]
docker compose up

docker compose up -d(--detach)
- 컨테이너를 백그라운드 모드로 실행

docker compose up --force-recreate
- 변경 사항이 없어도 컨테이너를 강제로 다시 생성

docker compose images

docker compose down
docker compose build
docker compose start

docker compose stop
docker compose stop -t 20 # stop with a timeout

docker compose restart

docker compose rf
docker compose rf -f

docker compose logs
docker compose logs -f # follow log output

docker compose exec [my-service] bash
docker compose ps
```
