# CORS
---
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
위의 예시에서는  Access-Control-Allow-Origin이 와일드 카드 값`*`으로 지정하여 서버는 모든 출처에 대해 허가한다는 의미로 에러가 발생하지 않고, 브라우저는 승인되었음을 인지하고 리소스를 받는데 성공한다.

만약 동일 출처로 제한하고 싶다면 요청에 담긴 Origin과 같은 값으로 지정하면 같은 출처에 대해서만 승인한다는 의미로 다른 출처는 접근하지 못하고 에러가 발생한다.
```http
Access-Control-Allow-Origin: https://foo.example
```

## 단순 요청(Simple requests), 사전 요청(Preflighted requests)

### 단순 요청
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

### 사전 요청
사전 요청은 CORS에서 데이터의 변경 등 민감한 요청에 대해 보안을 위해 추가된 요청이다.
먼저 OPTIONS 메서드를 통해서 서버에 요청을 보낸다. 이 때 아래 요청 헤더도 포함하여 전송된다.
```http
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type,x-pingother
```

그럼 서버는 응답으로 아래와 같이 보내며 해당 메서드와 헤더가 허용된다는 것을 응답한다.
```http
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
```
사전 요청이 완료되면 그 후에 실제 HTTP 요청이 전송된다. 

### CORS 사전 요청관련 HTTP 헤더
Access-Control-Request-Method - 사전 요청 시 사용할 메서드를 의미한다.
Access-Control-Request-Headers - 사전 요청시 사용할 사용자 정의 헤더를 나타낸다.

Access-Control-Allow-Methods - 해당 메서드가 유효한 메서드임을 알린다.
Access-Control-Allow-Headers - 이 헤더들이 실제 사용할 수 있는 허용된 헤더임을 알린다.
Access-Control-Max-Age - 사전요청에 대한 최대 캐시 시간이며 해당 시간동안은 사전 요청을 보내지 않도록 한다.

https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EC%A0%91%EA%B7%BC_%EC%A0%9C%EC%96%B4_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_%EC%98%88%EC%A0%9C

# CSP(Content-Security-Policy)
---
컨텐츠 보안 정책(CSP)은 교차 사이트 스크립팅(Cross-site scripting, CSR)공격을 탐지하고 완하하는 추가 보안계층을 말한다. 

CSP는 HTTP헤더에 추가하여 정책을 추가할 수 있다.
```http
	Content-Security-Policy: policy
```


## XSS(Cross-site-scripting)

크로스 사이트 스크립팅(Cross-site-scripting, XSS)공격은  관리자가 아닌 사용자가 웹에 스크립트를 삽입하게 하는 공격 방법이다.

종류로는 반사형(reflected), 저장형(stored), DOM기반(DOM based)이 있다.
반사형은 악성 URL을 사용자에게 제공하여 클릭시 해당 응답에 대해 악성 스크립트를 보내 실행되어 공격한다.
저장형은 공격자가 입력한 악성 스크립트를 서버에 저장시키고 해당 페이지에 방문하면 사용자가 그 스크립트를 실행하게하여 공격시키는 방법이다.
DOM기반은 서버와 상호작용없이 클라이언트 측에서 DOM을 조작하는 과정에서 발생한다.

방지방법은 
인풋 입력 값, URL 또는 화면 출력 값에 대해 필터링을 하여 스크립트 태그를 문자열로 치환하는 방법으로, 스크립트가 아닌 텍스트로 인식하게 하는 방법이 있다.
또 CSP를 설정하여  외부 스크립트 로드를 제한하여 불필요한 스크립트 실행을 방지할 수 있다.
마지막으로 DOM조작시 innerHTML대신 textContent나 innerText같은 안전한 메서드를 사용하는 방법이 있다.