# Learn GitHub Actions

## Overview

- 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 지속적 통합 및 지속적 배포(CI/CD) 플랫폼
- DevOps를 넘어 Repo에서 다른 이벤트가 발생할 때 워크플로를 실행할 수 있다.

- 예시:
  - Repo 대한 모든 풀 리퀘스트를 빌드하고 테스트하는 워크플로를 생성
  - 병합된 PR을 프로덕션에 배포
  - Repo에 새 이슈를 만들 때 마다 자동으로 적절한 레이블 추가하는 워크플로 실행

## The components of GitHub Actions

- Repo에서 PR이 열리거나 issue가 만들어지는 등의 event가 발생할 때 트리거되도록 Workflows 구성
- Workflows에는 순차적/병렬적으로 실행할 수 있는 하나 이상의 작업(job)이 포함
- 각 job(작업)은 자체 가상 머신 런쳐 내부 또는 컨테이너 내에서 실행
- 사용자가 정의한 스크립트 실행 또는 Workflows를 간소화 할 수 있는 재사용 가능한 확장인 job을
  실행하는 하나 이상의 단계가 있음

## Workflows(워크플로)

- 하나 이상의 작업을 실행하는 구성 가능한 자동화된 프로세스
- Repo에 체크인된 yaml 파일로 정의
- Repo의 이벤트에 의해 트리거될 때 실행되거나, 수동으로 또는 정의된 일정에 따라 트리거
- Workflows는 Repo의 .github/workflows 디렉토리에 정의되며, Repo에는 각각 다른 작업을
  수행할 수 있는 여러 워크플로가 있을 수 있음.
  - PR 빌드 -> test Workflows
  - 릴리스가 만들어질 때마다 앱을 배포하는 Workflows
  - 이슈 생성할 때 레이블 추가하는 Workflows
- 다른 Workflows 내에서 Workflows를 참조할 수 있음.

## Events(이벤트)

- Workflows 실행을 트리거하는 Repo의 특정 활동
  - PR을 만들거나 이슈를 생성했을 때, Repo에 커밋을 푸시할 때 Github에서 활동이 시작될 수 있음
  - 일정에 따라 워크플로가 실행되도록 트리거하거나 REST API에 게시하거나 수동으로 트리거 가능

[워크플로를 트리거하는데 사용할 수 있는 이벤트의 전체 목록](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

## Jobs(작업)

- 동일한 러너에서 실행되는 워크플로의 단계 집합
- 각 단계는 실행될 shell script이거나 실행될 작업
- 단계는 순서대로 실행되며, 서로 종속적
- 각 단계는 동일한 러너에서 실행되므로, 한 단계에서 다른 단계로 데이터를 공유 할 수 있음
  - 앱 빌드 -> 빌드된 앱 테스트
- 작업의 종속성을 다른 작업과 구성할 수 있음
- 기본적으로 작업은 종속성이 없으며, 서로 병렬로 실행
- 한 작업이 다른 작업에 종속된 경우, 종속된 작업이 완료될 때 까지 기다렸다가 실행 가능
- 빌드 작업은 병렬로 실행되며, 모두 성공적으로 완료되면 패키징 작업이 실행

## Actions(액션)

- 액션은 복잡하지만 자주 반복되는 작업을 수행하는 Github 액션 플랫폼용 사용자 지정 애플리케이션
- 액션을 사용하면 워크플로 파일에 작성하는 반복적인 코드의 양을 줄일 수 있음
- 액션은 Github에서 git repos를 가져오거나, 빌드 환경에 맞는 올바른 도구 체인을 설정하거나,
  클라우드 제공업체에 대한 인증을 설정할 수 있음

## Runner(러너)

- 러너는 워크플로가 트리거될 때 워크플로를 실행하는 서버
- 각 러너는 한 번에 하나의 작업을 실행할 수 있음
- Github은 워크플로를 실행할 수 있는 Ubuntu, Linux, Windows, MacOS 러너를 제공하며,
  각 워크플로 실행은 프로비저닝된 새로운 가상 머신에서 실행된다.
- Github은 더 큰 구성에서 사용할 수 있는 더 큰 러너도 제공한다.
- 다른 운영 체제가 필요하거나 특정 하드웨어 구성이 필요한 경우, 자체 러너를 호스팅할 수 있음

## Ref

- [building-and-testing-nodejs](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs)
- [Learn GitHub Actions](https://docs.github.com/en/actions/learn-github-actions)

## GitHub Action YAML Syntax

- name

  - (optional) 워크플로명
  - ex: name: leanring-github-action

- run-name

  - (optional) 워크플로에서 생성된 워크플로 실행 이름
  - ex: run-name: ${{ github.actor }} is leanring-github-action
  - ex: run-name: Deploy to ${{ inputs.deploy_target }} by @${{ github.actor }}

- on
  - 워크플로를 실행할 이벤트 정의
  - ex: on: push
  - ex: on: [push, fork]

```yml
# push 이벤트에 대해 특정 브랜치만 트리거 되도록 설정
on:
  push:
    branches:
      - main
      - 'releases/**'

# pull_request 이벤트에 대해 특정 브랜치만 트리거 되도록 설정
on:
  pull_request:
    branches:
      - main
      - 'mona/octocat'
      - 'releases/**'

# pull_request 이벤트에 대해 특정 브랜치를 무시하도록 설정
on:
  pull_request:
    branches-ignore:
      - 'mona/octocat'
      - 'releases/**-alpha'

# 그 외의 다양한 on
on:
  push:
    branches:
      - 'releases/**'
      - '!releases/**-alpha'

on:
  push:
    paths-ignore:
      - 'docs/**'

on:
  schedule:
    - cron:  '30 5,17 * * *'

on:
  schedule:
    - cron: '30 5 * * 1,3'
    - cron: '30 5 * * 2,4'

jobs:
  test_schedule:
    runs-on: ubuntu-latest
    steps:
      - name: Not on Monday or Wednesday
        if: github.event.schedule != '30 5 * * 1,3'
        run: echo "This step will be skipped on Monday and Wednesday"
      - name: Every time
        run: echo "This step will always run"
```

- jobs: 워크플로에서 실행되는 모든 작업을 함께 그룹화 한다.

```yml
# 워크플로에서 실행되는 모든 작업 그룹화
jobs:
  # check-bats-version이라는 작업 정의. 하위 키는 작업의 속성 정의
  check-bats-version:
    # 최신 버전의 Ubuntu Linux 런처에서 실행되도록 작업 구성.
    # 즉, GitHub에서 호스팅하는 새 가상 머신에서 작업 실행됨.
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install -g bats
      - run: bats -v
```

### Note

- 1,000개 이상의 커밋을 푸시하거나 시간 초과로 인해 GitHub가 Diff를 생성하지 않는 경우,
  워크플로는 항상 실행.
- 필터는 변경된 파일을 평가하고 path ignore 또는 list에 대해 실행하여 워크플로 실행 여부 결정.
  변경된 파일이 잆다면 워크플로가 실행되지 않음.
- Diff는 300개 파일로 제한됨.
