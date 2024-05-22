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



## gitlab-ci.yml

GitLab CI/CD를 사용하려면 `gitlab-ci.yml` 설정 파일 작성이 필요합니다.

이 파일은 CI/CD 파이프라인 중에 실행될 단계, 작업 및 스크립트를 지정합니다. 

자체 사용자 정의 구문이 포함된 YAML 파일입니다.

이 파일에서 변수, 작업간의 종속성을 정의하고 각 작업을 실행해야하는 시기와 방법을 지정합니다.

파일의 이름은 원하는 대로 지정할 수 있지만, ` gitlab-ci.yml` 이 가장 일반적으로 사용되는 이름 입니다.


