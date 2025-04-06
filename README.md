# Github 조사 리포트
### 팀원
- 컴퓨터학부/2022111805/배준호/BaeJunH0
- 컴퓨터학부/2014097035/박성원/tt0410tt
- 컴퓨터학부/2023013074/배규민/GyuminBae
- 컴퓨터학부/2021112698/박도윤/bagdodo8
- 전자공학부/2021110253/차민성/miinscar

## 개요

## 라이선스

## 주요 기능
### 1. VCS
### 2. Issue Management
### 3. Actions
GitHub Actions는 GitHub에서 제공하는 CI/CD(지속적 통합 및 지속적 배포) 도구로, 리포지토리의 이벤트를 트리거로 하여 자동화된 워크플로우를 실행할 수 있게 해준다.
코드가 특정 브랜치에 push 되었을 때 빌드 및 배포를 자동화하거나, PR이 생성되었을 때 테스트를 실행하고 리포트를 코멘트로 올리는 등의 작업을 할 수 있다. 

Actions 스크립트에는 총 5가지의 핵심 구성 요소가 존재한다.
- **Workflow**: 하나 이상의 Job으로 구성된 자동화 프로세스. `.github/workflows` 디렉토리에 `.yml` 파일로 정의한다.
- **Job**: 여러 Step으로 구성되며, 각각 독립적인 가상 환경(또는 컨테이너)에서 실행된다.
- **Step**: 명령어 실행 또는 Action 호출 단위.
- **Action**: 재사용 가능한 코드 조각. 공식 마켓플레이스 또는 커스텀으로 작성하여 사용할 수 있다.
- **Runner**: Job이 실제 실행되는 환경. GitHub에서 제공하는 Hosted Runner를 쓰거나, Self-hosted Runner를 직접 설치할 수도 있다.

**Actions CI/CD 스크립트 작성 예시**
```yaml
name: CI Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v3

    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Install Dependencies
      run: npm install

    - name: Run Tests
      run: npm test
```

이 예시는 `main` 브랜치에 코드가 push 되거나 PR이 생성되었을 때 `build`라는 Job이 실행되며, 프로젝트의 소스코드를 checkout하고 Node.js와 npm을 셋업한 뒤, 테스트를 실행한다.

### 4. Copilot
