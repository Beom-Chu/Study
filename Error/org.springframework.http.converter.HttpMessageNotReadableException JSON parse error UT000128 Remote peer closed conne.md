# org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: UT000128: Remote peer closed connection before all data could be read; nested exception is com.fasterxml.jackson.databind.JsonMappingException: UT000128: Remote peer closed connection before all data could be read (through reference chain: kr.co.x.xx.xxx.dto.XxxInDto["xxxx"]->java.lang.Object[][15]) 오류



해당 오류는 클라이언트에서 보낸 JSON 데이터가 서버 쪽에서 파싱할 수 없는 형식으로 전달되어 발생하는 문제입니다.

에러 메시지에 따르면, `com.fasterxml.jackson.databind.JsonMappingException`과 `UT000128: Remote peer closed connection before all data could be read` 두 가지 예외가 발생한 것으로 보입니다. `JsonMappingException`은 JSON 데이터를 Java 객체로 매핑할 때 발생하는 문제를 의미합니다. `UT000128: Remote peer closed connection before all data could be read`는 클라이언트와 서버 간의 연결이 끊겨서 데이터를 모두 읽지 못한 경우 발생합니다.

이러한 문제를 해결하기 위해서는 다음과 같은 방법들을 시도해 볼 수 있습니다:

1. 클라이언트에서 보내는 JSON 데이터가 올바른 형식으로 전달되었는지 확인합니다. 예를 들어, 배열이나 객체의 끝에 쉼표가 있는 경우에는 이를 수정해 주어야 합니다.
2. 서버에서 설정한 JSON 데이터의 최대 크기를 확인해 봅니다. 클라이언트에서 보내는 데이터가 이 크기를 초과하는 경우 이러한 오류가 발생할 수 있습니다. 이 경우에는 서버에서 JSON 데이터의 최대 크기를 늘려 주거나, 클라이언트에서 보내는 데이터의 크기를 줄여 주는 것이 필요합니다.
3. 서버와 클라이언트 간의 연결이 끊어지는 경우를 대비해서, 클라이언트에서는 데이터를 전송하기 전에 먼저 연결이 정상적으로 유지되고 있는지 확인해야 합니다. 이를 위해서는 클라이언트와 서버 간의 핑(Ping) 테스트를 수행하거나, 서버로부터 응답이 오지 않을 경우 타임아웃 시간을 설정해 두는 것이 좋습니다.
4. 만약 클라이언트가 HTTPS 연결을 사용하는 경우에는, SSL 인증서가 올바르게 설정되어 있는지 확인해 주어야 합니다. 만약 인증서가 올바르게 설정되지 않은 경우, 클라이언트와 서버 간의 연결이 중단되는 경우가 있을 수 있습니다.

위의 방법 중 하나를 시도해 보고도 문제가 해결되지 않는다면, 로그 파일이나 스택 트레이스를 살펴보면서 문제가 발생한 원인을 파악해 볼 필요가 있습니다.