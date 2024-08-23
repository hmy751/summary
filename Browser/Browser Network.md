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

장점은 상태를 직접 서버가 소유하지 않기 때문에 서버의 증설이나 축소에도 상관없이 대처가 된다. 
단점은 로그인의 사용자와 같은 상태를 유지해야 하는데 이를 할 수가 없다.
이를 보완하기 위해 쿠키나 세션을 이용한다.

### Connectionless
비 연결성은 연결을 유지하지 않는다는 의미다. 
HTTP 요청을 할 때 클라이언트와 서버가 TCP 연결을 통해 잠시 연결하여 통신을 하지만 요청 후 바로 끊는 형식이다.

장점은 서버 자원의 소모가 더 적고, 수많은 클라이언트의 요청에도 대응할 수 있게 된다.
단점은 연결이 끊기면 계속해서 연결을 다시 맺어야 하므로 시간이 추가되며 오버헤드가 크다. 
이러한 단점을 보완하기 위해 HTTP 지속 연결(Persistent Connections)로 보완한다.

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
CORS(Cross-Origin Resource Sharing)는 브라우저가 자신의 출처가 아닌 다른 출처로 자원을 서버에 요청하는 상황에서, 서버가 요청의 허가를 결정하는 HTTP 헤더 기반 메커니즘이다.
단순 요청(Simple requests)이 아닌 사전 요청(Preflighted requests)에 의해서도 결정된다.

브라우저는 기본적으로 동일 오리진 정책을 가지는데 이는 크로스 사이트 요청 위조(CSRF)문제를 방지하고자 구현된 정책이다.
CORS는 동일 오리진 정책을 확장하여 외부의 서드파티 역할을 하는 서버에 승인 하에 리소스를 공유할 수 있도록 한다. 브라우저는 요청을 하고 서버에서 크로스 오리진에 대해 엑세스를 허용하는 응답을 받으면 엑세스를 허용한다. 만약 승인이 안되면 이를 거부하고 CORS에러를 발생시킨다.

따라서 CORS에러를 발생시키지 않고 서드파티 서버에서 리소스를 받으려면 서버의 승인이 필요하며 이는 Access-Control-Allow-Origin속성의 응답을 통해 제어된다.


```http
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: https://foo.example
```

```http
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 00:23:53 GMT
Server: Apache/2
Access-Control-Allow-Origin: *
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml

[…XML Data…]

```
이렇게 되면  Access-Control-Allow-Origin이 와일드 카드 값으로 지정하여 서버는 모든 출처에 대해 허가한다는 의미로 에러가 발생하지 않고 브라우저는 승인되었음을 인지하고 리소스를 받는데 성공한다.

만약 동일 출처로 제한하고 싶다면 요청에 담긴 Origin과 같은 값으로 지정하면 같은 출처에 대해서만 승인한다는 의미로 다른 출처는 접근하지 못하고 에러가 발생한다.
```http
Access-Control-Allow-Origin: https://foo.example
```

### 단순 요청(Simple requests), 사전 요청(Preflighted requests)
#### 단순 요청
단순 요청은 비교적 간단한 데이터 요청이며 데이터의 변경이 없이 조회만 하는 경우다.
기술적으로는 아래의 조건을 만족하는 요청이다.

- 다음 중 하나의 메서드
	- GET
	- HEAD
	- POST
- 사용자 정의 헤더를 포함하지 않고, 브라우저가 기본적으로 설정하는 헤더만 포함한다. 기본 헤더는 아래와 같다.
	- Accept
	- Accept-Language
	- Content-Language
	- Content-Type의 경우 아래의 중 하나로 설정한 경우
		- application/x-www-form-urlencoded
		- multipart/form-data
		- text/plain
- ReadableStram 객체가 사용되지 않는 경우
#### 사전 요청
사전 요청은 CORS에서 데이터의 변경 등 민감한 요청에 대해 보안을 위해 추가된 요청이다.
먼저 OPTIONS 메서드를 통해서 서버에 요청을 보낸다. 이 때 아래 요청 헤더도 포함하여 전송된다.
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type,x-pingother

