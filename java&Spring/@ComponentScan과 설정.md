# @ComponentScan과 설정

* @ComponentScan 역할

​	`@Component` 및 StreoType(`@Service`,`@Repository`,`@Controller`,`@Configuration`) 어노테이션이 부여된 Class들을 자동으로 스캔하여 Bean으로 등록.

<br>

### 기본 사용

```java
@Configuration
@ComponentScan("com.package1.component")
class AppContextConfig{
    ...
}
```

```java
@Configuration
@ComponentScan(basePackages = {"com.package1.component"})
class AppContextConfig{
    ...
}
```

설정한 경로 패키지의 하위 패키지를 모두 스캔.

### 권장 방법

설정 정보 클래스의 위치를 프로젝트 최상단에 위치 시키고 `@ComponentScan` 어노테이션만 붙이고 패키지 경로 지정은 생략.

<br>

### includeFilters / excludeFilters

특정 조건을 만족하는 클래스만 스캔하거나 제외 가능.

| FilterType      | 설명                            |
| --------------- | ------------------------------- |
| ANNOTATION      | 어노테이션을 기준으로 객체 필터 |
| ASPECTJ         | AspectJ 패턴을 사용             |
| ASSIGNABLE_TYPE | 클래스를 기준으로 필터          |
| REGEX           | 정규식을 기준으로 필터          |
| CUSTOM          | 개발자가 직접 만든 필터를 적용  |

##### ANNOTATION

```java
@Configuration
@ComponentScan(
    basePackages = "com"
    includeFilters = {
        @Filter(
            type = FilterType.ANNOTATION, 
            classes = {Component.class, Repository.class, Service.class, Controller.class}
        )
    }
)
public class AppContextConfig {
    ...
} 
```

##### ASPECTJ

```java
@Configuration
@ComponentScan(
    basePackages = "com", 
    includeFilters = {
        @Filter(
            type = FilterType.ASPECTJ, 
            pattern = {"com.study.spring.*"}
        )
    }
)
public class AppContextConfig {
    ...
}
```

##### ASSIGNABLE_TYPE

```java
@Configuration
@ComponentScan(
    basePackages = "com", 
    includeFilters = {
        @Filter(
            type = FilterType.ASSIGNABLE_TYPE, 
            classes = {Component1.class}
        )
    }
)
public class AppContextConfig {
    ...
}
```

##### REGEX

```java
@Configuration
@ComponentScan(
    basePackages = "com", 
    includeFilters = {
        @Filter(
            type = FilterType.REGEX, 
            pattern = {".*component.*"}
        )
    }
)
public class AppContextConfig {
    ...
}
```

##### CUSTOM

```java
public class CustomFilter implements TypeFilter {
    @Override
    public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory){
      ...
    }
}
```

<br>

### lazyInit

기본적으로 `ComponentScan`을 통해 찾은 클래스는 바로 초기화 되어 빈으로 등록되지만 `lazyInit` 설정을 사용하면 실제 해당 클래스가 사용되는 시점에 초기화 됨.

```java
@Configuration
@ComponentScan(basePackages = {"com.package1.component"}, lazyInit = true)
class AppContextConfig{
    ...
}
```

