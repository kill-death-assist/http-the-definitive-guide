Chapter3.HTTP Messages

이 챕터는 HTTP Messages에 대해서 이야기하며, 어떻게 이것들이 만들어지고 어떻게 이해하면 되는지에 대해 이야기한다.

# 메세지의 흐름

HTTP Message는 HTTP Aplication의 메세지의 한 블럭이다. 이 메세지의 플로우는 서버와 클라이언트 그리고 프록시가 존재하며 이것을 `inbound`, `outbound` // `upstream`, `downstream`라고 부른다.


### 메세지는 origin server로 inbound한다.

HTTP Message플로우 에서는 `inbound`와 `outbound`를 사용한다. 그리고 이 둘의 용어는 작업의 방향에 따라 용어가 정의된다. 메세지가 만약 오리진 서버로 `inbound`라고 표현한다면 클라이언트가 서버로 부터 요청을 보내는 data의 flow이며 오리진 서버로 부터 `outbound`라고 표현한다면 오리진 서버로 부터 응답값을 받았다는 뜻이다.

### 메세지의 흐름은 downstream

HTTP message의 플로우는 강과도 같다. 이 때 HTTP message의 모든 메세지의 플로우는 `downstream`이다. 요청메세지이든 응답메세지이든 상관없이 HTTP의 메세지의 모든 플로우는 downstream이다.

![downstream message](./docs/3-2.downstream.message.jpg)

하지만 이것은 direction이다. 방향에따라서 upstream이라 볼 수있는데, 이는 방향적 측면으로 따졌을때에 upstream이라는 것으로, 우선은 HTTP의 모든 메세지는 downsteream방식이라는것을 알아두어야 한다.

그림에서 Request 일때에 Proxy1은 Proxy3으로 upstream하고있다. 하지만, Response일때에 Proxy1은 Proxy3에 대해 downstream하고 있다.

즉 HTTP의 메세지 측면에서 Request, Response가 아닌 데이터 노드간에 한번의 전송상황에서 Receiver쪽으로는 downstream, Sennder는 upstream이 된다.

# 메세지의 부분들

메세지는 start line, header, body으로 구성되어있다.

\[start line\](필수) : GET moducampus.com HTTP/1.1
\[header\](필수)     : Content-Type: application/json
\[body\](필요)       : { "Hello": "world" }

start line과 header는 ASCII text로 이루어져 있어야 하고, 라인 단위로 줄 바뀜으로 구분이 된다.

이 때, 중요한 점이 있는데 라인 단위로 줄 바뀜이 되어있어야 하는데, 이 줄 바뀜은 운영체제 마다 "CRLF", "LF"로 표현될 수 있는데 HTTP message에서는 이에 대해 명확하게 정의가 내려져있다.

> CRLF를 사용해야만 한다.

### 메세지의 문법

Request의 메세지는 서버로 보낼 메세지이고, Response 메세지는 클라이언트에게 보낼 메세지이다.
컴퓨터 관점에서보면 Request는 서버에 명령하기위해 전송하며, 명확하게 전달하기위해 Meta정보들을 함께 전달한다. Response는 서버가 클라이언트에게 어떠한 동작을 명령 및 반환 하기위해 전송하며, 좀 더 명확하게 전달하기위해 Meta정보들도 함께 전달한다. 즉 Request와 Response는 전달 해줄 대상의 노드에게 명령하기위해 정보를 전달한다.

![req, res message format](./docs/3-4.req.res.format.png)

- method: 어떤 명령을 실행시킬건지 ex) GET, POST, PUT
- request-URL: 완벽한 URL의 풀네임
- version: HTTP 프로토콜의 버전 HTTP/{major}.{minor}
- status-code: 숫자로 이루어진 응답값 성공 실패 등 서버의 다양한 처리결과를 응답코드로 반환한다.
- reason-phrase: protocol, status를 포함한 응답이 어떻게 처리되었는지 대략적으로 보이기 위한 부분
> ex> "HTTP/1.0 200 NOT OK" or "HTTP/1.0 200 OK"
- headers: request, response의 데이터에 대한 메타데이터. 서버는 클라이언트인 브라우저에게 명령을 할 수 있고, 서버는 클라이언트로 받은 요청에 대한 상세한 정보를 입력받기 위해 존재한다. 위에서 언급했지만, `start line`과 `header`의 줄의 마지막은 CRLF로 마무리 되어있어야 한다.
- entity-body: 메타정보가 아닌 유효한 실정보를 실어 나르는 곳. 응답body가 있을 수 도 있으며, 없을 수 도 있다. CRLF가 있던 없던, body를 해석하는것은 프로토콜이 아닌 응용프로그램들이다. (클라이언트의 역할을 할 browser, 직접짠 client ,server source code)