그럼 서버는 응답으로 아래와 같이 보내며 해당 메서드와 헤더가 허용된다는 것을 응답한다.
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400

사전 요청이 완료되면 그 후에 실제 HTTP 요청이 전송된다. 

Access-Control-Request-Method - 사전 요청 시 사용할 메서드를 의미한다.
Access-Control-Request-Headers - 사전 요청시 사용할 사용자 정의 헤더를 나타낸다.

Access-Control-Allow-Methods - 해당 메서드가 유효한 메서드임을 알린다.
Access-Control-Allow-Headers - 이 헤더들이 실제 사용할 수 있는 허용된 헤더임을 알린다.
Access-Control-Max-Age - 사전요청에 대한 최대 캐시 시간이며 해당 시간동안은 사전 요청을 보내지 않도록 한다.

https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EC%A0%91%EA%B7%BC_%EC%A0%9C%EC%96%B4_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_%EC%98%88%EC%A0%9C

## HTTP Header
HTTP 헤더는 HTTP통신에서 부가적인 정보를 전송할 수 있도록 해준다. 대소문자를 구분하지 않고 이름과 ':' 다음에 오는 값으로 이루어진다.

컨텍스트에 따라 그룹이 나뉜다.
- General Header - 요청 및 응답 메시지 모두 사용가능한 헤더 항목이다.
- Request Header - 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더
- Response Header - 서버 자체에 대한 정보와 같이 응답에 대한 부가적인 정보를 갖는 헤더
- Entity Header - 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 자세한 정보를 포함하는 헤더로 요청과 응답 모두에서 사용 가능하다.

### General Header
- Date - HTTP 메시지를 생성한 일시
- Connection - 클라이언트와 서버 간 연결에 대한 옵션으로 TCP 커넥션에 관한 제어다. 디폴트로 Connection: Keep-Alive가 지정되며 커넥션을 일정기간 유지하게 된다.
- Cache-Control - 캐시 제어와 관련이 있다.

### Entity Header
- Content-Type - 해당 개체에 포함되는 미디어 타입 정보
- Content-Language - 해당 개체와 가장 잘 어울리는 사용자 언어
- Content-Encoding - 해당 개체 대이터의 압축방식
- Content-Length - 해당 개체의 바이트 길이 또는 크기

### Request Header
- Host - 요청 하는 호스트에 대한 호스트 명 및 포트 번호
- User-Agent - 클라이언트 소프트웨어 명칭 및 버전 정보
- From - 클라이언트 사용자 메일 주소
- Cookie - 서버에 의해 설정된 쿠키 정보
- Referer - 바로 직전에 머물던 웹 링크 주소
- Origin - 요청 Origin으로 CORS처리에 관여된다.
- Authorization -  인증 토큰을 서버로 보낼때 사용하는 헤더, 토큰의 종류 + 실제 토큰 문자를 전송

- Accept관련 - 주로 클라이언트가 우너하는 내용을 담는다.
	- Accept - 클라이언트 자신이 원하는 미디어 타입 및 우선순위
	- Accept-Charset - 클라이언트 자신이 원하는 문자 집합
	- Accept-Encoding - 클라이언트 자신이 원하는 문자 인코딩 방식
	- Accept-Language - 클라이언트 자신이 원하는 가능한 언어
	- 각각이 Entity Header 항목들과 대응된다.

### Response Header
- Server - 서버 소프트웨어 정보
- Accept-Range
- Set-Cookie - 서버 측에서 설정하는 쿠키 정보
- Expires - 쿠키 관련
- Age - 쿠키 관련
- Etag - 쿠키 관련
- Proxy-authenticate - 프록시 서버 뒤에 있는 리소스에 접근하는데 사용되어야 하는 인증 메서드를 정의한다.
- Allow - 해당 엔티티에 대한 서버 측에서 지원 가능한 HTTP 메서드의 리스트를 나타낸다.
  OPTIONS에 대한 응답용 항목으로 사용되기도 한다.
- Access-Contorl-Allow-Origin - CORS와 관련이 있다.

https://gmlwjd9405.github.io/2019/01/28/http-header-types.html

