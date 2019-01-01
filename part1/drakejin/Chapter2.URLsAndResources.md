Chapter2. URLs and Resources

URLs은 인터넷의 리소스들에 대한 일반화된 이름이다. URLs의 목적은 전자정보의 부분을 가르키며, 사용자에게 이 정보가 어디에 존재하고 있는지를 알려주는 경로이다.

이 챕터에서 배울내용은
- URL에 대한 문법
- URL을 구성하는 요소들에 대한 뜻과 하는 역할
- URL Shortcuts 그리고 웹 클라이언트가 지원하는 부분
- 여러 정보시스템에서 사용되는 공통적으로 사용되는 URL Schema
- 미래의 URL들 그리고 URN

# 인터넷 자원 찾기
- URL은 실제로 URI의 부분집합으로 의미를 알고있다. URI는 일반적으로 부분집합으로는 URL, URN이 두개가 있는걸로 알려져 있다. URL은 어느 경로에 리소스가 위치되어있는지를 설명해놓은 경
로명이고, 반면 URN은 이름으로써 리소스를 식별한다. 하지만 최근 이 둘을 딱히 명확히 구분해놓고 사용하지는 않는다.

# URL의 문법

URL스키마의 베이스는 이 9개의 부분으로 이루어져있다.

> `<scheme>`://`<user>`:`<password>`@`<host>`:`<port>`/`<path>`;`<params>`?`<query>`#`<fragment>`

- scheme: 어떤 프로토콜을 사용할것인지에 대해
- user: 사용자 아이디
- password: 사용자의 비밀번호
- host: IP 주소 및 Domain Name
- port: host의 port number HTTP의 default는 80
- path: 해당 자원이 존재하는 경로를 의미한다
- params: HTTP에서는 잘 못봤겠지만, `;` 문자열을 이용하여 파라메터를 건내 줄 수 있다.
- query: HTTP Get method에서 많이 봐왔을것이다.
- frag: fragment, 이며 `#`을 통해서 많이 봐왔을 것이다 .hash라고도 하는데, 이는 해당 자원의 부분을 의미한다.

# URL Shortcuts

### Relative URLs

URL에서 사용하는 경로는 상대경로와 절대경로 두 가지를 사용할 수 있다. `relative` and `absolute` 

- 상대경로는 현재 경로를 기준으로 자원의 위치를 탐색할 수 있으며, 제약사항으로는 다른 remote host의 자원은 탐색할 수 없으며, 문법적으로도 작성할 수 있는 방법이 없다.
> ex) ../../../../test.html

- 절대경로는 URL의 주소를 full로 작성해서 자원을 찾아오는 방법을 의미한다.
> ex) https://tpc.googlesyndication.com/simgad/8948241245527508560?sqp=4sqPyQQrQikqJwhfEAEdAAC0QiABKAEwCTgDQPCTCUgAUAFYAWBfcAJ4AcUBLbKdPg&rs=AOga4qmBV9fwg9emo-_ZXxI8cT1ikD0GZw

### Base URLs

base URL은 현재 보고있는 자원까지의 url을 의미한다.

### Resolving Relative references

base URL을 기준으로 ./ ../같은것들을 사용하여 자원의 위치를 찾는다. 프로그램 내
부적으로 상대경로를 보면 이것을 절대경로로 치환해서 본다.

# Shady Characters 문자열의 어두운면, 무서운 면

URL은 외우기 쉽게 설계가 되었다. 하지만 URL에는 어두운 면이 있으니..
한글이라던지, 상형문자 등등의 다양한 문자열 인코딩에 대해서는 안전하진 않다. 
그렇기에 URL에 사용될 수 있는 URLencoding을 이용하며 호환되지 않는 문자열들을 인코딩하여 안전하게 자원을 가져오도록 한다.

### The URL Character Set

기본적으로 컴퓨터시스템에는 종종 Anglocentric bias를 가지고 있다. 역사적으로 US-ASCII코드를 사용할 때에는 매우 외우기 쉬웠으며, 오랜기간 사용되어왔다. 그러나 몇몇의 스페인어라던지, non-Romanic Language에 대해서 사용할 필요성이 생기게 되었다. 더 나아가서는 문자열이 아니라 2진 데이터 까지도 완벽하게 인식할 수 있어야 됬다. 그래서 URL designer설계자들은 정해지지않은 esacape 문자열들 까지 허용할 수 있도록 만들어야만 됬다.

### 결론

그러므로 길게 설명할 것 없이 영어제외한 문자열은 urlEncoding을 이용하여 넣도록 하자.

### 좀 더 나아가

URL은 주소이며 경로이다. 이름이 아니다. 그리고 그 자원은 어디론가 이동이 되면, 당신이 알고있는 주소는 틀리게된다. 잠깐 동안만 그 URL을 사용하게 될 뿐이다. 때문에 미래에는 URN이 사용될것이라 생각한다. 경로명이 변경되어도 자원의 이름으로 자원을 참조할 수 있게 되는 미래 말이다.
