# [Maven]메이븐 속성(properties, pom/project, settings)

### 속성

메이븐은 설정 파일에서 발생하는 중복 설정을 제거하기 위해 속성 `(<properties/> 엘리먼트)`을  정의하고 설정 파일 전체에서 사용할 수 있다.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- 프로젝트 정보 -->
    <groupId>com.example</groupId>
    <artifactId>my-project</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!-- 프로젝트 속성 (properties) -->
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
    </properties>

    <!-- 빌드 설정 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <!-- ${}를 사용하여 속성 참조 -->
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### pom/project 속성

* `${project.build.directory}` : 'target'디렉토리. 이 속성은 `${pom.build.directory}`와 같습니다.

- `${project.groupId}`: 프로젝트의 그룹 식별자를 나타냅니다.
- `${project.artifactId}`: 프로젝트의 아티팩트 식별자를 나타냅니다.
- `${project.version}`: 프로젝트의 버전을 나타냅니다.
- `${project.name}`: 프로젝트의 이름을 나타냅니다.
- `${project.description}`: 프로젝트에 대한 설명을 나타냅니다.
- `${project.url}`: 프로젝트의 웹 페이지 또는 관련 문서의 URL을 나타냅니다.
- `${project.build.finalName}`: 프로젝트 빌드 결과물의 최종 이름을 나타냅니다. 일반적으로 `${project.artifactId}-${project.version}`로 설정됩니다.
- `${basedir}`: 프로젝트의 기본 디렉토리 경로를 나타냅니다. 기본적으로 프로젝트의 `pom.xml` 파일이 있는 디렉토리를 가리킵니다.
- `${user.home}`: 사용자의 홈 디렉토리 경로를 나타냅니다.
- `${maven.home}`: Maven 설치 디렉토리의 경로를 나타냅니다.
- `${maven.build.timestamp}`: 빌드가 수행된 타임스탬프를 나타냅니다.

### settings 속성

메이븐 관련 속성은 settings.xml  파일에서 한다. settings.xml에 설정한 값은 settings 속성으로 참조할 수 있다. settings 속성은 settings 접두사를 사용한다.

* `${settings.localRepository}` : 로컬 저장소 경로

### 환경 변수 속성

메이븐은 env 접두사를 사용하여 시스템 환경 변수 값을 참조할 수 있다.

* `${env.PATH}` : 시스템의 PATH설정 값을 참조
* `${env.JAVA_HOME}` : 시스템의 JAVA_HOME 설정값 참조

### 자바 시스템 속성

메이븐은 자바 시스템 속성으로 설정된 모든 속성을 참조할 수 있다.

- `${java.home}`: Java 홈 디렉토리의 경로를 나타냅니다.
- `${java.version}`: 현재 사용 중인 Java 버전을 나타냅니다.
- `${os.name}`: 현재 운영 체제의 이름을 나타냅니다.
- `${os.arch}`: 현재 운영 체제의 아키텍처를 나타냅니다.
- `${os.version}`: 현재 운영 체제의 버전을 나타냅니다.