# HTTP Cache
---
캐시(Cache)는 자주 사용하는 데이터나 값을 미리 복사해놓는 임시 장소로, 미리 복사해 놓고 재 사용시 계산없이 빠른 속도록 데이터를 접근하게 할 수 있다.

HTTP 캐시는 추후에 재 사용할 수 있도록 HTTP요청에 대한 응답을 저장한다.

## 캐시의 종류
HTTP캐시에는 크게 사설 캐시, 공유 캐시 두 가지가 있다.

### 사설 캐시(Private Cache)
특정 클라이언트에 묶인 캐시로 일반적으로 브라우저 캐시를 의미한다. 즉 다른 클라이언트와 공유되지 않고 개인화된 응답을 저장한다.
만약 컨텐츠를 개인 캐시에만 저장하려면 private 속성을 지정해야 한다.
```HTTP
Cache-contol: private
```

### 공유 캐시(Shared Cache)
공유 캐시는 클라이언트와 서버 사이에 위치하는 캐시로 사용자간에 공유될 수 있다. 공유 캐시에는 프록시 캐시와 관리형 캐시가 있다.

- 프록시 캐시
프록시 캐시는 대표적인 공유 캐시로 회사측 로컬 네트워크나 ISP에 구축된 웹 프록시를 통해 저장하고 사용되는 캐시다.
서비스 개발자가 관리하지 않고 HTTP 헤더로 제어해야 한다.

최근에는 HTTPS가 보편화 되면서 프록시 캐시가 내용을 보지 못할 가능성이 커 중간에 제대로 동작하지 않을 가능성이 있다.
```HTTP
Cache-Control: no-store, no-cache, max-age=0, must-revalidate, proxy-revalidate
```

- 관리형 캐시
관리형 캐시는 서비스 개발자가 원본 서버 대신 우회하여 명시적으로 배포하는 서비스를 통해 사용하는 캐시다. 따라서 서비스 개발자가 직접 컨트롤이 가능하다.
대표적으로는 역방향 프록시, CDN, Cache API와 함께 사용하는 서비스 워커가 있다.

## 캐싱 대상
HTTP 캐시에 저장되는 데이터 덩어리 하나하나를 캐싱 대상, 캐시 엔트리라고 한다. 캐시들은 보통 GET에 대한 응답만을 캐싱하며 다른 메서드들은 제외된다.
기본 캐시키(primary cache key)는 요청 메서드 그리고 대상 URI로 구성되는데 주로 URI만 사용된다.
캐시 엔트리 의 대상은 아래와 같다.
- 성공적인 결과: GET요청에 대해 200 응답
- 영구적 리다이렉트: 301 응답
- 오류 응답: 404 페이지
- 완전하지 않은 결과: 206(Partial Content)
- GET 이외에 캐시 키로서 적절한 정의가 된 응답

캐시 엔트리는 요청이 컨텐츠 협상의 대상인 경우, 두 번째 키에 의해 구별되는 여러개의 응답들로 구성 될 수 있다.

### Vary
Vary를 이용해서 같은 URL로 이루어진 하나의 엔트리를 여러개의 구별되는 응답들로 나눌 수 도 있다. 
예를 들어`Accept-Language`와 같은 속성은 다른 언어의 컨텐츠이고 구별이 필요한데 Vary로 이를 명시하면 응답을 구분하여 엔트리에 저장될 수 있도록 한다.
```
Vary: Accept-Language
```

## Cache-control
먼저 캐시는 만료시간을 가지고 만료 시간 이후에는 재 검증 과정을 거쳐 재 사용되거나 폐기 하는 과정을 거친다.
이 만료시간에 따라 캐시는 fresh, stale 상태로 나뉜다.

### 만료 기간과 fresh, stale상태
먼저 캐시의 상태는 age를 기준으로 판별되며, 응답 생성 이후 경과한 시간을 의미한다.
이 age는 Date를 통해서 알 수 있다.
Cache-control 속성에 max-age 값을 지정할 수 있는데 이 max-age는 최대 경과 시간을 의미하며 캐시의 만료 기간을 지정할 수 있다. 
max-age와 age의 차이값을 통해 수명을 알 수 있으며, 이 유효기간에 따라 fresh상태에서 stale상태로 변경된다.

