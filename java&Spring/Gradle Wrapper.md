# Gradle Wrapper

Gradle 빌드에 권장되는 사용 방법은 `Gradle Wrapper`를 사용하는 것이다. 

`Wrapper`는 미리 선언된 버전의 Gradle을 호출하고, 필요한 경우 미리 다운로드한다. 

Java 프로젝트를 CI 환경에서 빌드할 때 CI 환경을 프로젝트 빌드 환경과 매번 맞춰줄 필요가 없는 이유가 바로 Gradle Wrapper를 사용하기 때문이다. 

즉, 환경에 종속되지 않고 프로젝트를 빌드할 수 있는데 이런 점이 Gradle이 가진 강력한 특징중 하나이다.

gradle wrapper 명령어를 실행하면 아래처럼 파일들이 생성된다.

```
$ gradle wrapper

BUILD SUCCESSFUL in 545ms
1 actionable task: 1 executed

$ tree
.
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
└── gradlew.bat

2 directories, 4 files
```

<br>

### Gradle Wrapper 구조

#### gradlew.bat

윈도우용 wrapper 실행 스크립트이다.

#### gradlew

유닉스용 wrapper 실행 스크립트이다. 컴파일, 빌드 등을 하는 경우 사용한다. `./gradlew {task}` 형태로 실행한다.

#### gradle/wrapper/gradle-wrapper.jar

Wrapper 파일이다. 실행 스크립트가 동작하면 Wrapper에 맞는 환경을 로컬 캐시에 다운로드 받은 뒤에 실제 명령에 해당하는 task를 실행한다.

#### gradle/wrapper/gradle-wrapper.properties

Gradle Wrapper 설정파일이다.

<br>

<br>

<br>

출처: https://jungseob86.tistory.com/21 [Random Access Memories]

