TCP/IP 4계층은 네트워크에 사용되는 프로토콜을 네개의 모델로 나누어 추상화 한 개념이다.

# Application Layer
---
애플리케이션 게층은 응용프로그램이 사용되는 계층으로 실질적으로 사람들에게 제공하는 계층이다. 사용자와 네트워크 간의 상호작용을 담당한다.

주요 프로토콜에는 HTTP, DNS, FTP, TFTP, SMTP등이 있다.

### HTTP/HTTPS
[[Browser Network#^f89b88]]

### DNS
^7823ff
DNS(Domain Name System)는 전화 번호부 처럼 고유한 IP주소를 사람이 해석하기 쉬운 도메인 이름으로 저장하고, 또 반대로 변환하여 고유한 IP주소를 찾을수 있도록 하는 관리 시스템을 말한다.

DNS에서 IP주소 검색 과정의 구성은 DNS서버와 DNS리졸버가 있고 이를 통해서 진행된다.

먼저 DNS서버는 호스트명과 IP주소의 대응 관계 및 여러 정보들인 리소스 레코드를 저장하는 장소다.

DNS 리졸버는 도메인 주소를 가지고 도메인 서버와 재귀적으로 질문 및 응답과정을 통해 저장하고 있는 DNS서버에서 IP주소를 찾는 역할을 한다. 
조회 과정은 DNS루트 서버에서 부터 시작하며 URL의 루트 부분부터 해당하는 요소들의 DNS서버들을 찾아나간다.

DNS의 리졸버의 재귀 질의를 통해 IP주소를 찾으면 이를 브라우저에게 전송하여 도메인에 해당하는 IP주소를 제공한다.

https://ko.wix.com/blog/post/domain-name-system-dns
https://aws.amazon.com/ko/route53/what-is-dns/?nc1=h_ls

# Transport Layer
---
전송 계층이라고도 하며 애플리케이션과 인터넷 계층 사이의 데이터가 전달 될때 중계역할을 한다.
데이터의 무결성과 신뢰성을 보장하며, 전송 중 오류를 검출하고 복구하여 안정적인 통신을 제공한다.

대표적인 프로토콜로는 TCP, UDP가 있다.

TCP는 연결지향 프로토콜을 사용해서 연결하며 신뢰성을 확보해준다.
UDP는 비연결형 프로토콜로, 단순 데이터그램 패킷 교환방식을 사용하며 데이터만 주고 받는다.

## TCP 3-way Handshake

TCP는 통신과정에서 연결지향 프로토콜로 정확한 접속을 성립 하기위해 3-way handshake 과정을 거친다. 

![[CS/Network/Network.excalidraw.md#^group=_Mojm0vUFf4SCfdvBS_uZ|1200]]
과정은 
Client가 Server에게 SYN패킷을 전달하고
Server에서 Client에게 이에 대한 응답으로 SYN-ACK응답을 보낸다.
Client가 다시 Server에게 응답을 확인한다는 의미로 ACK패킷을 보내며 연결이 이루어진다.

https://s3-ap-northeast-1.amazonaws.com/exemdocuments/intermax/wh-apm_TCP.pdf

## TLS Handshake

HTTP프로토콜이 아닌 HTTPS 프로토콜 통신에서는 TCP연결요청에 이어 TLS인증과정이 필요하다.

TLS인증과정은 TCP요청처럼 핸드 쉐이크 과정을 거치는데 차이점은 단순히 메시지만을 교환하지 않고 인증과정이 추가된다.

크게 공개 키 방식을 통해 인증을 하고 인증에 성공하면 대칭 키를 서로 가져 통신을 하게 된다.
먼저 대칭 키를 주고 받기 위한 공개 키 방식의 인증 과정이 시작된다.

![[CS/Network/Network.excalidraw.md#^group=pwS_pZEJ57tR10bJQqs_g|1200]]

(ClientHello) Client에서 Server에 초기 암호 설정을 제안한다.

(ServerHello) Server가 이를 수락하고 암호 설정을 응답한다.
(Certificate) Server가 디지털 인증서를 Client에 전달한다.
(Server Hello Done) Server가 Server Hello 절차를 완료되었음을 알리는 메시지 Server Hello Done 메시지를 전달한다.

(Client Key Exchage) Client가 Server의 인증서를 검증한다. 검증 후 세션 키 생성에 필요한 Pre-master Secret값을 생성하고 Server의 공개 키로 암호화 하여 Server에 전송한다.
(Change Cipher Spec) Client와 Server는 Change Cipher Spec 메시지를 통해 세션 키, 대칭 키 방식을 통해 통신할 것이라 전달한다.
(Finished) 서로 세션 키를 생성하고 처음으로 세션 키를 통해 암호화 하여 Finished 메시지를 전달한다.

https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/

# Internet Layer
---
인터넷 계층은 데이터를 목적지까지 전달하기 위해 네트워크 간의 경로를 설정하고 관리하는 계층이다.
네트워크 사이에서 네트워크 패킷을 라우팅 및 IP주소로 지정된 목적지로 전송하는 역할을 한다.

주요 프로토콜로는 IP, ARP, ICMP등이 있다.

구성요소에는 라우터와 네트워크 인터페이스가 해당한다.

패킷이 올바른 목적지에 도달할 수 있도록 하며, 논리적 주소(IP)를 기반으로 네트워크 내 모든 장치를 식별한다. 
또 라우팅 프로토콜을 통해 가장 효율적인 경로를 설정하여 네트워크 성능을 올린다.

### IP
IP(Internet Protocol)는 엔드투엔드 통신의 역할을 맡으며 PC간의 통신을 담당한다. 
IP데이터는 패킷으로 부르며 구성은 IP헤더와 TCP세그먼트의 조합으로 구성되어 있다.
TCP세그먼트에 IP헤더를 붙이면 IP패킷이 된다.

- IP 주소
IP에서 엔드투엔드 즉 출발지에서 목적지까지 전달하려면 주소가 제일 중요하다. IP주소는 호스트의 인터페이스를 식별하는 주소다.

- IP 헤더
IP 헤더의 형식은 IPv4라고 해서 통신을 위한 정보를 가지는데 제일 중요한 정보는 출발지, 목적지의 IP주소를 가지고 이를 통해 통신을 처리한다.
https://fastercapital.com/ko/content/IP-%EC%A3%BC%EC%86%8C--IP%EC%97%90%EC%84%9C-ISP%EA%B9%8C%EC%A7%80--%EC%9D%B8%ED%84%B0%EB%84%B7-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EC%9D%B4%ED%95%B4.html
https://study-recording.tistory.com/7

### ARP
ARP는 TCP/IP의 IP주소와 인터페이스를 식별하기 위한 MAC주소를 대응시키거나 구하는 역할을 한다.

예를 들어 이더넷에서 데이터를 전송할 때 IP헤더의 IP주소에 해당하는 MAC주소를 패킷화하여 전송해야 하는데 이때 ARP가 IP주소에 해당하는 MAC주소를 찾아 구하는 역할을 한다.

### 라우팅 프로토콜
라우팅 프로토콜은 네트워크 장치 라우터간에 네트워크 경로 정보를 교환하여 최적의 데이터 전송 경로를 결정하는 프로토콜이다. 장치내 데이터 패킷을 목적지까지 효율적으로 전달할 수 있도록 도와준다.


# Network Interface Layer
---
네트워크 인터페이스 계층은 네트워크의 하드웨어들을 제어한다.
데이터를 물리적으로 전달하며 물리적 연결을 담당한다.

- MAC(Media Access Control) - 물리적 주소를 통해 장치간의 통신을 관리한다.
- 이더넷 - 이더넷은 LAN에서 데이터 프레임을 전송하는 방식과 물리적 연결을 규정한다.


# 데이터의 송수신
---
## 계층별 데이터를 부르느 방법

애플리케이션의 데이터에는 다양한 프로토콜의 헤더가 추가되어 네트워크 상으로 보내진다.
데이터를 부르는 방법은 아래와 같다.
- 애플리케이션층 - 메시지
- 트랜스포트층 - 세그먼트 또는 데이터그램
- 인터넷층 - 패킷 또는 데이터 그램
- 네트워크 인터페이스층 - 프레임

## Encapsulation, Decapsulation

애플리케이션이 데이터를 주고받게 하려면 복수의 프로토콜을 조합할 필요가 있다. TCP/IP에서는 네 개의 프로토콜을 조합한다.
각 프로토콜에서 각각의 기능을 실현하기 위해 제어정보인 헤더가 필요하다.
전송시 데이터를 전송할 때 헤더를 추가하게 된다. 헤더를 바탕으로 처리하여 목적지까지 도달하게 한다.
이러한 헤더를 추가하는 처리를 캡슐화라고 한다.

데이터를 받을 때는 헤더를 벗겨내며 처리한 후 프로토콜 처리를 하며 받는데 이런 동작을 역캡슐화라고 한다.

![[CS/Network/Network.excalidraw.md#^group=wj8a5iqqeHoYxEnGgHTsp|1200]]