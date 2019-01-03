# Chapter1 빠른 페이스로 읽어보는 HTTP

#### 이번 챕터에서 배워볼 내용

1. 어떻게 웹 클라와 서버가 통신하는가?
2. 어디서 웹 자원들을 가져오는가?
3. 어떻게 웹 트랜젝션(작업 통신단위)이 작동하는가?
4. HTTP메세지 포맷은 어떻게 되어있는가?
5. 다양한 HTTP 프로토콜의의 버전에 대해서

# HTTP 인터넷의 멀티미디어 전송

HTTP는 신뢰있는 데이터를 전송하며 이것이 보장되어있다. 아무리 데이터가 대미지를 입거나 순서가 뒤죽박죽이어도, 온전하게 전송되는것을 보장한다. 이는 당신이 사용자로써도 좋으며, 서비스 제공자인 측면으로도 장점으로 발휘된다. 수신받은 데이터패킷의 통합에 대해서 전혀 걱정할 필요가 없기때문이다. 때문에 서비스 제공자일 당신은 그저 당신의 어플리케이션에 대해서만 집중할 수 있다.

# Web client와 server

web client와 server는 HTTP프로토콜로 통신한다.

# Resources

### media type
- media(avi, k3g ...etc)
- image(png, jpeg, jpg, gif, ...etc)
- document(html, pdf, xlsx, doc, ...etc)
- script(css, js)

### URIs

서버의 리소스 네임 (Uniform Resource Identifier) URI

### URLs

서버의 자원 경로 (Uniform Resource Location)

### URI와 URL구별

URI는 자원 자체, URL은 자원 경로 프로그래밍상이나 대화상에서 둘의 이름은 크게 차이가 없다.

URI, URL은 http에서만 쓰는게 아니라 그저 remote host의 자원에대한 경로 및 이름이므로 프로토콜 및 포트를 포함한 이름으로 작성한다.

> {protocol}://{sub domains}.{domain or IP}:{port}/{location}#{fragment}?{query}

### URN

uniform resource name. URN은 서버에 유니크한 이름으로써 컨텐츠의 부분을 지칭한다. 그리고 독립적으로 해당 자원이 현재 어디있는지에 대해 표시한다. URN은 아직 범용적으로 차용되지 않았다.

## 트렌젝션 Transaction

HTTP의 트렌젝션은 응답과 요청으로 하나의 작업단위로 구성되어있다.

### method


| HTTP Method | Description                                 |
|-------------|---------------------------------------------|
| GET         | 이름 있는 자원을 서버에게 요청한다.         |
| PUT         | 이름있는 자원을 서버에게 저장한다           |
| DELETE      | 이름있는 자원을 서버로 부터 삭제를 요청한다 |
| POST        | 사용자의 정보를 서버로 전송한다             |
| HEAD        | 단지 HTTP header만을 전송한다.              |

### Status Code

| HTTP Status Code | Description               |
|------------------|---------------------------|
| 2xx              | 성공                       |
| 3xx              | Redirection               |
| 4xx              | Hey Client, You fxxked up |
| 5xx              | Hey we fxxcked up         |

### 웹 페이지는 여러 오브젝트들로 구성할 수 있다.

웹 어플리케이션은 단일 요청 하나로만 구성할 수 도 있지만, 여러 요청으로 다양한 Object(documents)를 이용해 구성할 수 도 있다. 예를들어 web browser issues중에 하나로 HTTP transaction은 cascade로 browser의 display가 패치되는 순서로 요청된다.

# Message

메세지는 start line, header fields, body 세 파트로 이루어진다.

| Request                                                          | -            | Response                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------|
| POST /hello/world.html HTTP/1.1                                  | [ Start Line ] | HTTP/1.1 200 OK                             |
| Accept: * Accept-Language: en, kr Content-type: application/json | [ Headers ]    | Content-type: text/plain Content-length: 19 |
| { "hi": "heloo" }                                                | [ Body ]       | yo, what's up!?                             |

# Connection

어떻게 메세지가 노드간에 옮겨져 가는지에 대해 알아볼것이다. 이 때, 노드는 클라이언트와 서버를 의미한다. HTTP메세지는 TCP(Transmission Control Protocol) 전송제어 프로토콜을 이용한다.

### TCP/IP

