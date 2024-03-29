# AOP 포인트컷(Pointcut) execution() 표현식

### 포인트컷(Pointcut)

포인트컷이란 비즈니스 메소드 중에서 원하는 특정 메소드에게만 횡단 관심에 해당하는 공통 기능을 수행시키기 위해 클래스와 패키지, 메소드 시그니처를 이용해 메소드를 필터링하는 것입니다.

이때 필터링을 위해서 포인트컷 표현식을 활용해 공통 기능을 적용시킬 메소드들을 정밀하게 뽑아 올 수 있습니다.

<br>

### 포인트컷 execution() 표현식

> **execution( [리턴 타입] [패키지 경로] [클래스명].[메소드명] ( [매개 변수] ) )**

메소드를 정확히 뽑아오기 위해서는 위와 같이 5가지 옵션을 설정해주어야 합니다.

##### 샘플

* excution(* ch05obj.\*Service.*(..))
  * 모든 리턴 타입 허용
  * ch05obj 패키지
  * Service로 끝나는 클래스명
  * 모든 메소드
  * 매개 변수의 개수, 타입에 제약 없음

<br>

### 리턴 타입

| 표현식  | 설명                             |
| ------- | -------------------------------- |
| *       | 모든 리턴 타입                   |
| void    | 리턴 타입이 void인 메소드        |
| !String | 리턴 타입이 String이 아닌 메소드 |

### 패키지

| 표현식                       | 설명                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| com.springex.service         | com.springex.service 패키지만 선택                           |
| com.springex.service..       | com.springex.service 패키지와 하위 패키지까지 모두 선택      |
| com.springex.service..member | com.springex.service 패키지의 하위 패키지 중 member인 패키지 |

### 클래스

| 표현식         | 설명                                                    |
| -------------- | ------------------------------------------------------- |
| *Service       | 이름이 Service로 끝나는 클래스                          |
| MemberService+ | 해당 클래스로부터 파생된 모든 클래스, 인터페이스도 포함 |

### 매개변수

| 표현식                             | 설명                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| (..)                               | 모든 매개 변수 선택                                          |
| (*)                                | 매개 변수가 1개인 메소드 선택                                |
| (com.springex.model.Member, *, ..) | 반드시 2개 이상의 매개변수를 가지되 하나는 Member 객체,<br>나머지 하나는 모든 타입의 매개변수를 갖는다.<br>**클래스의 객체를 매개변수로 사용할 땐 패키지 경로가 포함되어야 한다.** |