Expires도 캐시의 수명을 지정하는데 경과 시간을 의미한다.  명시적으로 시간을 지정하여 파싱이나 구현에서 버그가 많아 잘 사용되지 않는다.

Expires, max-age가 둘 다 있을 경우 max-age가 더 우선된다.

### 만료 이후 재 검증
만료 이후 stale 상태가 되면 바로 폐기되지 않고 재검증 을 거친다.
만료 이후 재 검증은 조건부 요청을 통해서 진행된다. 여기에 사용되는 헤더 속성으로 If-Modifed-Since와 If-None-Match가 있다.

Last-Modifed값이 있으면  If-Modifed-Since속성을 통해 서버에 전송하며 재 검증을 하며, 서버가 Last-Modifed값을 기준으로 비교하며 데이터가 최신화 되지 않았다면 304 Not Modified를 통해 응답을 반환하고 기존에 있던 캐시를 갱신하고 다시 사용한다. 
만약 데이터가 최신화 되어 있다면 200응답의 새로운 데이터를 보낸다.

ETag값이 있으면 If-None-Match속성을 통해 서버에 전송하여 재 검증을 한다. ETag값을 비교하여 데이터가 최신화 되어 있다면 200의 응답으로 새로운 데이터를 보내고, 되지 않았다면 304 Not Modified 응답을 통해 회신하여 캐시를 재 사용한다.

Last-Modified, ETag 둘 다 있을시에는 Etag가 더 우선시 되어진다. 
RFC9110에서는 서버에서 응답 시 둘 다 사용할것을 권장한다. 캐시 뿐만 아니라 CMS의 마지막 수정 시간표시, 크롤러에서 크롤링 빈도 조정 등 정보의 목적도 필요하므로 Last-Modified속성도 보내는 것을 권장한다.

### no-cache, no-store 캐시 금지
no-store는 캐시 응답을 저장하지 않겠다는 의미다. 이는 저장하지 않는다는 의미지 이전의 캐시 응답 자체를 삭제한다는 의미는 아니다. 
그래서 이전에 응답이 있을 경우 재 검증을 하지 않기 때문에, no-store를 명시해도 이전 캐시를 재 사용할 여지가 있다.
그리고 개인 정보나 최신 정보를 위해 이를 사용하려고 한다면 no-store보다는 priviate을 고려해 볼 수 있다.
또 no-store를 남용 하다보면 응답을 저장하지 않기 때문에 뒤로/앞으로 가기와 같은 탐색에서도 캐싱이 되지 않을 가능성이 있다.

no-cache는 현재 저장한 캐시를 바로 사용하지 않고 재 검증한다는 의미다. 따라서 재 검증, 데이터의 최신화 확인이 필요한 경우에는 no-store보다는 no-cache가 더 적합하다. 그래서 오래된 응답에 대해서는 no-cache를 통해서 최신화 처리가 가능하다.

### reload, force reload
#### reload
페이지의 새로 고침은 최신 버전의 리소스로 업데이트 하는 경우로 
```http
GET / HTTP/1.1
Host: example.com
Cache-Control: max-age=0
If-None-Match: "deadbeef"
If-Modified-Since: Tue, 22 Feb 2022 20:20:20 GMT
```
와 같은 헤더로 조건부로 전송하여 유효성 검사를 실시하게 한다.

#### force reload
강제 새로 고침은 no-cache모드 동작처럼 모든 리소스를 재 검증하겠다는 의미로 새로고침을 한다.
```http
GET / HTTP/1.1
Host: example.com
Pragma: no-cache
Cache-Control: no-cache
```
헤더는 위와 같은 형태로 no-cache를 통해 진행된다.

### 정적파일의 재 검증 방지
CSS같은 자주 변경되지 않고 오래 유지하는 정적 파일들은 재 검증을 방지하여 사용한다.
```http
Cache-Control: max-age=31536000, immutable
```
max-age를 길게하거나, immutable를 통해서 재검증이 필요하지 않음을 명시적으로 나타낼 수 있다.
이후 재 검증이 필요하게 되면 URL에 버전을 명시하여 데이터를 최신화 한다.

