# URI와 웹브라우저 요청 흐름
* URI
* 웹 브라우저 요청 흐름

<br />
<hr />

## URI
URI는 Uniform Resource Identifier의 약자다.
해석하자면 리소스를 식별하는 통합된 방법이다.
리소스를 식별한다는 것은 주민번호로 사람을 식별하는 것처럼 자원을 식별한다는 뜻이다.

<br />

### URI? URL? URN?
* URL은 Uniform Resource Locator의 약자다. 즉, 리소스의 위치(locator)를 나타낸다.
* URN은 Uniform Resource Name의 약자다. 즉, 리소스의 이름(name)을 나타낸다.

URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.
URI는 URL과 URN을 포함한다.

#### URL, URN 예시
* URL(Resource Locator): foo://example.com:8042/over/there?name=ferret#nose
* URN(Resource Name): urn:example:animal:ferret:nose

URN처럼 이름을 부여한다면 찾기가 힘들다. 각 URN에 리소스의 결과가 맵핑이 되어 있는데, 중간에 다른 이름을 넣는다면 찾기가 어려울 것이다.
그래서 거의 URN은 쓰이지 않고 URL이 쓰인다.

<br />

### URI 단어 뜻
* Uniform: 리소스 식별하는 통일된 방식
* Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
* Identifier: 다른 항목과 구분하는데 필요한 정보

<br />

### URL, URN 단어 뜻
* URL - Locator: 리소스가 있는 위치를 지정
* URN - Name: 리소스에 이름을 부여
* 위치는 변할 수 있지만, 이름은 변하지 않는다.
* urn:isbn:8960777331 (어떤 책의 isbn URN)
* URN 이름민으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

<br />

### URL 전체 문법
* scheme://[userInfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko

<br />

* 프로토콜(https)
* 호스트명(www.google.com)
* 포트 번호(443)
* 패스(/search)
* 쿼리 파라미터(q=hello&hl=ko)

<br />

#### URL scheme
* 주로 프로토콜 사용
* 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
  * 예) http, https, ftp 등
* http는 80포트, https는 443 포틀르 주로 사용, 포트는 생략 가능
* https는 http에 보안 추가 (HTTp Secure)

#### URL userinfo
* URL에 사용자 정보를 포함해서 인증
* 거의 사용하지 않음

#### URL host
* 호스트명
* 도메인명 또는 IP 주소를 직접 사용 가능

#### URL port
* 포트(PORT)
* 접속 포트
* 일반적으로 생략, 생략시 http는 80, https는 443

#### URL path
* 리소스 경로(path), 계층적 구조
* 예)
  * /home/file1.jpg
  * /members
  * /members/100, /items/iphone12

#### URL query
* key=value 형태
* ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB
* query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터. 문자 형태

#### URL fragment
* https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-introducing-spring-boot
* fragment
* html 내부 북마크 등에 사용
* 서버에 전송하는 정보 아님

<br />
<hr />

## 웹 브라우저 요청 흐름
https://www.google.com:443/search?q=hello&hl=ko 로 요청을 하게 된다면,

1. DNS 서버를 조회해서 IP 정보를 찾아낸다. (www.google.com -> IP: 200.200.200.2, PORT는 명시하지 않으면 https는 기본적으로 443)
2. 웹 브라우저가 HTTP 요청 메시지를 생성한다.
    ```
    GET /search?q=hello?hl=ko HTTP/1.1
    Host: www.google.com
    ```
    ㄴ HTTP 요청 메시지
3. SOCKET 라이브러리를 통해 TCP/IP 계층으로 HTTP 요청 메시지를 전달한다.
   - A: TCP/IP 연결(IP, PORT) -> syn, syn ack, ack
   - B: 데이터 전달
4. TCP/IP 패킷 생성, HTTP 메시지 포함
    ```
    출발지 IP, PORT
    목적지 IP, PORT
    ...
    GET /search?q=hello?hl=ko HTTP/1.1
    Host: www.google.com
    ```
    ㄴ 패킷 생성
5. 이 패킷을 인터넷 망으로 던진다.
6. 구글 서버는 요청 패킷을 받으면 TCP/IP 패킷을 까서 버린다.
7. 그 다음 HTTP 메시지를 해석한다.
8. 해석이 끝난 구글 서버는 HTTP 응답 메시지를 생성한다.
    ```
    HTTP/1.1 200 OK
    Content-Type: text/html;charset=UTF-8
    Content-Length: 3423

    <html>
      <body>...</body>
    </html>
    ```
    ㄴ HTTP 응답 메시지
9. 응답 메시지를 가지고 TCP/IP 패킷을 생성한다.
10. 이 패킷을 인터넷 망으로 던진다.
11. 웹 브라우저가 HTTP 응답 메세지를 해석한다.
12. 웹 브라우저가 HTML을 렌더링 한다.

<br />
<hr />