### Start Lines
- request line: {method} {URL without domain and port}:{port} HTTP/{version}
- response line: HTTP/{version} {status code} {response-phrase}
- methods: request start line에 있으며 어떤 역할을 수행할지 명시하는 부분
- status codes: response start line에 있으며, 요청의 결과를 숫자값으로 표현한 부분
- reason phrase: response start line에 있으며, 요청의 결과를 문자열로 표현한 부분
- version numbers: HTTP에도 버전이 있는데, 이에대한 명시이다. 요청시에 보내면, 이 프로토콜로 대화할것이다 라는것을 알려주는 역할을 하게 된다.

> TIP: 2.3 하고 2.23 둘중에 어떤 버전이 더 높을까? 너무나도 당연하게 2.23이 더 높다.

2.3이나 2.23은 유리수 따위가 아니라 문자열이다. 그리고 그 문자열의 순서만 보는게아니라 버저닝 규칙이 존재하는데 그 버저닝 규칙을 모른다면 당연히.. 모를 수 밖에 없을것이다. 
`.` 을 기준으로 숫자 값을 나누고 앞 자리부터 비교를 한다. 2는 같은 숫자이기 때문에 3하고 23 둘 중에 큰것 은 당연 23 이다 그렇기 때문에 버전 2.3하고 버전 2.23중에 높은버전은 2.23이다.

### Headers
response, request의 추가적인 정보를 의미한다. 기본적으로 `name: value` pair로 작성되어 있다.

### Entity Bodies

request, response의 실제 payload값 이다. 일반 문자열이 들어올 수 도 있지만, 2진파일의 이미지, 영상, 소프트웨어 어플리케이션 등 이 들어갈 수 있다.

# Methods

### Safe Methods
GET, HEAD가 safe메서드 이다. should NEVER change the resource. However, this is not completely true. It means: it won't change the resource representation. It is still possible, that safe methods do change things on a server or resource, but this should not reflect in a different representation.

### GET
리소스를 불러올 때 사용하며 메서드의 구분이 없던 0.9에서 부터 사영되어 왔다.

### HEAD
HEAD는 GET하고의 역할이 완벽히 똑같다. 그러나 메서드 이름이 HEAD라고 해서 Response의 응답값이 header까지만 만들어져서 나오는것은 아니다.
주로 해당 URL에 자원이 존재 하는지 하지 않는지 알아보기위해 사용한다.

### PUT
PUT은 자원을 추가 핟거나 자원의 내용을 바꿀때에도 쓸 수 있다. 주로 RESTful설계에서는 수정할때에 전체적인 내용을 수정할때나, 아예 새로 추가할 때 사용하기도 한다.

### POST
이는 input값을 전송 받을때를 위해 설계되었다. 주로 HTML의 form tag에서 입력 받은것들을 받게끔 설계되었다.

### TRACE
클라이언트가 요청값을 만들때, 방화벽을 타고 프록시를 거쳐서 게이트웨이 등등등 여러 어플리케이션 노드들을 거쳐가게 된다. 이 때 TRACE메서드는 요청값을 수정할 수 있는 기회를 주게끔 설계되었다.

이는 loopback 진단을 통해서 goal remote host까지 도달하는데에 있어서 leg은 없는지 마저도 진단할 수 있다.

이 메서드를 사용해서 request, response의 한 트랜젝션을 수행하였다면 받은 response를 이용하여 chaning으로 요청을 다시 보낼 수 있다.

### OPTIONS
OPTION메서드는 서버에게 어떤 것들이 허용되는지를 묻는다.
이 제공의 뜻은, 클라이언트에 의해 서버를 보호하기위해 사용되기도 하며 인가된 사용자를 판단하기위해 사용하기도 한다. 이 문제는 CORS보안 문제와 직접적으로 연관이 있는 메서드이기 때문에 자세히 알아두는것이 좋을것이다.

