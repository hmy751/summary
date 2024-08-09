# HTTP
---
HTTP(Hypertext Transfer Protocol)는 통신 프로토콜로 클라이언트와 서버간의 인터넷 통신에서 필요한 정보를 요청하는 방법이다.
애플리케이션 계층의 프로토콜이다.

## HTTPS
HTTPS(Hypertext Transfer Protocol Secure)는 브라우저와 서버간에 통신할 때 암호화 연결을 설정하여 통신을 더 안전하게 만드는 HTTP를 확장한 프로토콜이다. 
HTTP의 TCP연결에 이어 SSL/TLS 인증서 확인 및 키를 통한 인증과 데이터의 암호화 통신을 통해 안정성을 더 증가시킨다.

SSL(Secure Sockets Layer)/TLS(Transport Layer Security)는 전송 계층 보안으로 인터넷 통신을 보호하는 암호화 프로토콜이다. 둘 다 같다고 보면된다.

### TCP, TLS 핸드 쉐이크 과정

https://blog.naver.com/taeyoun795/220818138527
https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/
https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/
^21f9ea

## HTTP 특징
### Stateless
무상태성이란 클라이언트와 서버사이에 상태를 유지하지 않음을 의미한다.
예를 들어 로그인을 한 경우 로그인에 대한 상태를 서버가 보존하지 않고 토큰 형태로 저장하여 통신에 실어서 보내는 형태로 전달한다.
상태를 직접 서버가 소유하지 않기 때문에 서버의 증설이나 축소에도 상관없이 대처가 된다. 
다만 단점은 데이터를 상대적으로 더 많이 사용한다.

### Connectionless
비연결성은 연결을 유지하지 않는다는 의미다. 
HTTP 요청을 할 때 클라이언트와 서버가 TCP 연결을 통해 잠시 연결하여 통신을 하지만 요청 후 바로 끊기 때문에 서버 자원의 소모가 더 적게 된다.

