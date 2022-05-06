# CloudFront

개발자 친화적 환경에서 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전 세계 고객에게 안전하게 전송하는 고속 콘텐츠 전송 네트워크(CDN) 서비스.

## CDN:Content Delivery Network

* 웹페이지, 이미지, 동영상 등의 컨텐츠를 본래 서버에서 받아와 캐싱
* 해당 컨텐츠에 대한 요청이 들어오면 캐싱해 둔 컨텐츠 제공
* 컨텐츠를 제공하는 서버와 실제 요청 지점간의 지리적 거리가 매우 먼 경우,  혹은 통신 환경이 안좋은 경우 요청 지점 근처의 CDN을 통해 빠르게 컨텐츠 제공 가능
* 서버로 요청이 필요 없기 때문에 서버의 부하를 낮추는 효과

**엣지 로케이션** : 컨텐츠가 캐싱되고 유저에게 제공되는 지점

## CloudFront 동작 방식

* 요청 받은 컨텐츠가 엣지 로케이션에 있다면, 바로 전달
* 요청 받은 컨텐츠가 엣지 로케이션에 없다면, 컨텐츠를 제공하는 근원에서 제공받아 전달

## CloudFront 구성

* Origin : 실제 컨텐츠가 존재하는 근원
  * AWS서비스(EC2, S3, ELB 등)
  * 온프레미스 서버
* Distribution : CloudFront의 CDN 구분 단위로 여러 엣지 로케이션으로 구성된 컨텐츠 제공 채널
* 주요 설정 및 용어
  * TTL(Time To Live) : 캐싱된 아이템이 살아 있는 시간 -> TTL 초 이후 캐싱에서 삭제
  * 파일 무효화(Invalidate) : TTL이 지나기 전에 강제로 캐시를 삭제하는 것
    * 주로 새로운 파일을 업데이트 한 후 TTL을 기다리지 못할 상황에 사용
    * 비용 발생
    * CloudFront API, 콘솔, Third-Party 툴 등을 사용 가능

* Cache Key
  * 어떤 기준으로 컨텐츠를 캐싱할 것인지 결정
  * 기본적으로 URL
  * 이후 Header와 Cookie, 쿼리 스트링 등을 사용 가능
* 정책(Policy)
  * CloudFront가 동작하는 방법을 정의
  * 어떻게 캐싱을 할지, 어떤 내용을 Origin에 보낼지, 어떤 헤더를 허용할 지 등을 결정


