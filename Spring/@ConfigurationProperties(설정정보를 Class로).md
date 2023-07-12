# @ConfigurationProperties(설정정보를 Class로)

*.properties , *.yml 파일에 있는 property를 자바 클래스에 값을 가져와서(바인딩) 사용할 수 있게 해주는 어노테이션

### 단순 형태 Property

``` yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: root
    password: root
```

위와 같은 yml 파일이 등록 되어 있다면?

```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConfigurationProperties("spring.rabbitmq")
@Data
public class ConfigProperties {
    private String host;
    private Integer port;
    private String username;
    private String password;
}
```

```java
@SpringBootTest
class ConfigPropertiesTest {

    @Autowired
    ConfigProperties configProperties;

    @Test
    public void test(){
        System.out.println("[[[configProperties = " + configProperties);
    }
}

// [[[configProperties = ConfigProperties(host=localhost, port=5672, username=root, password=root)
```

이렇게 사용 가능!

### Collection 형태

List, Map 같은 Collection 형태로도 사용 가능하다.

```yaml
config:
  properties:
    users:
      - name: kbs
        age: 20
      - name: ljs
        age: 15
      - name: ksy
        age: 10
    contact:
      mail: abcd@test.com
      tel: 123-4567-8910
```

```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

import java.util.List;
import java.util.Map;

@Configuration
@ConfigurationProperties("config.properties")
@Data
public class ConfigPropertiesCollection {

    private List<User> users;
    private Map<String, String> contact;

    @Data
    static class User {
        private String name;
        private Long age;
    }
}
```

```java
    @Test
    public void testConfigPropertiesList(){

        List<ConfigPropertiesCollection.User> users = configPropertiesCollection.getUsers();
        for(ConfigPropertiesCollection.User user : users) {
            System.out.println("[[[user = " + user);
        }

        System.out.println("[[[getContact = " + configPropertiesCollection.getContact());
    }

// [[[user = ConfigPropertiesCollection.User(name=kbs, age=20)
// [[[user = ConfigPropertiesCollection.User(name=ljs, age=15)
// [[[user = ConfigPropertiesCollection.User(name=ksy, age=10)
// [[[getContact = {mail=abcd@test.com, tel=123-4567-8910}
```