## HTTP 메시지
메시지 구조는 Start Line, Header, Empty Line, Message Body로 구성된다.
시작줄에는 메서드, 요청 타겟, HTTP 버전이 있다.
헤더는 부가 정보들이 담긴다.
빈줄은 헤더와 바디를 구분하기 위해 들어간다.
바디에는 실제 전송할 데이터들이 담긴다.
![[CS/Network.excalidraw.md#^group=DDgYSY-Ns_HMJwik63ysp|1200]]
## HTTP 메서드
주로 쓰는 메서드는 5가지가 있다.
- GET - 리소스 조회, 오직 데이터를 받기만 한다.
- POST - 요청 데이터 처리, 주로 신규 등록에 사용된다.
- PUT - 리소스를 대체 즉 전체를 변경할 때 사용된다. 만약 없다면 새로 생성한다.
- PATCH - 리소스의 일부분을 수정한다.
- DELETE - 특정 리소스를 삭제한다.
https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-HTTP-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%A2%85%EB%A5%98-%ED%86%B5%EC%8B%A0-%EA%B3%BC%EC%A0%95-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

### HTTP 메서드의 특성
- Safe(안정성) - 호출해도 리소스가 변경되지 않는 성질을 의미하며 GET이 이에 해당된다.
- Idempotent(멱등성) - 여러번 요청해도 결과 값이 달라지지 않음을 의미한며 서버의 상태가 여러 번 요청 돼도 변하지 않는다. GET, PUT, DELETE가 해당한다.
- Cacheable(캐시 가능) - 응답 결과를 캐싱해서 효율적으로 사용할 수 있는가에 대한 여부다. GET이 이에 해당하며 POST, PATCH는 스펙상으로 가능하지만 문제가 생길수 있어 일반적으로 지원되지 않는다.

## HTTP 상태 코드
상태 코드는 처리상태를 알려주며, 100~500번대의 숫자로 구성된다.
- 1xx - 요청을 받았으며 계속 진행해도 된다는 의미다.
- 2xx - 성공 응답으로, 요청이 성공적으로 됐음을 의미한다.
- 3xx - 리다이렉션, 요청 완료를 위해 추가 작업 조치인 리다이렉션이 필요함을 의미한다.
- 4xx - 클라이언트의 오류로, 요청의 문법이 잘못되거나 해당 리소스가 없음을 나타낸다.
- 5xx - 서버의 오류를 의미한다.

https://inpa.tistory.com/entry/HTTP-%F0%9F%8C%90-%EB%B0%B1%EC%97%94%EB%93%9C-%EB%A1%9C%EB%93%9C%EB%A7%B5-HTTP%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C%EC%9A%94

## HTTP 버전
### HTTP 1.0
HTTP 1.0은 한 연결당 하나의 요청을 처리하도록 되어 있다. 매 요청마다 TCP 핸드 쉐이크 요청을 해야 하기 때문에 RTT가 증가한다.
> RTT(Round Trip Time)는 요청 부터 요청에 대한 응답을 받을 때까지의 왕복시간을 의미한다.

그 다음 발전한 것이 1.1이다. 1.1은 한 번 TCP 연결을 한 후에는 Connection: Keep-Alive 옵션을 통해 이후 요청에도 연결이 끊기지 않고 유지되게 됐다 1.1부터는 기본 옵션으로 설정되었다.
### HTTP 2.0
HTTP 2.0 부터는 1.0보다 RTT을 줄이기 위해 멀티 플렉싱, 헤더 압축, 서버 푸시, 요청의 우선순위 처리등이 지원 됐다.
멀티 플렉싱은 한 커넥션에서  HTTP메시지를 바이너리 형태의 프레임으로 나누고 여러개의 메시지 스트림을 응답 순서에 상관없이 주고 받는 것을 말한다. 병렬로 실행하기 때문에 처리속도가 빠르다.
(frame - message - stream - connection)
2.0부터는 헤더를 압축하여 전송한다. 그리고 중복 되는 필드를 재전송하지 않는다.
서버 푸쉬는 요청을 해석하여 서버에서 미리 css, js, 이미지 파일등을 클라이언트에 보내는 것을 말한다.

https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-HTTP-20-%ED%86%B5%EC%8B%A0-%EA%B8%B0%EC%88%A0-%EC%9D%B4%EC%A0%9C%EB%8A%94-%ED%99%95%EC%8B%A4%ED%9E%88-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90

### HTTP 3.0
3.0부터는 TCP가아닌 UDP기반의 QUIC 프로토콜을 사용한다. 그래서 핸드쉐이크 과정이 필요없이 진행된다.
그리고 처음 연결 설정 시 인증 정보도 같이 전송하여 한번에 연결하고 통신을 시작할 수 있게 된다.
TCP의 HOLB 현상을 해결했다.

## CORS


# TCP/IP 5계층
---
## 물리 계층
- MAC 주소
- LAN 카드
- 케이블

## 네트워크 계층
### IP
IP(Internet Protocol)는 엔드투엔드 통신의 역할을 맡으며 PC간의 통신을 담당한다. 
IP데이터는 패킷으로 부르며 구성은 IP헤더와 TCP세그먼트의 조합으로 구성되어 있다.
TCP세그먼트에 IP헤더를 붙이면 IP패킷이 된다.

- IP 헤더
IP 헤더의 형식은 IPv4라고 해서 
https://fastercapital.com/ko/content/IP-%EC%A3%BC%EC%86%8C--IP%EC%97%90%EC%84%9C-ISP%EA%B9%8C%EC%A7%80--%EC%9D%B8%ED%84%B0%EB%84%B7-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EC%9D%B4%ED%95%B4.html
https://study-recording.tistory.com/7
### Encapsulation, Decapsulation

### 라우팅 프로토콜
## 트랜스포트 계층

- 트랜스포트 계층
- TCP,IP
- UDP

## 어플리케이션 계층

- 3 way TLS 핸드 쉐이크
### DNS

^7823ff

DNS(Domain Name System)는 전화 번호부 처럼 고유한 IP주소를 도메인 이름으로 저장하는 시스템이다.
DNS 룩업
ARP
https://ko.wix.com/blog/post/domain-name-system-dns
https://aws.amazon.com/ko/route53/what-is-dns/?nc1=h_ls
### 프록시 서버

# 네트워크 보안
---
공개키 방식

대칭키 방식