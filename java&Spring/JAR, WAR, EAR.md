# JAR, WAR, EAR

JAR(Jave Archive), WAR(Web Application Archive) 모두 Java의 jar 툴을 이용하여 생성된 압축(아카이브) 파일이며, 어플리케이션을 쉽게 배포하고 동작시킬 수 있도록 관련 파일(리소스, 속성파일 등)들을 패키징 해주는 것이 주 역할 입니다.

### JAR(Java Archive)

`.jar` 확장자 파일에는 Class와 같은 java 리소스와 속성파일, 라이브러리 및 액세서리 파일이 포함되어 있습니다.

쉽게 Java 어플리케이션이 동작할 수 있도록 **자바 프로젝트를 압축한 파일**로 생각할 수 있습니다.

실제 **JAR 파일은 플랫폼에 귀속되는 점만 제외하면 WIN ZIP파일과 동일한 구조** 입니다.

JAR파일은 **원하는 구조로 구성이 가능**하며 JDK(Java Development Kit)에 포함하고 있는 **JRE(Java Runtime Environment)만 가지고도 실행이 가능** 합니다.

### War(Web Application Archive)

`.war` 확장자 파일은 servlet / jsp 컨테이너에 배치 할 수 있는 **웹 어플리케이션 압축 파일 포맷**입니다.

JSP, SERVLET, JAR, CLASS, XML, HTML, JAVASCRIPT 등 servlet context 관련 파일들도 패키징 되어 있습니다.

WAR는 웹 응용 프로그램을 위한 포맷이기 때문에 **웹 관련 자원만 포함하고 있으며 이를 사용하면 웹 어플리케이션을 쉽게 배포**하고 테스트 할 수 있습니다.

원하는 구성을 할 수 있는 JAR 포맷과 달리 WAR은 WEB-INF 및 META-INF 디렉토리로 **사전 정의된 구조를 사용**하며 **WAR 파일을 실행하려면 Tomcat, Weblogic, Websphere 등의 웹 서버 또는 웹 컨테이너가 필요** 합니다.

**WAR 파일도 JAVA의 JAR 옵션(java - jar)을 이용해 생성하는 jar파일의 일종**으로 웹 어플리케이션 전체를 패키징하기 위한 JAR파일로 생각할 수 있습니다.

### EAR(Enterprise Archive)

EAR 파일은 JAVA EE(Enterprise Edition)에 쓰이는 파일 형식으로 **한 개 이상의 모듈을 단일 아카이브로 패키징하여 어플리케이션 서버에 동시에 일관적으로 올리기 위해 사용**되는 포맷 입니다.