### Cache-control 헤더 디렉티브(Directive)
Cache-control 속성에 디렉티브를 지정하여 캐시를 컨트롤 할 수 있다. 같은 디렉티브라도 요청과 응답에서 사용되는 의미가 약간 다르기도 하며 차이가 있다.
#### 요청에서 디렉티브

| 디렉티브     | 설명                                                                          |
| -------- | --------------------------------------------------------------------------- |
| max-age  | 명시된 시간보다 나이가 많은 응답은 받아들이지 않겠다는 의미며, 기존에 남아있는 캐시를 max-age=0으로 설정하여 막을 수도 있다. |
| no-cache | 저장되어 있는 HTTP응답을 사용하기 전에 반드시 검증(Validation)을 진행한다는 의미다.                      |
| no-store | 해당 HTTP 요청을 통해 받아오는 HTTP응답은 어떠한 종류의 캐시에도 저장하지 않겠다는 의미다.                     |

#### 응답에서 디렉티브

| 디렉티브            | 설명                                                                             |
| --------------- | ------------------------------------------------------------------------------ |
| max-age         | 해당 HTTP 응답이 유효하다고 판단될 수 있는 최대시간이다. 이를 통해 만료시간을 컨트롤 할 수 있다. Expires보다 더 우선시 된다. |
| must-revalidate | 해당 HTTP 응답이 만료되면 반드시 서버에 검증 요청을 보내야 한다는 의미며, 검증 요청을 강제한다.                      |
| no-store        | 해당 HTTP응답을 어떠한 종류의 캐시에도 저장하면 안 된다는 의미다.                                        |
| public          | 해당 HTTP응답이 어떠한 캐시에도 저장될 수 있다는 의미다.                                             |
| private         | 해당 HTTP응답이 개인, 사설 캐시에만 저장될 수 있다는 의미다.                                          |
https://it-eldorado.tistory.com/142

Cache-Control 값에는 max-age, nocache, no-store, private, public
Etag
Last-Modified
expires


If-Modified_since
If-None-Match

stale-while-revalidate

### 디렉티브 외에 HTTP캐시에 관여되는 속성들
- ETag
ETag는 데이터를 식별하는 값으로, 캐시가 만료된 이후 재 검증을 할 때 쓰인다.

- Last-Modified
Last-Modified는 데이터의 최근 수정 날짜를 의미하며, 재 검증의 조건부 요청에 사용되기도 하며 휴리스틱 캐싱으로 판별의 기준으로도 사용된다.

- Expires
max-age와 같이 수명을 판별할 때 사용되는 값이다. 절대적인 값으로 사용이 어려워 max-age사용이 더 권장되고 우선순위에서도 밀린다.

- If-Modified-since
조건부 요청에 사용되는 속성으로 만료후에 Last-Modified값을 통해 재 검증을 요청하는 속성이다. 만료 후 Last-Modified값이 있다면 요청에 If-Modified-since속성으로 전송하여 재 검증을 요청한다.

- If-None-Match
조건부 요청에 사용되는 속성으로 만료후에 ETag값을 통해 재 검증을 요청하는 속성이다. 만료 후 ETag값이 있다면 이를 If-None-Match를 통해 전송하여 재 검증을 요청한다.

https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#reload_and_force_reload
https://toss.tech/article/smart-web-service-cache

# Web Storage
---
## Cookie
쿠키는 서버가 브라우저에 전송하는 작은 데이터 조각으로 브라우저가 받아 서버에 요청을 할 때 함께 전송되는 데이터다.

동일 브라우저에 대한 판단할 때 사용된다. 주로 사용자의 상태를 저장하는데 이용된다.

3가지 목적으로 사용된다.
세션 관리
	서버에 저장해야할 로그인, 장바구니, 게임 스코어등의 정보관리
개인화
	사용자 선호, 테마 등
트래킹
	사용자 행동에 대한 데이터 분석

요즘에는 쿠키보다 웹 스토리지 API인 localStorage, sessionStorage등을 더 많이 권장한다. 쿠키는 모든 요청마다 함께 전송되어 성능이 떨어질 우려가 있다.

