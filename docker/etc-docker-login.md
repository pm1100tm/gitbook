# Docker login

Docker Login 을 사용해보자.

```shell
docker login -u [username]

# 위의 명령어 실행 후 password 를 올바르게 입력하면, 아래와 같은 메세지를 볼 수 있다.

Password:
Login Succeeded

Logging in with your password grants your terminal complete access to your account.
For better security, log in with a limited-privilege personal access token. Learn more at https://docs.docker.com/go/access-tokens/

# 로그인한 패스워드 정보는 ~/.docker/config.json에 저장된다.
```

## Docker Hub에서 2단계 인증을 활성화한 경우

- 패스워드로 로그인을 할 수 없음
- 이 경우 Personal Access Token을 발급해야 함
- Docker Hub에서 2단계 인증을 활성화하는 방법과 토큰을 발급하는 방법에 대해서는 다음 글에서 소개합니다.

## Ref

- [초심자를 위한 Docker 입문](https://www.lainyzine.com/ko/article/summary-of-how-to-use-docker/)