솔직히 HTTP는 TCP/IP의 핵심을 몰라도 어플리케이션을 개발하는데에 있어서 아무 신경 쓸 필요가 없다. 그리고 네티워크들 간의 통신 지식도 몰라도 된다. 하지만 분명한것은 우리는 TCP/IP위에서 통신을 하고 있음은 틀림없다. 때문에 우리는 이것에 대해서 좀 더 알아볼 필요가 있다. 이번장에서는 간략히 설명하고 넘어가도록 한다.

TCP프로토콜은 다음과 같은 기능들을 제공한다.

- 에러로부터 자유로운 데이터 전송
- 순서가 지켜진 데이터 전송
- Un-Chunked(Unsegmented) data stream
- 한번 커넥션이 맺어지면 유지된다.
- 데이터전송간의 loss가 없다.

### Connection, IP Address, and Port Numbers

HTTP 클라이언트가 전송할 수 있기 전에 서버에게 먼저 메세지를 보낸다.

> "나 메세지 보내도 되겠니?"

HTTP는 TCP커넥션을 이용하기 때문에 전송전에 이 둘이 연결이 되어있어져야 한다. 그렇기 때문에 먼저 서버에게 연결을 해도 되겠는지 물어보는것이다. 이 때, TCP연결을 맺기위해 클라이언트는 서버의 IP Address와 Port Number를 알아야 하는 수 밖에 없다.

종종 우리는 naver.com같은 주소를 볼 수 있을 것이다. 이를 Domain Name이라 하며 Domain Name은 IP주소로 치환받을 수 있다. 이를 가능케하는게 Domain Name Service DNS이다. 기억하기 좋거나 입에 착착 달라붙는 Domain Name을 사용한것들을 IP주소로 반환해서 뱉어주는 Service이다.

즉 naver.com:80은 DNS에 의해 IP로 변형이되고 미리 알고있던 port 80으로 접속할 수 있게 된다.

## Prodocol Versions

- HTTP/0.9: GET메서드밖에 없으며 MIME type을 지원하지 않음
- HTTP/1.0: 이 때 version number가 추가되었으며, 몇개의 메서드도 넣어졌으다. 이번 파트의 가장 마지막에서 자세히 설명하겠지만, Connection에 대한 요건들이 추가되며 변경되기도 하였다.
`Connection: Keep-Alive`Connection 이라던지 Virtual Host(가상 호스트)라던지, 가상호스트라던지..
- HTTP/1.1: HTTP에 대한 정확하고 효율적인 플로우를 가진 아키텍쳐에 대해 집중하게 되었고, 성능개선 및 어울리지 않는 스펙(기능)들에 대해 제고가 되었다. 이게 현재쓰이고 있는 버전이다.
- HTTP-NG(a.k.a HTTP/2.0): 이전버전 대비 인증성능 및 강력한 서버로직을 가져갈 수 있게 프레임워크들을 제공한다.

## 웹을 이루는 아키텍쳐 컴포넌트들

어플리케이션은 메세지를 보내고 그 하나의 메세지를 보내기위해 여러개의 호스트들이 통신을 한다. 이 호스트들이 HTTP프로토콜로 송수신을 할 때에, 아키텍쳐 구성요소들에 대해서 알아보도록 한다.

### Proxy

클라이언트와 서버간의 중개역할을 한다.

### Caches

서버로 부터 받은 데이터를 어느곳으로 부터 임시로 저장해서 꺼내 서버의 처리를 줄여주는 역할을한다.

### Gateways

게이트웨이는 특별한 서버이며 이는 다른 서버들간의 중개역할을 한다. 종종 HTTP traffic을 다른 프로토콜로 변경하는 역할로도 쓰이기도 한다. 예를들면 ftp프로토콜의 서버로부터 데이터를 송수신 할 때 쓰이기도 하고, 인증의 역할로도 쓰이기도 한다. 아키텍쳐의 노드관점에서 게이트웨이를 생각하면 좀 더 편할것이다.

### Tunnels

안보이는 두 Raw데이터 그리고 두 Connection을 선택해서 우회할 수 있게 한다. 주로 HTTP의 Tunneling의 예는 SSL(Secure Sockets Layers)이 있다.

### Agents

웹 브라우저를 말한다. 더 정확하게는 클라이언트가 어떤 것인지를 의미한다.
예를 들면 크롤러는 spiders, web robots라는 이름을 달고 크롤링을 한다.