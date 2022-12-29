# [Java] Serializable

### 직렬화(Serialize)

* 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 시스템에서도 사용 할 수 있도록 Byte 형태로 변환하는 기술.
* JVM의 메모리에 상주되어 있는 객체 데이터를 Byte 형태로 변환하는 기술.

### 역직렬화(Deserialize)

* Byte로 변환된 데이터를 원래의 객체나 데이터로 변환하는 기술.
* 직렬화된 Byte형태의 데이터를 JVM으로 상주시키는 형태.

### 직렬화 조건

* java.io.Serializable 인터페이스를 상속받은 객체가 직렬화 할 수 있는 기본 조건 입니다.

```java
public calss Member implements Serializable {
  private String name;
  private String email;
  private int age;

  public Member(String name, String email, int age){
    this.name = name;
    this.email = email;
    this.age = age = age;
  }
}
```

### 직렬화 방법

* java.io.ObjectOutputStream 을 사용하여 직렬화합니다.

```java
public static void main(String[] args){
    Member member = new Memer("kim", "kim@sample.com", 30);
    byte[] serializeMember;
    try(ByeArrayOutputStream baos= new ByteArrayOutputStream()){
        try(ObjectOutputStream oos= new ObjectOutputStream(baos)){
            oos.writeObject(member);
            // serializedMember -> 직렬화된 member 객체
            serializedMember = baos.toByteArray();
        }
    }
    //바이트 배열로 생성된 직렬화 데이터를 base64로 변환
    System.out.println(Base64.getEncoder().encodeToString(serializedMember));
}
```

### 역직렬화 방법

* java.io.ObjectInputStream 을 사용하여 역직렬화 합니다.

```java
public static void main(String[] args){
    //직렬화 예제에서 생성된 base64 데이터
    String base64Member="...생략";
    byte[] serializedMember = Base64.getDecoder().decode(base64Member);
    try(ByteArrayInputStream bais = new ByteArrayInputStream(serializedMember)){
        try(ObjecInputStream ois = new ObjectInputStream(bais)){
            //역직렬화된 member객체를 읽어온다.
            Object objectMember = ois.readObject();
            Member member = (Member)objectMember;
            System.out.println(member);
        }
    }
}
```

### 자바 직렬화를 사용하는 이유

* 복잡한 데이터 구조의 클래스 객체라도 직렬화 기본 조건만 지키면 큰 작업없이 바로 직렬화/역직렬화가 가능합니다.
* 데이터 타입이 자동으로 맞춰지기 때문에 관련 부분을 크게 신경쓰지 않아도 됩니다.

### 주의 사항

* 객체의 버전간 호환성을 유지하기 위해서는 `serialVersionUID` 정의가 필수 입니다.
  * Default는 클래스의 기본 해쉬값을 사용합니다.
* 멤버 변수의 타입이 변경된다면 Exception이 발생합니다.
* 멤버 변수가 빠지게 된다면 Exception 대신 null 값이 들어가게 됩니다.
* 일반적인 메모리 기반의 Cache에서는 Data를 저장 할 수 있는 용량의 한계가 있기 때문에 Json형태와 같은 경량화된 형태로 직렬화 하는 것도 좋은 방법입니다.
* 자주 변경되는 비즈니스적인 데이터의 자바 직렬화는 지양합니다.