# GitLab CI/CD

## GitLab CI/CD WorkFlow

![](.\images\gitlab_ci_cd_workflow.png)

1. Issues에서 코드 구현에 대해 논의하고 제안된 변경사항을 로컬에서 작업합니다.
2. GitLab 원격 저장소의 Feature 브랜치에 커밋/푸시 합니다.
3. 푸시는 프로젝트의 CI/CD 파이프라인을 트리거 합니다.
4. 자동화된 스크립트(순차 or 병렬)를 실행하여 다음을 수행합니다.
   1. 애플리케이션을 빌드하고 테스트 합니다.
   2. 로컬 호스트에서 보는 것과 동일하게 리뷰 앱에서 변경사항을 미리 봅니다.
5. 구현이 예상대로 작동하면 다음을 수행합니다.
   1. 코드를 검토하고 승인합니다.
   2. Feature 브랜치를 기본 브랜치에 병합합니다.
      1. GitLab CI/CD는 변경사항을 프로덕션 환경에 자동 배포합니다.
6. 문제가 발생하면 변경사항을 롤백할 수 있습니다.

## GitLab CI/CD 용어 및 개념

#### Pipelines

지속적 통합, 전달 및 배포의 최상위 구성요소 입니다.

파이프라인은 다음으로 구성됩니다.

* 수행할 작업을 정의하는 Jobs
  
  * ex) 코드를 컴파일하거나 테스트하는 Job

* 작업을 실행할 시기를 정의하는 stages
  
  * ex) 코드를 컴파일 후 테스트를 실행하는 단

Job은 러너에 의해 실행됩니다.

한 단계의 모든 Job이 성공하면, 파이프라인은 다음 단계로 넘어갑니다.

한 단계의 어떤 Job이 실패하면, 다음 단계는 (일반적으로) 실행되지 않고 파이프라인이 일찍 종료 됩니다.

일반적으로 파이프라인은 자동으로 실행되며 생성된 후에는 개입이 필요하지 않습니다.

#### Jobs

`gitlab-ci.yml` 파일의 가장 기본적인 요소 입니다.

Job은 Runner가 실행해야 하는 명령 모음입니다.

Job의 결과물(Output)을 실시간으로 볼 수 있습니다.

* Job 특징
  
  * 어떤 조건에서 실행되어야 하는지를 명시하는 제약 조건으로 정의
  
  * 임의의 이름을 가진 최상위 요소이며, 최소한 cript절을 포함해야 함
  
  * 정의할 수 있는 수에는 제한이 없음

```yml
# 서로 다른 명령을 실행하는 두 개의 개별 Job이 있는 CI/CD 구성
job1:
  script: "execute-script-for-job1"
job2:
  script: "execute-script-for-job2"
```

#### Variables

환경변수의 유형

* Jobs 및 파이프라인의 동작을 제어

* 재사용하려는 값을 저장

* `gitlab-ci.yml` 파일에 값 하드 코딩 방지

미리 정의된 변수 사용이 가능합니다.

[Predefined CI/CD variables reference | GitLab](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)

```yml
# 사전 정의된 변수 CI_JOB_STAGE를 사용하여 Job의 단계를 출력
test_variable:
  stage: test
  script:
    - echo "$CI_JOB_STAGE"
# test_variable Job의 stage인 test를 출력 
```

#### Environments

코드가 배포되는 위치를 설명합니다.

환경에 코드 버전을 배포할 때마다 배포(Deployment)가 생성됩니다.

* 각 환경에 대한 전체 배포 이력을 제공합니다.

* 배포를 추적하므로 서버에 배포된 항목을 항상 알 수 있습니다.

프로젝트와 연결된 K8s와 같은 배포 서비스가 있는 경우, 이를 사용하여 배포를 지원할 수 있습니다.

#### Runners

러너는 `gitlab-ci.yml`에 정의된 코드를 실행합니다.

러너는 GitLab CI/CD의 코디네이터 API를 통해 CI Job을 선택하고, Job을 실행한 다음, 결과를 GitLab 인스턴스로 다시 보내는 경량의 확장성이 뛰어난 에이전트입니다.

러너는 관리자가 생성하며 GitLab UI에 표시됩니다. 러너는 특정 프로젝트에 한정되거나 모든 프로젝트에서 사용할 수 있습니다.

