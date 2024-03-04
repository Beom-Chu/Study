# AttributeConverter

### AttributeConverter란

Entity와 DB사이에서 속성의 변환을 담당합니다.
Converter가 지정되지 않은 속성의 경우 조회 시 DB의 데이터를 특정 속성의 값으로 읽어오며, 저장 또는 수정 시 Entity의 속성 값을 DB의 컬럼에 저장합니다.
반면 Converter가 지정된 속성의 경우는 조회 시 DB의 데이터를 읽어와 Converter의 특정 동작을 수행 후 결과를 Entity속성에 값을 입력하며, 
저장 또는 수정 시 Entity의 속성 값을 Converter의 특정 동작을 수행 후 결과를 DB의 컬럼에 저장합니다.

### Attribute Converter Interface

`AttributeConverter` 인터페이스의 경우 2개의 메소드만 정의되어 있는 아주 간단한 인터페이스입니다.
정의되어 있는 2개의 메소드 `convertToDatabaseColumn`와 `convertToEntityAttribute`를 구현함으로써 Converter를 만들 수 있습니다.

```java
/**
 * A class that implements this interface can be used to convert 
 * entity attribute state into database column representation 
 * and back again.
 * Note that the X and Y types may be the same Java type.
 *
 * @param <X>  the type of the entity attribute
 * @param <Y>  the type of the database column
 */
public interface AttributeConverter<X,Y> {

    /**
     * Converts the value stored in the entity attribute into the 
     * data representation to be stored in the database.
     *
     * @param attribute  the entity attribute value to be converted
     * @return  the converted data to be stored in the database 
     *          column
     */
    public Y convertToDatabaseColumn (X attribute);

    /**
     * Converts the data stored in the database column into the 
     * value to be stored in the entity attribute.
     * Note that it is the responsibility of the converter writer to
     * specify the correct <code>dbData</code> type for the corresponding 
     * column for use by the JDBC driver: i.e., persistence providers are 
     * not expected to do such type conversion.
     *
     * @param dbData  the data from the database column to be 
     *                converted
     * @return  the converted value to be stored in the entity 
     *          attribute
     */
    public X convertToEntityAttribute (Y dbData);

}
```

AttributeConverter Generic Type의 값들은 Entity에 저장되는 속성 Type과, DB에 저장될 컬럼의 Type을 전달합니다.
AttributeConverter의 주석에 작성된 것처럼 **첫 번째 Generic Type은 Entity의 속성 Type을 전달하고, 두 번째 Generic Type은 DB에 저장될 컬럼 Type을 전달**합니다.
convertToDatabaseColumn는 Entity의 속성 값을 DB에 저장하기 전 변경 동작을 구현하는 메소드입니다.
반대로 convertToEntityAttribute는 DB에서 읽어온 데이터를 Entity의 속성에 입력하기 전 변경 동작을 구현하는 메소드 입니다.

### Attribute Converter 구현

##### Converter

```java
@Component
@Converter
public class CustomConverter implements AttributeConverter<String, String> {
    @Override
    public String convertToDatabaseColumn(String attribute) {
        return attribute + "-" + LocalDateTime.now().format(DateTimeFormatter.BASIC_ISO_DATE);
    }

    @Override
    public String convertToEntityAttribute(String dbData) {
        return dbData.substring(0, dbData.indexOf("-"));
    }
}
```

`convertToDatabaseColumn` 메소드에 attribute 데이터에 `-`기호와 현재일자를 덧붙여서 DB에 등록하도록 하고,
`convertToEntityAttribute` 메소드에 DB 데이터에 `-` 이후의 문자는 잘라내도록 구현해보았습니다.

##### Entity

```java
@Entity
@Getter
@Setter
@ToString
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class ConverterUser {

    @Id
    @GeneratedValue
    private Long id;
    private String name;
    @Convert(converter = CustomConverter.class)
    private String convertName;
}
```

`convertName` 필드에 위에서 구현한 `CustomConverter`를 사용해서 변환되도록 설정했습니다.

### 테스트 결과

```sh
Hibernate: 
    call next value for hibernate_sequence
Hibernate: 
    call next value for hibernate_sequence
Hibernate: 
    insert 
    into
        converter_user
        (convert_name, name, id) 
    values
        (?, ?, ?)
2023-11-27 14:55:37.913 DEBUG 22120 --- [           main] tributeConverterSqlTypeDescriptorAdapter : Converted value on binding : kbs -> kbs-20231127
2023-11-27 14:55:37.917 TRACE 22120 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [VARCHAR] - [kbs-20231127]
2023-11-27 14:55:37.917 TRACE 22120 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [2] as [VARCHAR] - [kbs]
2023-11-27 14:55:37.919 TRACE 22120 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [3] as [BIGINT] - [1]
Hibernate: 
    insert 
    into
        converter_user
        (convert_name, name, id) 
    values
        (?, ?, ?)
2023-11-27 14:55:37.923 DEBUG 22120 --- [           main] tributeConverterSqlTypeDescriptorAdapter : Converted value on binding : ljs -> ljs-20231127
2023-11-27 14:55:37.923 TRACE 22120 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [VARCHAR] - [ljs-20231127]
2023-11-27 14:55:37.923 TRACE 22120 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [2] as [VARCHAR] - [ljs]
2023-11-27 14:55:37.923 TRACE 22120 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [3] as [BIGINT] - [2]
```

DB Insert Log를 보면 convert_name 컬럼에는 `-` 기호와 현재일자가 붙어서 등록되는 것을 확인 할 수 있습니다.