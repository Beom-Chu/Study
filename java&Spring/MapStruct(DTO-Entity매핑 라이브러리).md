# MapStruct(DTO-Entity매핑 라이브러리)

* 장점

  * DTO – Entity 간 매핑 간편화	

  * Entity 업데이트 코드 간소화

<br>

### 의존성 추가

##### gradle

```groovy
dependencies {
    // ......
    implementation 'org.mapstruct:mapstruct:1.4.1.Final'
    compileOnly 'org.projectlombok:lombok'
    compileOnly 'org.projectlombok:lombok-mapstruct-binding:0.2.0'
    annotationProcessor 'org.projectlombok:lombok'
    annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.1.Final'
    // ......
}
```

##### maven
```xml
<!-- https://mvnrepository.com/artifact/org.mapstruct/mapstruct -->
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.4.2.Final</version>
</dependency>
```

<br>

## DTO – Entity 생성

DTO 와 Entity 의 필드명을 1:1 일치 권장

`@Setter`, `@Getter`, `@NoArgsConstructor` 가 추가 확인

```java
@Setter
@Getter
@NoArgsConstructor
@Entity
public class Brands extends BaseTimeEntity implements Persistable<String> {

    @Id
    private String brandId;

    @NotNull
    @Column(length = 32)
    private String partnerId;

    @NotNull
    @Column(length = 128)
    private String brandName;

    @NotNull
    @ColumnDefault("Y")
    @Column(length = 1)
    private String useyn = "Y";

    public Brands(String brandId) {
        this.brandId = brandId;
    }

    @Override
    public String getId() {
        return brandId;
    }

    @Override
    public boolean isNew() {
        return getRegdate() == null;
    }
}
```
```java
@Getter
@Setter
@NoArgsConstructor
public class BrandsDto extends BaseTimeDto implements Serializable {

    @ApiModelProperty(example = "conitale")
    private String brandId;

    @ApiModelProperty(example = "conitale")
    private String partnerId;

    @ApiModelProperty(example = "코니테일")
    private String brandName;

    @ApiModelProperty(example = "Y")
    private String useyn;
}
```

<br>

## GenericMapper 생성

DTO – Entity 간 필드명을 동일하게 생성한 경우 수동 매핑 불필요

`updateFromDto()` 는 Entity 업데이트시 값이 null 이 아닌 값만 업데이트 하도록 가능

```java
public interface GenericMapper<D, E> {

    D toDto(E e);
    E toEntity(D d);

    @BeanMapping(nullValuePropertyMappingStrategy = NullValuePropertyMappingStrategy.IGNORE)
    void updateFromDto(D dto, @MappingTarget E entity);
}
```

<br>

## Mapper 생성

상속만으로 코딩이 끝나며, 필드명이 다르면 하나 하나 수동으로 매핑 필요.

```java
@Mapper(componentModel = "spring")
public interface BrandsMapper extends GenericMapper<BrandsDto, Brands> {

//    @Override
//    @Mapping(source = "name", target = "modelName")
//    @Mapping(source = "color", target = "modelColor")
//    BrandsDto toDto(Brands brands);
}
```

<br>

## Service 생성

```java
@Service
public class BrandsService {

    private final BrandsMapper mapper = Mappers.getMapper(BrandsMapper.class);

    // ......

    public void update(String id, BrandsDto d) {

        Brands e = get(id, true);
        updateFromDto(d, e);
        repository.save(e);
    }

    public D select(String id) {
        return toDto(get(id, true));
    }

    protected BrandsDto toDto(Brands brands) {
        return mapper.toDto(brands);
    }

    protected Brands toEntity(BrandsDto e) {
        return mapper.toEntity(e);
    }

    protected void updateFromDto(BrandsDto dto, Brands brands) {
        mapper.updateFromDto(dto, brands);
    }
}
```

<br><br><br><br>

출처 : https://www.skyer9.pe.kr/wordpress/?p=1596