### DELETE
말 그대로 자원의 삭제를 의미한다. 말 그대로 해당 URL에 존재하던 자원을 지우는것인데, 말 처럼 완벽하게 데이터를 지우지 않을 수 도 있다. 그것은 어플리케이션 설계자 및 소스코드 작성자에 따라 다르다.

### Extenstion Methods

LOCK, MKCOL, COPY, MOVE등이 존재하지만, PATCH정도 알고있으면 좋다. 해당 자원의 속성값 하나를 변경하거나 할 때 사용된다. PUT과 PATCH가 사용시기가 겹치는데 PUT은 전체적인 데이터를 수정할 때 라면, PATCHS는 보이기 안보이기 같은 status 값 변경이라 보면 된다.

# Status Code

### 1xx
요청값을 잘 받았고 이해했다. 의 의미를 갖고 있다. 이는, 계속해서 해당 프로세스를 진행해도 좋다는 의미이다. 대부분 step으로 나뉘어진 설문 또는 방대한 양의 데이터 일 때에 사용되지만.. 아직 한번도 본 적이 없다... HTTP/1.1때 부터 생긴듯 하다.

#### 클라이언트와 100코드
클라이언트 입장에서의 1xx코드를 받기 전 까지 다른 요청을 보낼경우에는 alert나 에러가 발생하게 되는데 이때에는 Expect Request Header를 이용하여 해결할 수 있다.

#### 서버와 100코드
만약 서버가 클라이언트로 부터 Expect Request Header를 받았으면, 100코드를 던지거나 에러코드를 던져야 한다. 서버코드는 절대 100 status code를 클라이언트에게 보내지 않는다. 100코드는 엄밀히 따지면 expectation이다.

만약 어떤 이유에서건 서버가 클라이언트에게 100을 던질일이 발생하게 된다면,  직접적으로 status code를 던지지 않아도 된다. 그냥 서버는 존나 가만히 있어도 클라이언트가 알아서 지속할지 말지를 결정할것이기 때문이다.

### 2xx

성공적인 요청이었고, 서버 또한 성공적으로 처리해서 요청한 것에 대한 응답값을 내려주었을 때 사용한다.

### 3xx

해당 작업은 성공이긴 하지만, 이 곳이 아니라는것을 알려주는 것이다.
다른곳으로 redirect 시켜버리거나, 저장되어있던 자원이나 사용하라는 의미이다.

### 4xx

사용자의 요청값이 올바르지 않다고 알리기위함이다.

### 5xx

프록시, 게이트웨이, 터널링 등 어쨋든 클라이언트가 아닌 서버영역에서 문제가 있다고 알리기 위해 존재한다.

# Headers

헤더는 클라이언트나 서버가 어떻게 하라는 명령을 담는다. 물론 body는 payload를 담는다.

- **General Headers:** 일반적으로 클라
나 서버 둘 다 사용되고있으며, 일반적인 목적으로 유용하게 사용된다.
- **Request Headers:** Request시에 특별하게 사용할 수 있는 요청용 헤더값들이다.
- **Response Headers:** Response시에만 특별하게 사용할 수 있는 응답용 헤더값들이다.
- **Entity Headers:** 해당 body에 들어가있는 자원들을 어떻게 다루어 주었으면 하는지에 대해 명시되어있는 헤더값들이다.
- **Extension Headers:** 정식으로 사용되는 것은 아니다. 개발자들이 직접 만들어서 사용할 수 있다.

### General Headers
일반적으로 요청 및 응답에 대한 메세지들에 대한 헤더이다.

