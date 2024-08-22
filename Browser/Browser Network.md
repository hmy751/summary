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
CORS(Cross-Origin Resource Sharing)는 브라우저가 자신의 출처가 아닌 다른 출처로 자원을 서버에 요청하는 상황에서, 서버가 요청의 허가를 결정하는 HTTP 헤더 기반 메커니즘이다.
단순 요청(Simple requests)이 아닌 사전 요청(Preflighted requests)에 의해서도 결정된다.

브라우저는 보안 상의 이유로 자기 자신의 출처가 아닌 HTTP응답에 대해서 CORS에러를 발생시켜 응답을 거부한다.
하지만 다른 출처에서의 리소스가 필요한 경우가 있다. 다른 출처의 리소스를 받기 위해서 Origin, Access-Control-Allow-Origin를 통해 컨트롤을 한다. 

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
이렇게 되면 서버는 모든 출처에게 자신의 리소스를 접근할 수 있음을 의미한다.

만약 제한하고 싶다면 
```http
Access-Control-Allow-Origin: https://foo.example
```
출처를 명시하며 이런 상황에서는 foo.example이외의 도메인에서는 교차출처정책 위반으로 접근하지 못한다.

### 단순 요청(Simple requests), 사전 요청(Preflighted requests)
#### 단순 요청
단순 요청은 비교적 간단하게 데이터를 요청이며 데이터의 변경이 없는  
기술적으로 아래의 조건을 만족하는 요청이다.

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
- RedableStram 객체가 사용되지 않는 경우
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


https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EC%A0%91%EA%B7%BC_%EC%A0%9C%EC%96%B4_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_%EC%98%88%EC%A0%9C

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
종류

생명 주기



# User Auth
---
쿠키

세션

토큰

JWT