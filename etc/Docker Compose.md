# Docker Compose

## Docker Compose란?

**다중 컨테이너 애플리케이션을 정의 공유** 할 수 있도록 개발된 도구로 **단일 명령을 사용하여 모두 실행 또는 종료** 할 수 있도록 개발된 도구

#### 다중 컨테이너 애플리케이션 정의

서비스를 서버 위에 실행시킬 때 가끔 여러개의 서비스들이 필요한 경우가 존재합니다.

예를 들어 새로 만든 application이 사용하는 DB는 mysql이라면, mysql을 설치/실행 후 application을 실행 해야 합니다.

이때 `docker-compose`는 여러 개의 서비스를 한 번에 정의가 가능합니다.

```yaml
services:
  app:
    image: test_app_v2

  mysq:
    image: mysql
```

이런 식으로 하나의 문서에 여러 개의 컨테이너를 정의 할 수 있습니다.

#### 단일 명령을 사용하여 모두 실행 또는 종료

* `docker-compose up` : 명령어 하나로 문서에 정의한 서비스들을 한꺼번에 실행
* `docker-compose down` : 명령어 하나로 정의한 서비스들 한꺼번에 종료

## Docker Compose 특징

#### 단일 호스트의 여러 격리된 환경

Compose는 프로젝트 이름을 사용하여 환경을 서로 격리하고 여러 다른 콘텍스트에서 이 프로젝트 이름을 사용하여 접근 합니다.

예를 들어 `application` 이라는 서비스를 실행 시킬 때 `first_app`, `second_app` 같은 방식으로 여러 개를 서로 격리하여 서비스가 가능합니다.

프로젝트 이름은 기본으로 실행한 폴더명이 기준이 되며 별도로 지정이 가능합니다.

```shell
$ docker-compose -p first up
$ docker-compose -p second up
```

위와 같이 `-p` 옵션으로 프로젝트명을 주어 실행이 가능합니다.

#### 컨테이너 생성시 볼륨 데이터 보존

컨테이너 생성시 볼륨 데이터를 보존하여 데이터가 휘발되지 않도록 처리해줍니다.

컨테이너 내부에서 생성하여 사용하는 파일 볼륨을 로컬 경로와 공유하여 실수로 컨테이너가 종료되더라도 볼륨을 유지해주어 재실행시 이전에 사용했던 환경 그대로 사용이 가능한 장점이 있습니다.

#### 변경된 컨테이너만 재생성

컨테이너를 만드는데 사용되는 구성을 캐시하여 변경되지 않은 서비스를 다시 시작하면 Compose는 기존 컨테이너를 다시 사용합니다.

#### 변수 및 환경간 구성 이동

Compose 파일의 변수를 지원하여 다양한 환경 또는 다른 사용자에 맞게 컴포지션 커스텀이 가능합니다.

## 서비스 정의

Dockerfile을 Compose로 변환하기

상단의 Docker 명령어를 아래 Compose로 작성이 가능합니다.

#### App Service 정의

* Docker 명령어

```sh
$ docker run -dp 3000:3000 \
  -w /app -v ${PWD}:/app \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:12-alpine \
  sh -c "yarn install && yarn run dev"
```

* Compose

```yaml
version: "3.8"

services:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
```

#### Mysql Service 정의

* Docker 명령어

```sh
$ docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:5.7
```

* Compose

```yaml
version: "3.8"

services:
  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment: 
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

#### 최종본

두 Compose를 합쳐서 최종 Compose를 만듭니다.

```yaml
version: "3.8"

services:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment: 
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

이런식으로 두개의 설정을 한꺼번에 정의하여 실행환경을 빠르게 구성할 수 있습니다.

하나의 설정 파일로 관리가 되어 서비스를 빠르게 올리고 내릴 수 있습니다.

## Kubernetes와 차이

- Docker Compose는 **오케스트레이션없이 다중 컨테이너** Docker 응용 프로그램을 정의하고 실행하기위한 도구입니다.
- Kubernetes에는 pod이란 개념이 있는데 그것은 여러 컨테이너들을 가지고 있고, 사용 가능한 노드 사이의 분배 개념(클러스트링) 해준다.
  - Kubernetes 기능
    - 상태 관리 : 상태를 선언하고 선언한 상태를 유지 / 노드가 죽거나 컨테이너 응답이 없으면 자동 복구
    - 스케줄링 : 클러스터의 여러 노드 중 조건에 맞는 노드를 찾아 컨테이너 배치
    - 클러스터 : 가상 네트워크를 통해 하나의 서버에 있는 것처럼 통신
    - 서비스 디스커버리 : 서로 다른 서비스를 쉽게 찾고 통신 가능
    - 리소스 모니터링 : cAdvisor를 통한 리소스 모니터링
    - 스케일링 : 리소스에 따라 자동으로 서비스 조정
    - RollOut/RollBack : 배포/롤백 및 버전 관리

<br>

<br>

<br>

<br>

reference : https://meetup.toast.com/posts/277/