- **Connection:** client와 server에서 연결에 대한 특별한 옵션이며 TCP연결과 밀접하게 연관이 있다.
- **Date:** 응답 및 요청이 만들어진 시간에 대해 작성되어있다.
- **MIME-Version:** 요청자가 사용할 MIME버전에 대해 서버에게 주는 것
- **Trailer:** 인코딩된 메세지에 대한 header 리스트 셋이다.
- **Transfer-Encoding:** 수신자에게 이것이 무엇으로 인코딩 되었는지 알려주며 인코딩에 대해 안전하게 송신될 수 있도록 하는 기능이다.
- **Upgrade:** 요청자의 HTTP프로토콜 버전이 낮거나 서버가 새로운 버전으로 통신하자고 알려주는 헤더이다.
- **Via:** (Proxy, Gateway) 어디를 거쳐서 메시지가 도착했는지 요청자에게 알려주는 header이다.

#### General caching headers

HTTP/1.0 부터 소개된 헤더는 서버가 클라이언트에게 해당 객체를 캐싱하라고 알리는 헤더들이다.
항상 서버로부터 오브젝트를 받아서 렌더링을 fetching하는 대신에, 이전에 서버로 부터 받은 동일한 데이터를 다시 한번 서버로 요청해야할 때, 클라이언트의 로컬에 저장한 이전 데이터를 다시 불러와서 빠르게 사용하는 것을 말한다.

- **Cache-Control:** 메세지를 보낼 때, 이 object는 어떻게 cache를 하라고 명령하는 값이다.
- **Pragma:** HTTP/1.1 이전에 Cache-Control 대용으로 사용하였다. 하지만 HTTP/1.1이 일반화되어 사용되는 지금 잘 사용되지는 않는다. HTTP/1.0 유저를 위해 사용되기는 함

### Request Headers
닉값대로 요청에 관련된 헤더들. 언제, 어디서, 누가, 무엇을 주었는지 서버에게 알리기 위함이다. 서버는 이정보를 이용하여 클라이언트에게 좀 더 정확한 정보를 준다.

- **Client-IP:** IP address of client machine
- **From:** E-Mail address of client user
- **Host:** 요청받을 remote host(서버)의 hostname(domain)과 port를 기술해놓은 곳
- **Referer:** 어디서부터 해당 요청이 왔는지 알기위해 사용하는 URI, Google Analytics같은것들이 주로 이 헤더를 기반으로 웹 서비스의 분석시에 자주 사용된다.
- **UA-*:** 알아보기 귀찮기도하고 너무 닉값 대로다. 몰라도 된다. RFC2616 공식도 아니다.
- **User-Agent:** UA-* 이 따위것들보다 주로 User-Agent를 많이 사용한다. 이는 client machine에 대한 정보를 담고 있다. 물론 UA-*가 더 자세하고 디테일하지만, User-Agent만 사용해도 충분하다.

#### Accept headers
클라이언트가 알아먹는데 어떻게 보내주는게 이해하기 좋은지 알리기 위함

> "내 수듄은 요래"

- **Accept:** MIME타입으로 표현되는 클라이언트가 이해 가능한 컨텐츠 타입이 무엇인지 알리기 위함
- **Accept-Charset:** 어떤 문자열 charset이 클라이언트가 이해하기 좋은지 알리기 위함
- **Accept-Encoding:** 어떤 문자열 encoding이 클라이언트가 이해하기 좋은지 알리기 위함
- **Accept-Language:** 어떤 문자열 언어가 클라이언트가 이해하기 좋은지 알리기 위함. 
- **TE:** 특별히 추가로 사용할 수 있는 인코딩이 무엇이 있는지 알리기 위함이다. (난 본적도 없다. 이거대신에 오히려 [Transfer-Encoding](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding) 이런것을 보는게 더 좋을것이다.)(you could informally call it Accept-Transfer-Encoding, which would be more intuitive).

#### Conditional request headers
때때로 클라이언트는 서버에게 제한사항을 request message에 담기도 한다. 예를들어 만약 클라이언트가 이미 해당문서를 copy(caching)을 했다고 했을때, 아마도 서버에게 "같은거면 보내지 말고 다른거면 보내줘" 따위의 것들을 요청할 수 도 있다. 이럴 때 사용한다 Conditional request header는.

