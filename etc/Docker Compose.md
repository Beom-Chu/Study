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

<br>

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

<br>

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

<br>

## 명령어

### up

`docker-compose.yml` 파일의 내용에 따라 이미지를 빌드하고 서비스를 실행합니다.

```sh
$ docker-compose -p first up -d
```

`up` 명령어로 compose 실행 시 단계별 진행 사항

1. 서비스를 띄울 네트워크 설정
2. 필요한 볼륨 생성(혹은 이미 존재하는 볼륨과 연결)
3. 필요한 이미지 pull
4. 필요한 이미지 build
5. 서비스 의존성에 따라 서비스 실행

##### options

* `-d` : 서비스 백그라운드 실행.
* `--force-recreate` : 컨테이너를 지우고 새로 생성.
* `--build` : 서비스 시작 전 이미지를 새로 생성.
* `-f` : 기본으로 제공하는 `docker-compose.yml`이 아닌 다른 파일명을 실행할 때 사용.
* `docker-compose -f docker-compose.yml -f docker-compose-test.yml up` 형태로 두 개의 파일 실행도 가능.
* Yaml을 두 개 이상 설정할 경우 뒤에 있는 파일이 우선.

###  down

실행 중인 서비스를 삭제합니다.

컨테이너와 네트워크를 삭제하며, 옵션에 따라 볼륨도 같이 삭제할 수 있습니다.

```sh
$ docker-compose down
```

##### options

* `-v` , `--volume` : 볼륨까지 같이 삭제.
  * DB 데이터 초기화하는데 용이함
  * 모든 설정을 초기화하고 새로 시작할 때 사용

### stop, start

서비스를 멈추거나, 멈춘 서비스를 시작합니다.

```sh
$ docker-compose stop
$ docker-compose start
```

### ps

현재 환경에서 실행 중인 서비스의 상태를 표시합니다.

```sh
$ docker-compose ps
```

### exec

실행 중인 컨테이너에서 명령어를 실행합니다.

명령어는 아래와 같은 패턴으로 실행됩니다.

> docker-compose exec {service_name} {실행될 명령어}

```sh
$ docker-compose exec app ./upload_logs.sh
$ docker-compose exec mysql mysql -uroot -psecret todos
```

### run

특정 명령어를 일회성으로 실행합니다.

`run` 은 `exec`와 비슷한 역할을 하지만 다른 점은 컨테이너의 재실행 여부입니다.

`run`은 실행시 새로운 컨테이너를 띄우는 반면, `exec`는 실행되어 있는 컨테이너에 접속합니다.

`exec`는 프로세서를 실행시켜 놓을 때 사용되고, `run`은 batch성 작업에 특화 되어있습니다.

##### options

* `--rm` : 해당 명령어가 종료된 뒤 컨테이너도 종료

### logs

output으로 나온 log들을 확인할 때 사용합니다.

```sh
$ docker-compose logs -f
```

##### options

* `-f`, `--follow` : 실시간 로그 출력

### config

docker-compose에 최종적으로 적용된 설정을 보여줍니다.

```sh
$ docker-compose config
```



<br>

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