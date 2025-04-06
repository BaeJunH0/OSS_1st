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

Actions 스크립트에는 총 6가지의 핵심 구성 요소가 존재한다.
- **Workflow** : 하나 이상의 Job으로 구성된 자동화 프로세스. `.github/workflows` 디렉토리에 `.yml` 파일로 정의한다.
- **Event** : 워크플로 실행을 트리거하는 리포지토리의 특정 행동을 의미한다.
- **Job** : 여러 Step으로 구성되며, 각각 독립적인 가상 환경(또는 컨테이너)에서 실행된다.
- **Step** : 명령어 실행 또는 Action 호출 단위.
- **Action** : 재사용 가능한 코드 조각. 공식 마켓플레이스 또는 커스텀으로 작성하여 사용할 수 있다.
- **Runner** : Job이 실제 실행되는 환경. GitHub에서 제공하는 Hosted Runner를 쓰거나, Self-hosted Runner를 직접 설치할 수도 있다. 
워크플로우는 기본적으로 `workflow → jobs → steps` 구조로 구성된다. 
하나의 워크플로우는 여러 개의 작업(job)을 포함할 수 있고, 각 job은 독립적인 실행 단위로서 서로 다른 러너에서 병렬로 수행될 수 있다. 
각 job 내부에는 step이 있으며, 이는 실제로 실행되는 명령어나 액션들로 구성된다. 이러한 구조를 통해 복잡한 파이프라인도 논리적으로 구성할 수 있다. 

Actions에서는 반복적인 워크플로우 정의를 줄이기 위해 워크플로우를 재사용할 수 있다. 
중복되는 빌드/배포 로직을 별도의 `.yml` 파일로 분리하여, 여러 리포지토리나 워크플로우에서 호출할 수 있도록 해준다.  
이를 통해 유지보수성이 향상되고, 대규모 프로젝트나 조직 내 일관된 CI/CD 정책을 사용할 수 있다. 

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

**Actions의 장점** 

GitHub Actions의 가장 큰 장점 중 하나는 GitHub와의 깊은 통합이다. GitHub의 `push`, `pull request`, `issues`, `release` 같은 이벤트를 워크플로우 트리거로 사용할 수 있어, 개발 흐름 안에 자연스럽게 자동화를 녹여낼 수 있다. 
또한 GitHub는 Actions 마켓플레이스를 통해 수천 개의 오픈소스 액션을 제공하고 있어, 복잡한 설정 없이도 다양한 기능을 바로 활용할 수 있다.

실행 환경은 기본적으로 제공되는 호스팅 러너(Github-Hosted Runner) 외에도, 사용자가 온-프레미스 환경에서 직접 관리할수 있는 Self-hosted Runner 를 설정할 수 있다. 
Self-hosted Runner는 특정 하드웨어나 운영체제, 네트워크 조건 등 사용자 맞춤 환경에서 동작할 수 있기 때문에, 고사양 빌드 머신, 내부 시스템과 연동이 필요한 워크플로우 등에 특히 유용하다. 
또한 반복적으로 실행되는 워크플로우에 대해 실행 시간을 단축할 수 있으며, 기업에서는 자체 방화벽 안에서 모든 자동화 작업을 처리할 수 있다는 점에서 보안성도 크게 향상된다. 

GitHub Actions는 Docker 컨테이너 또는 커스텀 스크립트를 실행할 수 있는 유연성을 가지고 있어 복잡한 빌드 및 배포 파이프라인도 쉽게 설정 가능하다. 
보안 측면에서도 GitHub Secrets를 통해 민감한 정보(API 키, 토큰 등)를 안전하게 관리할 수 있으며, 환경 변수와 조합하여 다양한 설정을 할 수 있다. 
또한 워크플로우 내에서 job을 병렬 또는 순차적으로 실행할 수 있고, 조건문(`if`)과 매트릭스 전략(matrix strategy)을 이용하면 다양한 환경에 대한 테스트를 동시에 실행할 수 있어 테스트 커버리지 향상에도 기여한다. 

GitHub Actions는 GitOps 방식에도 널리 활용된다. GitOps는 애플리케이션의 배포 및 운영 환경 설정을 Git 리포지토리로부터 선언적으로 관리하는 접근 방식으로, 
GitHub Actions를 사용하면 코드 변경 사항이 Git에 반영됨과 동시에 자동으로 클러스터에 배포되도록 설정할 수 있다. 
이로써 인프라 변경 사항 역시 코드 리뷰 및 변경 이력의 추적이 가능해지게 된다. 

**Best Practice**
- 워크플로우는 작고 명확하게 구성하도록 한다. Job 단위로 분리해 병렬 처리 효율을 높이는 것이 좋다.
- 민감한 secret key나 api 토큰 등의 값은 직접 코드에 하드코딩하여 노출시키지 않고, `secrets`에 등록하여 사용한다.
- 워크플로우 파일 변경 시에는 별도의 브랜치에서 테스트하고 main에 `merge`하는 것이 안전하다.
- 주기적인 캐시 설정(`actions/cache`)을 통해 빌드 속도를 개선할 수 있다.


### 4. Copilot