### 헤더 설정
Set-Cookie를 통해 서버가 클라이언트에게 저장을 요구한다. 
클라이언트는 해당 정보를 쿠키에 저장하고 이후 모든 전송에 Cookie헤더에 담아 전송하게 된다.
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

### 라이프 타임
쿠키의 라이프 타임은 두가지로 나뉜다 
세션 쿠키와 영속 쿠키로

세션 쿠키는 Expires, Max-Age속성이 없는 쿠키며  브라우저의 세션을 기준으로 삭제된다.
퍼시스턴트, 영구(Persistent, Permanent)쿠키는 Expires, Max-Age속성이 명시된 쿠키며 만료 후 쿠키가 삭제 된다.
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;

### 보안
쿠키를 보호하는 방법에는 Secure, HttpOnly등이 있다.
Secure는 HTTPS 프로토콜에서만 전송되도록 하는 속성이다. 
HttpOnly는 XSS(Cross-site 스크립팅) 공격을 방지하기 위해 사용되며 해당 쿠키는 자바스크립트의 Document.cookie API로 접근하지 못하게 되며 서버에 전송하기만 된다.

SameSite 쿠키는 크로스 출처에 대해 전송을 막는 속성이다 CSRF에 대한 보호 할 수 있다. 아직은 실험단계다.

### 쿠키의 스코프
쿠키의 스코프는 Domain 및 Path 속성으로 범위를 정의한다.

도메인을 지정하지 않는 경우 서브 도메인은 포함되지 않으며 명시되면 서브 도메인들이 항상 포함된다.

Path는 Cookie헤더를 전송하기 하기 위해 요청되는 URL 경로다. 서브 디렉토리까지 포함된다.

## Local Storage, Session Storage
로컬 스토리지와 세션 스토리지는 브라우저에 데이터를 저장하는 방법이다. 키,값 형태로 ㄱ저장할 수 있다.

세션 스토리지는 브라우저의 탭을 기준으로 유지가 된다. 
로컬 스토리지는 window를 기준으로 유지 되며, 명시적으로 제거하지 않는 이상 영구적으로 저장된다.

둘 다 동기적 성격을 가지고 있어 작업이 블로킹이 될 수 있다. 대안으로는 비동기적으로 처리하는 IndexDB가 있다.

## 쿠키와 웹 스토리지의 비교
### 만료 기간
쿠키는 Expires, Max-Age속성으로 시간 제한을 설정할 수 있지만 로컬 스토리지와 세션 스토리지는 불가능 하다

### 소멸 기준
쿠키는 시간 제한을 두면 창을 닫아도 기간 동안 유지 되며, 시간 제한을 두지 않을 경우 창과 함께 소멸 된다.
로컬 스토리지는 명시적으로 삭제해야 소멸 된다.
세션 스토리지는 브라우저의 탭을 기준으로 소멸이 된다.

### 문자열 크기
쿠키는 4KB 작은 데이터 용량이며, 웹 스토리지들은 약 5MB에서 10MB사이의 크기를 가진다.

### 데이터 형태
쿠키는 문자열만 가능하며 이외의 스토리지는 객체로 저장이 가능하다.

### 개발자의 직접 선별, 제어 가능
쿠키는 저장되면 모두 전송되어진다. 그리고 Httponly속성이 지정되면 서버에서만 변경할 수 있다.
이외 웹 스토리지들은 직접 선택해서 전송할 수 있고 자바스크립트로 가공이 가능하다.

## 사용처
쿠키는 세션을 기준으로 유지되며 기한을 지정할 수 있어 세션에 대한 정보를 유지할 수 있고, 보안을 강화할 수 있는 특징이 있다. 브라우저, 유저를 구별할 수 있는 특징이 있어 사용자 인증, 추적 등에 사용이 된다.
로컬 스토리지는 영구 보관되어 장기간 데이터를 보관해야 하는 경우이며, 사용자 설정, 테마 토큰등이 있다
세션 스토리지는 브라우저의 창을 기준으로 유지되어, 단기 데이터를 보관할 때 사용될 수 있다.

# User Auth
---
## 쿠키, 세션 인증
먼저 쿠키와 서버의 세션은 HTTP의 무상태성과 비연결성을 보완하기 위해 사용된다.

