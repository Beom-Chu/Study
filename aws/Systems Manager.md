# Systems Manager

완전 관리형 AWS 서비스로 EC2 및 온프레미스 인스턴스, 가상머신을 브라우저 기반의 쉘 혹은 AWS CLI로 관리 할 수 있는 서비스

## Session Manager

* 인스턴스에 대해 원클릭 액세스를 제공하는 관리형 서비스
* 인스턴스에 SSH 연결 없이, 포트를 열 필요 없이, Bastion 호스트를 유지할 필요 없이 인스턴스에 로그인 가능
* IMA 유저 단위로 제어 가능 (Key파일로 제어할 필요 없음)
  * 예 : 수백개의 인스턴스에 대해 일일이 로그인을 위한 키 파일을 관리해야 할 때
  * 개발자 별로 지정된 팀의 인스턴스만 로그인 할 수 있도록 하고 싶을 때
* 웹 브라우저 기반으로 OS와 무관하게 사용 가능
* 로깅과 감사
  * 언제 어디서 누가 접속했는지 확인 가능 (CloudTrail)
  * 접속 기록과 사용한 모든 커맨드 및 출력 내역을 S3 혹은 CloudWatch로 전송 가능
  * AWS의 서비스와 연동되어 있어 다양한 시나리오 구현 가능
    * 예 : EventBridge 등과 연동하여 실시간 접근에 대한 알림 받기