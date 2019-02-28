# Chapter4. Connection Management

HTTP 특징으로는 HTTP는 제법 편하게 설명이 잘 되어있다. 하지만 HTTP conenction에 대해서는 자세히 언급하지 않는다. 참 웃기는일이다. 비 연결형 통신인 HTTP는 L4의 연결형 통신 TCP를 기반으로 데이터손실 없는 서비스를 제공하고 있으니 말이다. 이 챕터는 이 부분에 대해서 자세히 diving할 것이다.

#### 전제

여기에서 말하는 HTTP connection에 관련된 성능이 후진 이슈에 대해 언급한것은 최근의 HTTP application이 가지고 있던 성능문제가 아닌, 이 전의 것들에 대해서 언급한것이다. 물론 최근의 것들도 부분 그럴 수 있지만, 전제가 최신의 HTTP Application에 대한 내용이 아니런것만 알아두자.

#### 알아볼 것

- HTTP는 어떻게 TCP connection을 사용할까?
- TCP 커넥션의 지연(Delays), 병목(Bottlenecks) 그리고 (끊킴)clogs
- Parallels(병렬), Keep-Alive(연결 유지), Pipelined connection이 있는 HTTP의 최적화 작업
- Dos(해야 하는 것들) and don'ts(하면 안되는 것들) for managing connection

# HTTP Connection Handling

### The Oft-Misunderstood Connection Header

### Serial Transaction Delays

# Parallel Connections

### Parallel Connections May Make Pages Load Faster

### Parallel Connections Are Not Always Faster

### Parallel Connections May "Feel" Faster

# Persistent Connections

### Persistent Versus Parallel Connection

### HTTP/1.0+ Keep Alive Connection

### Keep-Alive Operation

### Keep-Alive Options

### Keep-Alive Connection Restrictions and Rules

### Keep-Alive and Dumb Proxies

#### The Connection header and blind relays

#### Proxies and hop-by-hop headers

### The Proxy-Connection Hack

### HTTP/1.1 Persistent Connections

### Persistent Connection Restrictions and Rules

# Pipelined Connections

# The Mysteries of Connection Close

### "At Will" Disconnection

### Content-Length and Truncation

### Connection Close Tolerance, Retries, and Idempoency

### Graceful Connection Close

#### Full and half closes

#### TCP close and reset errors

#### Graceful Close