쿠키 인증 방식은 고유 식별 정보를 쿠키에 담아 보내는 방식이며, 정보를 클라이언트에서 저장한다. 쿠키 자체는 저장 용량도 작고 정보가 그대로 노출되어 보안에 취약할 수 있다. 

세션 인증은 인증 정보를 서버측 세션에 저장하고 관리하는 방식이다. 유저 인증 후에 유저를 식별하기 위한 정보를 세션에 저장하고 쿠키로 클라이언트에 전송하는 방식이다. 
세션 아이디를 보내어 유의미한 정보는 아니지만 어쨋든 탈취 될 위험이 있으며, 유저가 많아지게 되면 세션 사용으로 서버에 부하가 심해질수도 있다.

## 토큰 인증
토큰 인증 방식은 서버에서 인증 후 인증되었다는 정보를 토큰으로 저장하고 이를 헤더에 담아 클라이언트에 보내며 헤더를 통해 서로 주고받는 형식이다. 클라이언트는 저장한 토큰을 HTTP 요청 헤더에 포함시켜 전송하고 서버는 전달받은 토큰을 검증하고 요청에 응답한다. 또 서버의 DB를 조회할 필요가 없다.

세션을 사용하지 않아 무상태성 특징으로 서버의 부하가 덜 하다는 특징이 있다.
토큰 자체의 데이터 길이가 길어 네트워크 부하가 심해질수 잇다.


## JWT
토큰 인증 방식을 보완할 수 있는 방법이다. JWT(JSON Web Token)은 인증에 필요한 정보들을 암호화 시킨 JSON 토큰이다. 
JWT 인증방식은 암호화된 토큰을 HTTP헤더에 실어 서버가 식별하는 방식이다.

JWT는 JSON데이터를 Base64 URL-safe Encode를 통해 인코딩하여 직렬화 한것이며 변조 방지를 위해 개인키를 통한 전자서명도 들어가 있다.

JWT구조는 Header, Payload, Signature로 나뉜다.
헤더에는 사용할 타입과 해시 알고리즘의 종류가 있고, 페이로드에는 서버에서 보낸 사용자 권한 정보와 데이터가 담겨 있다. 시그니쳐 에는 헤더에서 정의한 알고리즘 방식을 통해 암호화를 한 내용으로 서버에서 비밀키로 복호화가 가능하며 토큰의 위변조 여부를 확인하는데 사용된다.

JWT는 별도의 저장소가 필요 없고, 무상태로 서버의 부하도 적고 확장에도 유리하다 토큰 기반으로 다른 로그인 시스템에 접근 및 권한 공유가 가능하다(쿠키와 차이)

토큰이 길어질 수록 네트워크의 부하가 커지고 stateless이기 때문에 탈취 되면 제어가 불가능하다 따라서 중요한 정보는 담지 말아야 한다.

### Acess Token, Refresh Token
JWT는 탈취 당할 경우 보안에 취약하여 이를 보완하기 위해 Access Token, Refresh Token의 이중 방식을 사용한다.

유효 기간 축소를 통해 탈취를 방지할 수 있지만 로그인을 자주 해야 하며 길게 하면 보안에 취약하게 된다.

Access Toekn은 접근에 관여하며, Refresh Token은 재발급에 관여하는 토큰으로 이를 해결한다.
상대적으로 Access Token은 유효기간을 짧게 주고, Refresh Token은 길게 준다. 

HTTP 요청 시 Access Token을 통해 인가를 통과하며 만료 후에는 재 발급 용도인 Refresh Token으로 재 발급하여 다시 Access Token을 받아 사용하게 된다.

#### 과정
우선 첫 로그인 시 인증 후 서버에서  Access Token과 Refresh Token을 생성하고 Refresh Token을 서버측 DB 에 저장하고 Access Token, Refresh Token둘다 클라이언트에 전송한다.
클라이언트에서는 두 토큰을 받아 저장하고 데이터 요청에 Access Token을 헤더에 담아 보낸다.
만료 전까지는 Access Token을 서버에서 검증하여 데이터 요청을 통과 시킨다.
Access Token이 만료되면 서버에서 이를 알리고 클라이언트에서는 다시 Access Toekn, Refresh Token을 전송한다.
서버에서는 두 토큰을 받아 확인하여 Refresh Token을 DB에 저장되어 있던 Refresh Token을 비교한다. 만료되지않고 유효하다면 다시 새로운 Access Token을 발급하여 클라이언트에 보낸다.
이후 로그아웃 할 경우 서버에서 Refresh Token을 삭제한다