* 러너 유형
  
  * 공유 러너(Shared runners) : GitLab 인스턴스의 모든 그룹 및 프로젝트에서 사용
  
  * 그룹 러너(Group runners) : 그룹의 모든 프로젝트와 하위 그룹에서 사용
  
  * 특정 로너(Specific runners) : 특정 프로젝트와 연결. 일반적으로 특정 러너는 한 번에 하나의 프로젝트에 사용.

#### Artifacts

단계(Stages) 간에 전달되는 단계 결과에 사용합니다.

아티팩트는 저장 및 업로드 할 수 있도록 Job에 의해 생성되는 파일입니다.

동일한 파이프라인의 이후 단계의 Job에서 아티팩트를 가져와 사용할 수 있습니다. 한 단계의 Job에서 아티팩트를 생성하여 동일한 단계의 다른 Job에서 이 아티팩트를 사용할 수 없습니다. 이 디에터는 다른 파이프라인에서 사용할 수 없지만 UI에서 다운로드할 수 있습니다.

애플리케이션을 빌드하는 동안 모듈을 다운로드하는 경우, 이를 아티팩트로 선언하고, 후속 단계의 Job에서 이를 사용할 수 있습니다.

#### Cache

프로젝트 의존성(Dependencies)을 저장하는데 사용합니다.

캐시는 후속 파이프라인에서 지정된 Job의 속도를 높일 수 있습니다.

인터넷에서 다시 가져올 필요가 없도록 다운로드한 의존성을 저장할 수 있습니다.

단계 간에 중간 빌드 결과를 전달하도록 캐시를 구성할 수 있지만 대신 아티팩트를 사용해야 합니다.

## gitlab-ci.yml

GitLab CI/CD를 사용하려면 `gitlab-ci.yml` 설정 파일 작성이 필요합니다.

이 파일은 CI/CD 파이프라인 중에 실행될 단계, 작업 및 스크립트를 지정합니다. 

자체 사용자 정의 구문이 포함된 YAML 파일입니다.

이 파일에서 변수, 작업간의 종속성을 정의하고 각 작업을 실행해야하는 시기와 방법을 지정합니다.

파일의 이름은 원하는 대로 지정할 수 있지만, ` gitlab-ci.yml` 이 가장 일반적으로 사용되는 이름 입니다.

```yml
# gitlab-ci.yml 예
stages:
  - build
  - test

image: alpine:latest

variables:
  GREETING_MESSAGE: Hello

build_a:
  stage: build
  script:
    - echo "$GREETING_MESSAGE, GitLab CI!"
    - echo "이 Job은 무언가를 빌드합니다."

build_b:
  stage: build
  script:
    - echo "이 Job은 다른 무언가를 빌드합니다."
    - sleep 5

test_a:
  stage: test
  script:
    - echo "이 Job은 무언가를 테스트합니다."
    - echo "빌드 단계의 모든 Job이 완료된 경우에만 실행됩니다."

test_b:
  stage: test
  script:
    - echo "이 Job은 다른 무언가를 테스트합니다."
    - echo "이 Job도 빌드 단계의 모든 Job이 완료된 경우에만 실행됩니다."
    - echo "test_a와 거의 동시에 시작됩니다."
    - sleep 5
```

##### maven을 사용하는 SpringBoot 프로젝트 사용 예

```yml
stages:
  - build
  - test

maven-build:
  image: maven:3.6.3-openjdk-11
  stage: build
  script: "./mvnw clean compile"

maven-test:
  image: maven:3.6.3-openjdk-11
  stage: test
  script: "./mvnw test"
```

##### GitLab CI 속도 향상

GitLab CI 파이프라인에서 Cache를 사용하면 속도를 향상시킬 수 있습니다.

```yml
variables:
  MAVEN_OPTS: "-Dhttps.protocols=TLSv1.2 -Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true"
  MAVEN_CLI_OPTS: "--batch-mode --errors --fail-at-end --show-version -DinstallAtEnd=true -DdeployAtEnd=true"

image: maven:3.6.3-openjdk-11

cache:
  paths:
    - .m2/repository

stages:
  - build
  - test

maven-build:
  stage: build
  script: "./mvnw $MAVEN_CLI_OPTS clean compile"

maven-test:
  image: maven:3.6.3-openjdk-11
  script: "./mvnw $MAVEN_CLI_OPTS test"
```
