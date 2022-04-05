# AWS의 유저

### 루트 유저

* 생성한 계정의 모든 권한을 가짐
* 생성시 만든 이메일 주소로 로그인
* 탈튀 당했을 경우 복구가 힘듬 : 사용 자제하고 MFA 설정 필수
* 루투 유저는 관리용으로만 이용 : 계정 설정 변경, 빌링 등
* AWS API 호출 불가(AccessKey / Secret AccessKey 부여 불가)

### IAM 유저

* IAM(Identity and Access Management)을 통해 생성한 유저

* 만들 때 주어진 아이디로 로그인
* 기본 권한 없음 : 따로 권한 부여 필요
  * 예 : 관리자 IAM User, 개발자 IAM User, 디자이너 IAM User
* 사람이 아닌 어플리케이션 등의 가상의 주체를 대표로 가능
* AWS API 호출 가능
  * AccessKey : 아이디 개념
  * Secret Access Key : 패스워드 개념
* AWS의 관리를 제외한 모든 작업은 관리용 IAM User 를 만들어 사용

* 권한 부여시 루트유저와 같은 모든 권한을 가질 수 있지만 빌링 관련 권한은 루트 유저가 허용 해야함.