- **Expect:** 클라이언트가 서버에게 기대하는 행동들을 기입하는 곳이다. `ex) Expect: 100-continue`
- **If-Match:** ETag와 함께 사용하는게 일반적, GET and HEAD메서드로 요청을 했을때, 서버는 ETag에 있는 조건값들을 보고 일치하는게 있으면 반환한다. 또는 PUT일 때에는 업로드할 때에 만 적용된다. 자세한것은 이 문서를 읽어보자 [If-Match](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Match)
- **If-None-Match:** 서버 리소스의 ETag가 이 헤더에 나열된 것들과 일치하는 것이 없다면 성공, 기본적으로, etag에 'W/'가 접두사로 붙지 않았다면, strict한 검사가 이루어지게 된다.
- **If-Modified-Since:** 서버 리소스의 Last-Modified 날짜가 이 헤더 내에 주어진 것보다 좀 더 최근인 경우 응답해준다.
- **If-Unmodified-Since:** 서버 리소스의 Last-Modified 날짜가 이 헤더에 주어진 것 보다 더 오래되었다면 응답해준다.
- **If-Range:** 범위요청이라 한다. If-Match 혹은 If-Unmodified-Since와 유사함. 하나의 단일 etag 혹은 하나의 날짜만을 가질 수 있다. 만약 실패한다면, 범위 요청이 실패하고, 206 Partial Content 응답 대신에, 200 OK가 완전한 리소스와 함께 전송된다.
- **Range:** HTTP 요청 헤더는 서버에게 문서의 일부분만 돌려주어야 한다는 것을 알려준다.
`[성공: 206 Partial Content]` // `[실패: 416 Range Not Satisfiable]`..
`ex) Range: bytes=200-1000, 2000-6576, 19000-`

#### Request security headers
HTTP나름 보안에 대한 도전, 사용자 세션과 인증에 대한 헤더 부분이다.

- **Authorization:** 요청데이터가 서버로 부터 인증받은 것임을 알리는 부분이다. 더 정확히는 서버의 사용자 에이전트임을 증명하는 자격을 포함한다. 그리고 보통 서버에서 401 Unauthorized 상태를 WWW-Authenticate 헤더로 알려준 이후에 나온다. `ex) Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l`
- **Cookie:** 클라이언트에서 서버로 token을 전송할 때 주로 사용된다. Cookie에 대해서는 많은 기능들이 있다. 해당파트에서 자세히 보기보다는 다른 챕터에서 자주 언급될 예정일 것이다. (SPA Web 에서는 매우 중요..)
- **Cookie2:** 더 이상 안씀. 가끔 레거시에서 쓰였었는데, 필요할 때 알아서 찾아보자. 이건 Bad Practice다.


#### Proxy request headers
프록시에 관련된 헤더, 프록싱에 대해 도움을 준다.

- **Max-Forwards:** 이것은 HTTP Method TRACE일 때에 함께 쓰인다.
프록시 수를 제한하거나, 다른 중개자들을 통해 요청할 것때에 쓰인다. 이것의 값은 숫자값이며 거칠때마다 값이 감소된다.
- **Proxy-Authoriozation:** Authentication과 같다. 하지만 이것은 Proxy일 때에 쓰인다. 요청헤더에 user-agent에 대한 credential 한 인증을 포함한다.
- **Proxy-Connection:** Connection 하고 같다. 하지만 이것은 Proxy일 때에 쓰인다

### Response Headers
닉값대로 응답메세지에 포함할 때 쓰인다.

- **Age:** 이 응답값의 나이, 0이라면 서버로부터 방금 막 받은 것이다. 그게 아니라면 프록시의 현재 시간과 HTTP 응답 내에 포함된 Date 일반 헤더의 차로 계산한다 {delta-seconds}.
- **Public:** 이전 RFC2068에서 정의 됬었지만, RFC2616(HTTP/1.1) 에서 는 언급되진 않았다. 그리고 실제로 잘 사용되지 않고있다. server가 지원하는 METHOD에 대한 리스트 셋을 클라이언트에게 알려준다.
- **Retry-After:** 만약 요청이 올바르지 않았을때, 최소 지정한 시간이후에 요청하라는 알림 헤더이다.
- **Server:** 헤더는 요청을 처리하기 위한 origin server의 정보를 포함하고있다. 굳이 이 헤더를 응답값에 포함하지 않는데, 보안적 문제로 공격자에게 좋은 단서를 줄 수 있는 이유가 되기 때문이다.
- **Title:** 응답한 html태그의 제목을 알려줍니다. 잘 안씁니다. title태그에 기술하기때문에..
- **Warning:** 상세한 에러에 대한 내용을 기술한다.

