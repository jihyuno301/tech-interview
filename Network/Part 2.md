# 2. Network
**:book: Contents**
* [HTTP와 HTTPS](#http와-https)
* [HTTP 요청/응답 헤더](#http-요청-응답-헤더)
* [CORS란](#cors란)
* [GET 메서드와 POST 메서드](#get-메서드와-post-메서드)
* [쿠키(Cookie)와 세션(Session)](#쿠키와-세션)


---

### HTTP와 HTTPS
<img src="./img/HTTP_HTTPS.jpg" width="70%" height="70%">

- HTTP 프로토콜
    - 개념 
        - HyperText Transfer Protocol(하이퍼텍스트 전송 프로토콜)
        - 웹 상에서 클라이언트와 서버 간에 요청/응답(request/response)으로 정보를 주고 받을 수 있는 프로토콜
        - 서로 다른 시스템들 사이에서 통신을 주고받게 해주는 가장 기초적인 프로토콜
        - ISO 모형의 응용계층(L7), TCP/IP 의 응용계층(L4) 이다.
    - 특징 
        - 주로 HTML 문서를 주고받는 데에 쓰인다. 
        - TCP와 UDP를 사용하며, **80번 포트**를 사용한다.
        - 1) 비연결(Connectionless)
            - 클라이언트가 요청을 서버에 보내고 서버가 적절한 응답을 클라이언트에 보내면 바로 연결이 끊긴다.
        - 2) 무상태(Stateless)
            - 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않는다.
            
- HTTPS 프로토콜 
    - 개념 
        - HyperText Transfer Protocol over Secure Socket Layer
            - 또는 HTTP over TLS, HTTP over SSL, HTTP Secure
        - 웹 통신 프로토콜인 HTTP의 보안이 강화된 버전의 프로토콜
    - 특징 
        - SSL은 서버와 브라우저 사이에 안전하게 암호화된 연결을 만들 수 있게 도와주고, 서버 브라우저가 민감한 정보를 주고받을 때 이것이 도난당하는 것을 막아준다.
        - HTTPS의 기본 TCP/IP 포트로 **443번 포트**를 사용한다.
        - HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, 웹 상에서 정보를 암호화하는 SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다. 
            - TLS(Transport Layer Security) 프로토콜은 SSL(Secure Socket Layer) 프로토콜에서 발전한 것이다.
            - SSL은 넷스케이프가 개발한 프로토콜인 반면 TLS는 IETF 표준이다.
            - 두 프로토콜의 주요 목표는 기밀성(사생활 보호), 데이터 무결성, ID 및 디지털 인증서를 사용한 인증을 제공하는 것이다.
        - 따라서 데이터의 적절한 보호를 보장한다. 
            - 보호의 수준은 웹 브라우저에서의 구현 정확도와 서버 소프트웨어, 지원하는 암호화 알고리즘에 달려있다.
        - 금융 정보나 메일 등 중요한 정보를 주고받는 것은 HTTPS를, 아무나 봐도 상관 없는 페이지는 HTTP를 사용한다.
- HTTPS가 필요한 이유?
    - 클라이언트인 웹브라우저가 서버에 HTTP를 통해 웹 페이지나 이미지 정보를 요청하면 서버는 이 요청에 응답하여 요구하는 정보를 제공하게 된다.
    - 웹 페이지(HTML)는 텍스트이고, HTTP를 통해 이런 텍스트 정보를 교환하는 것이다.
    - 이때 주고받는 텍스트 정보에 주민등록번호나 비밀번호와 같이 민감한 정보가 포함된 상태에서 네트워크 상에서 중간에 제3자가 정보를 가로챈다면 보안상 큰 문제가 발생한다.
    - 즉, 중간에서 정보를 볼 수 없도록 주고받는 정보를 암호화하는 방법인 HTTPS를 사용하는 것이다.
- HTTPS의 원리 
    - 공개키 알고리즘 방식
    - 암호화, 복호화시킬 수 있는 서로 다른 키(공개키, 개인키)를 이용한 암호화 방법 
        - 공개키: 모두에게 공개. 공캐키 저장소에 등록 
        - 개인키(비공개키): 개인에게만 공개. 클라이언트-서버 구조에서는 서버가 가지고 있는 비공개키 
    - 클라이언트 -> 서버 
        - 사용자의 데이터를 **공개키로 암호화** (공개키를 얻은 인증된 사용자)
        - 서버로 전송 (데이터를 가로채도 개인키가 없으므로 **복호화할 수 없음**)
        - 서버의 **개인키를 통해 복호화**하여 요청 처리 
- HTTPS의 장단점
    - 장점 
        - 네트워크 상에서 열람, 수정이 불가능하므로 안전하다.
        - 사용자들이 안전하다고 생각하여 많이 방문되어, 검색엔진 최적화(SEO)에서 혜택을 받는다.
        - 가속화된 모바일 페이지(AMP, Accelerated Mobile Pages)를 만들기에 적합하다.
    - 단점 
        - 암호화를 하는 과정이 웹 서버에 부하를 준다.
        - HTTPS는 설치 및 인증서를 유지하는데 추가 비용이 발생한다.
        - HTTP에 비해 느리다. 
        - 인터넷 연결이 끊긴 경우 재인증 시간이 소요된다.
            - HTTP는 비연결형으로 웹 페이지를 보는 중 인터넷 연결이 끊겼다가 다시 연결되어도 페이지를 계속 볼 수 있다.
            - 그러나 HTTPS의 경우에는 소켓(데이터를 주고 받는 경로) 자체에서 인증을 하기 때문에 인터넷 연결이 끊기면 소켓도 끊어져서 다시 HTTPS 인증이 필요하다.
            
> :arrow_double_up:[Top](#2-network)    :leftwards_arrow_with_hook:[Back](https://github.com/jihyuno301/tech-interview/blob/master/README.md)    :information_source:[Home](https://github.com/jihyuno301/tech-interview)
> - [https://ko.wikipedia.org/wiki/HTTPS](https://ko.wikipedia.org/wiki/HTTPS)
> - [https://coding-start.tistory.com/208](https://coding-start.tistory.com/208)
> - [https://jeong-pro.tistory.com/89](https://jeong-pro.tistory.com/89)
> - [https://m.blog.naver.com/reviewer__/221294104297](https://m.blog.naver.com/reviewer__/221294104297)
> - [https://www.ibm.com/support/knowledgecenter/ko/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10630_.htm](https://www.ibm.com/support/knowledgecenter/ko/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10630_.htm)


### HTTP 요청 응답 헤더
- HTTP 헤더 내 일반 헤더(General Header) 항목
    - 요청 및 응답 메시지 모두에서 사용 가능한 일반 목적의(기본적인) 헤더 항목
    - 주요 항목들
        - **Date**: HTTP 메시지를 생성한 일시 (RFC 1123에서 규정)
            - `Date: Sat, 2 Oct 2018 02:00:12 GMT`
        - **Connection**: 클라이언트와 서버 간 연결에 대한 옵션 설정(다소 모호한 복잡성 있음)
            - `Connection: close` => 현재 HTTP 메시지 직후에 TCP 접속을 끊는다는 것을 알림
            - `Connection: Keep-Alive` => 현재 TCP 커넥션을 유지
        - **Cache-Control** 
        - **Pragma**
        - **Trailer**
-  HTTP 헤더 내 엔터티/개체 헤더 (Entity Header) 항목
    - 요청 및 응답 메시지 모두에서 사용 가능한 Entity(콘텐츠, 본문, 리소스 등)에 대한 설명 헤더 
    - 주요 항목들
        - **Content-Type**: 해당 개체에 포함되는 미디어 타입 정보
            - 컨텐츠의 타입(MIME 미디어 타입) 및 문자 인코딩 방식(EUC-KR,UTF-8 등)을 지정
            - 타입 및 서브타입(type/subtype)으로 구성 
            - `Content-Type: text/html; charset-latin-1` => 해당 개체가 html으로 표현된 텍스트 문서이고, iso-latin-1 문자 인코딩 방식으로 표현됨
        - **Content-Language**: 해당 개체와 가장 잘 어울리는 사용자 언어(자연언어)
        - **Content-Encoding**: 해당 개체 데이터의 압축 방식
            - `Content-Encoding: gzip, deflate`
            - 만일 압축이 시행되었다면, Content-Encoding 및 Content-Length 2개 항목을 토대로 압축 해제 가능 
        - **Content-Length**: 전달되는 해당 개체의 바이트 길이 또는 크기(10진수)
            - 응답 메시지 Body의 길이를 지정하거나, 특정 지정된 개체의 길이를 지정함
        - **Content-Location**: 해당 개체가 실제 어디에 위치하는가를 알려줌
        - **Content-Disposition**: 응답 Body를 브라우저가 어떻게 표시해야 할지 알려주는 헤더
            - inline인 경우 웹페이지 화면에 표시되고, attachment인 경우 다운로드
            - `Content-Disposition: inline`
            - `Content-Disposition: attachment; filename='filename.csv'`
            - 다운로드되길 원하는 파일은 attachment로 값을 설정하고, filename 옵션으로 파일명까지 지정해줄 수 있다.
            - 파일용 서버인 경우 이 태그를 자주 사용
        - **Content-Security-Policy**: 다른 외부 파일들을 불러오는 경우, 차단할 소스와 불러올 소스를 명시
            - *XSS 공격*에 대한 방어 가능 (허용한 외부 소스만 지정 가능)
            - `Content-Security-Policy: default-src https:` => https를 통해서만 파일을 가져옴
            - `Content-Security-Policy: default-src 'self'` => 자신의 도메인의 파일들만 가져옴
            - `Content-Security-Policy: default-src 'none'` => 파일을 가져올 수 없음
        - **Location**: 리소스가 리다이렉트(redirect)된 때에 이동된 주소, 또는 새로 생성된 리소스 주소
            - 300번대 응답이나 201 Created 응답일 때 어느 페이지로 이동할지를 알려주는 헤더
            - 새로 생성된 경우에 HTTP 상태 코드 `201 Created`가 반환됨
            - `HTTP/1.1 302 Found  Location: /`
                - 이런 응답이 왔다면 브라우저는 / 주소로 redirect한다.
        - **Last-Modified**: 리소스를 마지막으로 갱신한 일시
- HTTP 헤더 내 요청 헤더 (Request Header) 항목
    - 요청 헤더는 HTTP 요청 메시지 내에서만 나타나며 가장 방대하다.
    - 주요 항목들
        - **Host**: 요청하는 호스트에 대한 호스트명 및 포트번호 (***필수***)
            - Host 필드에 도메인명 및 호스트명 모두를 포함한 전체 URI(FQDN) 지정 필요
            - 이에 따라 동일 IP 주소를 갖는 단일 서버에 여러 사이트가 구축 가능
        - **User-Agent**: 클라이언트 소프트웨어(브라우저, OS) 명칭 및 버전 정보
        - **From**: 클라이언트 사용자 메일 주소 
            - 주로 검색엔진 웹 로봇의 연락처 메일 주소를 나타냄
            - 때로는, 이 연락처 메일 주소를 User-Agent 항목에 두는 경우도 있음
        - **Cookie**: 서버에 의해 Set-Cookie로 클라이언트에게 설정된 쿠키 정보
        - **Referer**: 바로 직전에 머물었던 웹 링크 주소
        - **If-Modified-Since**: 제시한 일시 이후로만 변경된 리소스를 취득 요청
        - **Authorization**: 인증 토큰(JWT/Bearer 토큰)을 서버로 보낼 때 사용하는 헤더
            - 토큰의 종류(Basic, Bearer 등) + 실제 토큰 문자를 전송
        - **Origin**
            - 서버로 POST 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지 나타냄
            - 여기서 요청을 보낸 주소와 받는 주소가 다르면 *CORS 에러*가 발생
            - 응답 헤더의 **Access-Control-Allow-Origin**와 관련 
    - 다음 4개는 주로 HTTP 메세지 Body의 속성 또는 내용 협상용 항목들
        - **Accept**: 클라이언트 자신이 원하는 미디어 타입 및 우선순위를 알림
            - `Accept: */*` => 어떤 미디어 타입도 가능
            - `Accept: image/*` => 모든 이미지 유형
        - **Accept-Charset**: 클라이언트 자신이 원하는 문자 집합
        - **Accept-Encoding**: 클라이언트 자신이 원하는 문자 인코딩 방식
        - **Accept-Language**: 클라이언트 자신이 원하는 가능한 언어
        - 각각이 HTTP Entity Header 항목 중에 `Content-Type, Content-Type charset-xxx, Content-Encoding, Content-Language`과 일대일로 대응됨
- HTTP 헤더 내 응답 헤더 (Response Header) 항목
    - 특정 유형의 HTTP 요청이나 특정 HTTP 헤더를 수신했을 때, 이에 응답한다.
    - 주요 항목들
        - **Server**: 서버 소프트웨어 정보
        - **Accept-Range**
        - **Set-Cookie**: 서버측에서 클라이언트에게 세션 쿠키 정보를 설정 (RFC 2965에서 규정)
        - **Expires**: 리소스가 지정된 일시까지 캐시로써 유효함
        - **Age**: 캐시 응답. max-age 시간 내에서 얼마나 흘렀는지 알려줌(초 단위)
        - **ETag**: HTTP 컨텐츠가 바뀌었는지를 검사할 수 있는 태그
        - **Proxy-authenticate**
        - **Allow**: 해당 엔터티에 대해 서버 측에서 지원 가능한 HTTP 메소드의 리스트를 나타냄
            - 때론, HTTP 요청 메세지의 HTTP 메소드 OPTIONS에 대한 응답용 항목
                - OPTIONS: 웹서버측 제공 HTTP 메소드에 대한 질의
            - `Allow: GET,HEAD` => 웹 서버측이 제공 가능한 HTTP 메서드는 GET,HEAD 뿐임을 알림 (405 Method Not Allowed 에러와 함께)
        - **Access-Control-Allow-Origin**: 요청을 보내는 프론트 주소와 받는 백엔드 주소가 다르면 *CORS 에러*가 발생
            * 서버에서 이 헤더에 프론트 주소를 적어주어야 에러가 나지 않는다.
            * `Access-Control-Allow-Origin: www.zerocho.com`
                * 프로토콜, 서브도메인, 도메인, 포트 중 하나만 달라도 CORS 에러가 난다.
            * `Access-Control-Allow-Origin: *`
                * 만약 주소를 일일이 지정하기 싫다면 *으로 모든 주소에 CORS 요청을 허용되지만 그만큼 보안이 취약해진다.
            * 유사한 헤더로 `Access-Control-Request-Method, Access-Control-Request-Headers, Access-Control-Allow-Methods, Access-Control-Allow-Headers` 등이 있다. 

> :arrow_double_up:[Top](#2-network)    :leftwards_arrow_with_hook:[Back](https://github.com/Do-Hee/tech-interview#2-network)    :information_source:[Home](https://github.com/Do-Hee/tech-interview#tech-interview)
> - [http://www.ktword.co.kr/abbr_view.php?nav=&m_temp1=5905&id=902](http://www.ktword.co.kr/abbr_view.php?nav=&m_temp1=5905&id=902)
> - [https://gmlwjd9405.github.io/2019/01/28/http-header-types.html](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)
> - [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

### CORS란 
- CORS(Cross Origin Resource Sharing)란 
    - 웹 서버에게 보안 cross-domain 데이터 전송을 활성화하는 cross-domain 접근 제어권을 부여한다.
- 배경
    - 처음 전송되는 리소스의 도메인과 다른 도메인으로부터 리소스가 요청될 경우 해당 리소스는 cross-origin HTTP 요청에 의해 요청된다.
    - 보안 상의 이유로, 브라우저들은 스크립트 내에서 초기화되는 cross-origin HTTP 요청을 제한한다.
        - 예를 들면, XMLHttpRequest는 same-origin 정책을 따르기에 XMLHttpRequest을 사용하는 웹 애플리케이션은 자신과 동일한 도메인으로 HTTP 요청을 보내는 것만 가능했다.
        - 웹 애플리케이션을 개선시키기 위해, 개발자들은 브라우저 벤더사들에게 XMLHttpRequest가 cross-domain 요청을 할 수 있도록 요청했고 이에 따라 CORS가 생겼다.
- 과정
    - CORS 요청 시에는 미리 OPTIONS 주소로 서버가 CORS를 허용하는지 물어본다.
    - 이때 Access-Control-Request-Method로 실제로 보내고자 하는 메서드를 알리고,
    - Access-Control-Request-Headers로 실제로 보내고자 하는 헤더들을 알린다.
    - Allow 항목들은 Request에 대응되는 것으로, 서버가 허용하는 메서드와 헤더를 응답하는데 사용된다.
    - Request랑 Allow가 일치하면 CORS 요청이 이루어진다.

> :arrow_double_up:[Top](#2-network)    :leftwards_arrow_with_hook:[Back](https://github.com/Do-Hee/tech-interview#2-network)    :information_source:[Home](https://github.com/Do-Hee/tech-interview#tech-interview)
> - [https://zamezzz.tistory.com/137](https://zamezzz.tistory.com/137)
> - [https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS)

### GET 메서드와 POST 메서드
* HTTP 프로토콜을 이용해서 서버에 데이터(요청 정보)를 전달할 때 사용하는 방식
* GET 메서드 방식
  * 개념
    * 정보를 조회하기 위한 메서드
    * 서버에서 어떤 데이터를 가져와서 보여주기 위한 용도의 메서드
    * **가져오는 것(Select)**
  * 사용 방법
    * URL의 끝에 '?'가 붙고, 요청 정보가 (key=value)형태의 쌍을 이루어 ?뒤에 이어서 붙어 서버로 전송한다.
    * 요청 정보가 여러 개일 경우에는 '&'로 구분한다.
    * Ex) `www.urladdress.xyz?name1=value1&name2=value2`
  * 특징
    * URL에 요청 정보를 붙여서 전송한다.
    * URL에 요청 정보가 이어붙기 때문에 길이 제한이 있어서 대용량의 데이터를 전송하기 어렵다.
      * 한 번 요청 시 전송 데이터(주솟값 + 파라미터)의 양은 255자로 제한된다.(HTTP/1.1은 2048자)
    * 요청 정보를 사용자가 쉽게 눈으로 확인할 수 있다.
      * POST 방식보다 보안상 취약하다.
    * HTTP 패킷의 Body는 비어 있는 상태로 전송한다.
      * 즉, Body의 데이터 타입을 표현하는 'Content-Type' 필드도 HTTP Request Header에 들어가지 않는다.
    * POST 방식보다 빠르다.
      * GET 방식은 캐싱을 사용할 수 있어, GET 요청과 그에 대한 응답이 브라우저에 의해 캐쉬된다.
* POST 메서드 방식
  * 개념
    * 서버의 값이나 상태를 바꾸기 위한 용도의 메서드
    * **수행하는 것(Insert, Update, Delete)**
  * 사용 방법
    * 요청 정보를 HTTP 패킷의 Body 안에 숨겨서 서버로 전송한다.
    <!-- * Form 태그을 이용해서 submit 하는 형태 -->
    * Request Header의 Content-Type에 해당 데이터 타입이 표현되며, 전송하고자 하는 데이터 타입을 적어주어야 한다.
      * Default: application/octet-stream
      * 단순 txt의 경우: text/plain
      * 파일의 경우: multipart/form-date
  * 특징
    * Body 안에 숨겨서 요청 정보를 전송하기 때문에 대용량의 데이터를 전송하기에 적합하다.
    * 클라이언트 쪽에서 데이터를 인코딩하여 서버로 전송하고, 이를 받은 서버 쪽이 해당 데이터를 디코딩한다.
    * GET 방식보다 보안상 안전하다.
* Q. 조회하기 위한 용도 POST가 아닌 GET 방식을 사용하는 이유?
  1. 설계 원칙에 따라 GET 방식은 서버에게 여러 번 요청을 하더라도 동일한 응답이 돌아와야 한다. (Idempotent, 멱등)
      * GET 방식은 **가져오는 것(Select)** 으로, 서버의 데이터나 상태를 변경시키지 않아야 한다.
          * Ex) 게시판의 리스트, 게시글 보기 기능
          * 예외) 방문자의 로그 남기기, 글을 읽은 횟수 증가 기능
      * POST 방식은 **수행하는 것** 으로, 서버의 값이나 상태를 바꾸기 위한 용도이다.
          * Ex) 게시판에 글쓰기 기능
  2. 웹에서 모든 리소스는 Link할 수 있는 URL을 가지고 있어야 한다.
      * 어떤 웹페이지를 보고 있을 때 다른 사람한테 그 주소를 주기 위해서 주소창의 URL을 복사해서 줄 수 있어야 한다.
      * 즉, 어떤 웹페이지를 조회할 때 원하는 페이지로 바로 이동하거나 이동시키기 위해서는 해당 링크의 정보가 필요하다.
      * 이때 POST 방식을 사용할 경우에 값(링크의 정보)이 Body에 있기 때문에 URL만 전달할 수 없으므로 GET 방식을 사용해야한다. 그러나 글을 저장하는 경우에는 URL을 제공할 필요가 없기 때문에 POST 방식을 사용한다.

<!-- * 클라이언트에서 서버로 데이터를 전송하려면 GET이나 POST 방식밖에 없다. -->

> :arrow_double_up:[Top](#2-network)    :leftwards_arrow_with_hook:[Back](https://github.com/Do-Hee/tech-interview#2-network)    :information_source:[Home](https://github.com/Do-Hee/tech-interview#tech-interview)
> - [https://blog.outsider.ne.kr/312](https://blog.outsider.ne.kr/312)
> - [https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)
> - [https://www.w3schools.com/tags/ref_httpmethods.asp](https://www.w3schools.com/tags/ref_httpmethods.asp)

### 쿠키와 세션
* HTTP 프로토콜의 특징
  * 비연결 지향(Connectionless)
      * 클라이언트가 request를 서버에 보내고, 서버가 클라이언트에 요청에 맞는 response를 보내면 바로 연결을 끊는다.
  * 상태정보 유지 안 함(Stateless)
      * 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않는다.
* 쿠키와 세션의 필요성
    * HTTP 프로토콜은 위와 같은 특징으로 모든 요청 간 의존관계가 없다.
    * 즉, 현재 접속한 사용자가 이전에 접속했던 사용자와 같은 사용자인지 아닌지 알 수 있는 방법이 없다.
    * 계속해서 연결을 유지하지 않기 때문에 리소스 낭비가 줄어드는 것이 큰 장점이지만, 통신할 때마다 새로 연결하기 때문에 클라이언트는 매 요청마다 인증을 해야 한다는 단점이 있다.
    * 이전 요청과 현재 요청이 같은 사용자의 요청인지 알기 위해서는 상태를 유지해야 한다.
    * HTTP 프로토콜에서 상태를 유지하기 위한 기술로 쿠키와 세션이 있다.
* 쿠키(Cookie)란?
  * 개념
      * 클라이언트 로컬에 저장되는 키와 값이 들어있는 파일이다.
      * 이름, 값, 유효 시간, 경로 등을 포함하고 있다.
      * 클라이언트의 상태 정보를 브라우저에 저장하여 참조한다.
  * 구성 요소
      * 쿠키의 이름(name)
      * 쿠키의 값(value)
      * 쿠키의 만료시간(Expires)
      * 쿠키를 전송할 도메인 이름(Domain)
      * 쿠키를 전송할 경로(Path)
      * 보안 연결 여부(Secure)
      * HttpOnly 여부(HttpOnly)
  * 동작 방식  
      <img src="./images/cookie-process.png" width="50%" height="50%">
      1. 웹브라우저가 서버에 요청
      2. 상태를 유지하고 싶은 값을 쿠키(cookie)로 생성
      3. 서버가 응답할 때 HTTP 헤더(Set-Cookie)에 쿠키를 포함해서 전송
          ```java
          Set−Cookie: id=doy
          ```
      4. 전달받은 쿠키는 웹브라우저에서 관리하고 있다가, 다음 요청 때 쿠키를 HTTP 헤더에 넣어서 전송
          ```java
          cookie: id=doy
          ```
      5. 서버에서는 쿠키 정보를 읽어 이전 상태 정보를 확인한 후 응답
  * 쿠키 사용 예
      * 아이디, 비밀번호 저장
      * 쇼핑몰 장바구니
* 세션(Session)이란?
  * 개념
      * 일정 시간 동안 같은 브라우저로부터 들어오는 요청을 하나의 상태로 보고 그 상태를 유지하는 기술이다.
      * 즉, 웹 브라우저를 통해 서버에 접속한 이후부터 브라우저를 종료할 때까지 유지되는 상태이다.
  * 동작 방식  
      <img src="./images/session-process.png" width="70%" height="70%">
      1. 웹브라우저가 서버에 요청
      2. 서버가 해당 웹브라우저(클라이언트)에 유일한 ID(Session ID)를 부여함
      3. 서버가 응답할 때 HTTP 헤더(Set-Cookie)에 Session ID를 포함해서 전송  
      쿠키에 Session ID를 JSESSIONID 라는 이름으로 저장
          ```java
          Set−Cookie: JSESSIONID=xslei13f
          ```
      4. 웹브라우저는 이후 웹브라우저를 닫기까지 다음 요청 때 부여된 Session ID가 담겨있는 쿠키를 HTTP 헤더에 넣어서 전송
          ```java
          Cookie: JSESSIONID=xslei13f
          ```
      5. 서버는 세션 ID를 확인하고, 해당 세션에 관련된 정보를 확인한 후 응답
  * 세션 사용 예
    * 로그인
> 세션도 쿠키를 사용하여 값을 주고받으며 클라이언트의 상태 정보를 유지한다.  
> 즉, 상태 정보를 유지하는 수단은 **쿠키** 이다.
* 쿠키와 세션의 차이점
  * 저장 위치
      * 쿠키 : 클라이언트
      * 세션 : 서버
  * 보안
      * 쿠키 : 클라이언트에 저장되므로 보안에 취약하다.
      * 세션 : 쿠키를 이용해 Session ID만 저장하고 이 값으로 구분해서 서버에서 처리하므로 비교적 보안성이 좋다.
  * 라이프사이클
      * 쿠키 : 만료시간에 따라 브라우저를 종료해도 계속해서 남아 있을 수 있다.
      * 세션 : 만료시간을 정할 수 있지만 브라우저가 종료되면 만료시간에 상관없이 삭제된다.
  * 속도
      * 쿠키 : 클라이언트에 저장되어서 서버에 요청 시 빠르다.
      * 세션 : 실제 저장된 정보가 서버에 있으므로 서버의 처리가 필요해 쿠키보다 느리다.

> :arrow_double_up:[Top](#2-network)    :leftwards_arrow_with_hook:[Back](https://github.com/Do-Hee/tech-interview#2-network)    :information_source:[Home](https://github.com/Do-Hee/tech-interview#tech-interview)
> - [https://doooyeon.github.io/2018/09/10/cookie-and-session.html](https://doooyeon.github.io/2018/09/10/cookie-and-session.html)
