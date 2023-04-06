# MultipartFile과 File

Spring Boot에서 MultipartFile과 File은 파일 업로드 및 처리에 사용되는 두 가지 다른 클래스입니다.

* **MultipartFile**
  * Spring에서 제공하는 인터페이스로, 클라이언트가 업로드한 파일을 나타냅니다. 
  * 이 인터페이스는 멀티파트 요청에서 파일 데이터를 읽을 수 있도록 도와주며, 업로드된 파일의 메타데이터를 제공합니다. 
  * MultipartFile은 스프링의 MultipartResolver 구현체를 사용하여 처리됩니다.

* **File**
  * Java에서 제공하는 클래스로, 파일 시스템에서 파일을 나타냅니다. 
  * 이 클래스는 파일 시스템에서 파일을 읽거나 쓰기 위해 사용됩니다.

따라서, Spring Boot에서 파일 업로드를 처리할 때는 클라이언트에서 업로드된 파일을 나타내는 MultipartFile을 사용하며, 파일 시스템에서 파일을 처리해야 할 때는 File 클래스를 사용해야 합니다.

#### MultipartFile -> File 변환

```java
import java.io.File;
import java.io.IOException;
import org.springframework.web.multipart.MultipartFile;

public class FileUploadUtil {

    public static File convertMultipartFileToFile(MultipartFile multipartFile) throws IOException {
        File file = new File(multipartFile.getOriginalFilename());
        multipartFile.transferTo(file);
        return file;
    }
}
```

#### File -> MultipartFile 변환

```java
import org.springframework.mock.web.MockMultipartFile;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileUploadUtil {

    public static MultipartFile convertFileToMultipartFile(File file) throws IOException {
        FileInputStream input = new FileInputStream(file);
        MultipartFile multipartFile = new MockMultipartFile(file.getName(), file.getName(),
                "application/octet-stream", input);
        return multipartFile;
    }
}
```