#### Negotiation headers


- **Accept-Ranges:** 서버가 받을 수 있는 타입들의 범위에 대한 내용을 기술한다.
- **Vary:** Vay header는 HTTP response header이며, 나중의 request header에서 캐쉬된 response를 사용할지 아니면 origin server로 부터 새로 받을지에 대해 결정하데 쓰이는 헤더입니다.
이것은 종종 서버에 서버에 의해 사용되었으며 헤더들이 보여줄 리소스를 선택하는 데에 사용되고있는 것을 보입니다. 이는 content negotiaion algorithm에 의해 리소스를 선택합니다.
만약 Vary헤더가 304 Not Modified응답값을 받았다면 equivalent 200 OK 하고 같은 이야기입니다. [HTTP 캐싱 - **상황에 따른 응답](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching#%EC%83%81%ED%99%A9%EC%97%90_%EB%94%B0%EB%A5%B8_%EC%9D%91%EB%8B%B5)

#### Response security headers
아마 이전에 request security header에서 한번쯤 보았을 헤더들이다. 기본적으로 response응답값과 함께 사용된다.

- **Proxy-Authentitcate:** 인증된 proxy의 클라이언트들이 응답헤더를 받으며, 받을 때에는 `Proxy-Authorization: {type} {credentials}` 형태로 받게된다. 
- **Set-Cookie:** cookie에 값을 설정할 때 사용된다. 엄청 중요하다 CORS하고 아주 아주 아주 연관이 깊다. withCredential에 따라 헤더가 되기도 하고 안되기도하고 아주 아주 아주 중요하다.
- **Set-Cookie2:** Set-Cookie와 같다. Cookie2에 사용된다. 몰라도 된다. 넘어가자.
- **WWW-Authenticate:** 401과 함께 쓰인다. 리소스에 대한 접근 권한을 얻기위한 인증 method들을 정의해서 내려준다.

### Entity Headers
HTTP message들에 대한 request, response에 대한 payload를 설명하는 헤더들은 많이 존재한다. 

- **Allow:** 내려준 entity(payload)에서, 요청 가능한 http method에 대한 목록들을 기술한 곳
- **Location:** 클라이언트에게 해당 resource가 어디있는지 알려주는 헤더이다. (302 Redirection과 함께 자주 사용된다.)

#### Content headers
content header는 entity(payload)에 대한 특별한 정보들을 포함한다. 사이즈라던지, 타입이라던지, 인코딩이라던지 등

- **Content-Base:** 해당 리소스가 어디있는지 알려주는 헤더 ex) http://m.me/hello.png 라면 Content-Base는 http://m.me/
- **Content-Encoding:** body를 읽기위한 어떠한 encoding에 대한 기술
- **Content-Language:** body가 문자열이고, 이 언어는 어떤 자연어(Kr, Jp, En, ...등)로 읽어야 하는지?
- **Content-Length:** body의 (8 bit, octets)byte수가 얼마인지?
- **Content-Location:** 해당 리소스의 경로 ex) Content-Location: /index.html
- **Content-MD5:** 이 body는 MD5 checksum이라는것을 알려주기 위함
- **Content-Range:** range of bytes that this entity represents from the entity resource
- **Content-Type:** body의 type of entity(payload)

#### Entitiy caching headers
general caching headers는 directives들을 제공한다. 어떻게 언제까지 캐싱을 할것인지 등. 

- **ETag(중요):** HTTP 응답 헤더는 특정 버전의 리소스를 식별하는 식별자입니다. 웹 서버가 내용을 확인하고 변하지 않았으면, 웹 서버로 full 요청을 보내지 않기 때문에, 캐쉬가 더 효율적이게 되고, 대역폭도 아낄 수 있습니다. 허나, 만약 내용이 변경되었다면, "mid-air collions" 이라는 리소스 간의 동시 다발적 수정 및 덮어쓰기 현상을 막는데 유용하게 사용됩니다.
- **Expires:** 언제까지 해당 entity를 캐싱할것인지에 대해 명시한다.
- **Last-Modified:** entity가 수정된 last date를 의미한다.