https://tansfil.tistory.com/59

## SSO
SSO(Single Sign-ON)은 한번의 사용자 인증으로 다수의 앱에 사용자 인증을 허용하는 방식이다.

하나의 사용자 정보를 기반으로 여러 시스템을 하나의 통합 인증으로 가능하게 한다.

SSO를 사용하게 되면 여러 사이트에 대한 암호를 기억하며 관리할 필요가 없다. 그리고 수동으로 입력하지 않아도 돼 접근에 대한 생산성이 향상된다.
기업 입장에서는 수많은 유저의 암호를 기억하고, 재설정 요청등에 관리가 필요가 없어 관리 리소스를 줄일 수 있다.

SSO 에는 OAuth, SAML 방식이 있다

### OAuth 2.0
SSO의 한 방법으로 앱이 암호를 제공하지 않고도 다른 웹사이트의 사용자 정보에 안전하게 액세스할 수 있도록 하는 개방형 표준이다.

즉 클라이어트(서비스 기업)이 구글, 페이스북과 같은 플랫폼의 사용자 데이터에 접근하기 위해 사용자의 접근 권한을 위임 받을수 있는 표준 프로토콜이다.

#### 주체
- Resource Owner - 리소스 소유자 유저를 말한다.
- Authorization, Resource Server - Authorization은 유저를 인증하고 Client에게 엑세스 토큰을 발급해주는 서버이며, Resource 서버는 유저의 정보, 리소스를 가지고 있는 서버를 말한다. 두 서버는 같은 곳에서 관리할 수 도 있다.
- Client - 개발하려는 서비스를 말한다. 리소스에 접근하려는 입장에서는 Client로 볼 수 있기 때문이다.

#### 애플리케이션 등록
OAuth 서비스를 이용하기전에 선행되어야 하는 작업으로 Client를 Resource Server에 등록해야 하는 작업이다. 이때 Redirect URI를 등록해야 한다.

Redirect URI는 OAuth서비스 인증에 성공한 유저를 리다이렉션 시키는 URI다. 

등록과정을 마치면, Client ID와 Client Secret을 얻는데  둘은 Access Token을 획득하는데 사용된다. Client Secret은 절대 유츌되어서는 안된다.

#### 동작 메커니즘
1. 로그인 요청
유저가 서비스에서 로그인을 요청하면 Client에서 OAuth 프로세스를 시작한다. 
클라이언트는 인증서버에 사용자의 브라우저를 보내야 하는데 쿼리 스트링에 정보를 담아내서 전송한다. 
인증서버가 제공하는 Authorization URL에 redirect url, client id, scope, response_type등을 보낸다. 
2. 로그인 페이지 제공
클라이언트가 빌드한 Authorization URL로 이동된 유저는 제공된 로그인 페이지에서 ID/PW를 입력하여 인증한다.
3. 인증 성공 및 Authorization Code 발급, Redirect URI로 디렉션
인증이 성공하면 Redirect URI로 Authorization Code를 포함하여 사용자를 리디렉션 시킨다.
4. Authorization Code, Access Token 교환
client는 Authorization Sever에 Authorization code를 전달하고 Access Token을 저장한다.
이후 Resource Server에서 리소스에 접근하기 위해 사용된다.
5. 로그인 성공
이후 클라이언트는 위 과정을 성공적으로 마치면 유저에게 로그인 성공을 알린다.
6. Access Token 리소스 접근
이후 유저 정보에 접근할 때 클라이언트에 요청하면 Access Token을 가지고 클라이언트는 Resouce server에 접근하여 정보를 얻고 서비스를 제공한다.

Authorization code는 리디렉션시 url에 토큰을 직접 전달하면 탈취 가능성이 있어 사용된다.

https://hudi.blog/oauth-2.0/
