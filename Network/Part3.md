# 2. Network - Part3

<br>

### :book: Contents
- [DNS](#DNS란)
- [REST와 RESTful의 개념](#REST와-RESTful)
- [소켓(Socket)이란](#Socket이란)
- [Socket.io와 WebSocket의 차이](#양방향-통신)
- [Frame, Packet, Segment, Datagram](#Frame-Packet-Segment-Datagram)

<br>

---

<br>

### DNS란
<img src ="https://www.seobility.net/en/wiki/images/d/d0/DNS-Server.png" width=600px height=400px></img>
- DNS의 등장 배경
	- TCP/IP 프로토콜을 사용하는 네트워크 안에서는 IP주소를 알고 있어야 상대방 장비와 연결이 가능하다.
	- 즉, 네트워크에서 도메인이나 호스트 이름을 숫자로 된 IP 주소로 해석해 주기 위해 TCP/IP Network Service인 DNS 등장
- DNS의 개념 (Domain Name System)
    - 숫자로 구성된 네트워크 주소인 IP주소를 사람이 이해하기 쉬운 명칭인 도메인 이름으로 상호 매칭시켜주는 시스템
    - 도메인에 해당하는 IP주소를 알려주거나 반대로 IP주소에 해당하는 도메인을 알려주는 서비스
	- UDP와 TCP **포트 53번**을 사용한다.
		- UDP : 일반적인 DNS 조회를 할 경우에 사용
		- TCP : Zone Transfer(영역 전송)와 512Byte를 초과하는 DNS 패킷을 전송해야 할 경우
	- 분산된 DB를 이용해 각 조직들은 자신들의 도메인 정보를 관리하는 DNS 서버를 자체적으로 운영
	- 이 수 많은 도메인의 **DNS 서버들이 상호 연동되어 있는 Domain Name Space를 구성**하게 된다.
<br></br>
- DNS의 동작 원리
	![DNS 프로세스](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F269b0a50-1b13-482f-921c-254237b3c8e5%2FUntitled.png?table=block&id=3fb206b4-46ba-4592-8ec9-951ab5c30b09&width=3140&userId=&cache=v2)
1. 웹브라우저에 `www.naver.com`을 입력하면 먼저 `Local DNS`에게   
`www.naver.com`이라는 hostname에 대한 IP 주소를 질의하여 Local DNS에 없으면   
다른 DNS name 서버 정보를 받는다. `(Root DNS 정보)`
2. `Root DNS` 서버에 `www.naver.com` 질의
3. Root DNS 서버로부터 `com 도메인`을 관리하는 `TLD(Top-Level Domina)` 서버 정보를 전달 받는다.
4. `TLD` 서버에 `www.naver.com` 질의
5. TLD에서 `www.naver.com`을 관리하는 DNS 정보를 전달한다.
6. `"www.naver.com"` 도메인을 관리하는 DNS 서버에 `"www.naver.com"` 호스트 네임에 대한 IP 주소 질의
7. `Local DNS` 서버에게 `www.naver.com`에 대한 IP 주소는 `222.122.195.6이라고 응답`
8. Local DNS는 `www.naver.com`에 대한 IP 주소를 캐싱하고 IP 주소 정보를 전달
<br></br>
- DNS의 구성 요소   
    ![DNS_img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F3k159%2Fbtqzb8L6Qnu%2FJk5Z1RzHNuvqwZMcMGEwSk%2Fimg.png)
	1. 도메인 네임 스페이스 (Domain Name Space) : DNS가 저장, 관리하는 계층적 구조
		- 최상위에 루트 DNS 서버가 존재하고, 그 하위로 인터넷에 연결된 모든 노드가 연속해서 이어진 계층 구조로 구성
		- 각 레벨의 도메인은 그 하위 도메인에 관한 정보를 관리하는 구조 (계층적 구조)
		- 도메인(Domain) : 도메인 네임 스페이스의 서브 트리
	2. 네임 서버 (Name Sever) : 도메인 네임 스페이스의 트리 구조에 대한 정보를 가지고 있는 서버
		- 네임 서비스 : 도메인 이름을 IP 주소로 변환하는 것
		- 리졸버로부터 요청 받은 도메인 이름에 대한 IP주소를 다시 리졸버로 전달해주는 역할을 수행
		- 해당 도메인을 관리하는 주 네임 서버인 Primary Name Server와 보조 Secondary Name Server로 구성
	3. 리졸버 (Resolver)
		- client의 요청을 네임 서버로 전달하고 네임 서버로부터 정보를 받아 client에게 제공하는 기능 수행
	4. 스티브 리졸버(Stub Resolver)
		- 질의를 네임 서버로 전달하고 응답을 웹 브라우저로 전달하는 인터페이스 기능만을 수행
<br></br>
- DNS의 질의/응답 과정
  1. 재귀적 질의 (Recursive Queries)
    - **가장 간단한 유형의 DNS 쿼리**로, Client가 원하는 정보를 전달해 주거나, 정보가 없다면 에러 메시지를 전달
    - www.naver.com에 대한 변환 요청 과정  
    ![DNS_img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile7.uf.tistory.com%2Fimage%2F9961C53B5B1E96630C436A)
  2. 반복적 질의 (Iterative Queries)
    - 질의를 요청한 client 또는 server가 최종적인 응답을 받을 때까지 요청과 응답을 반복적으로 진행
    - 질의를 날릴 때 마다 서버는 질의에 응답이 가능한 NS 목록으로 응답한다.
    - www.naver.com에 대한 변환 요청 과정
    ![DNS_img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F9996813A5B1E97FB2B150D)
<br></br>
- DNS의 역할
  1. 호스트 에일리어싱(Host Aliasing) : 복잡한 호스트 네임을 가진 호스트는 하나 이상의 별명을 가질 수 있다.
    - ex) www.naver.com == naver.com 따라서 정식 호스트 네임을 얻기 위해 사용한다.
  2. 메일 서버 에일리어싱(Mail Server Aliasing)
    - 위와 마찬가지로 메일주소의 별명을 가질 수 있고 DNS는 정식 호스트 네임을 알려주는 역할
  3. 도메인 네임을 IP주소로 변환해주는 역할을 한다.
<br></br>
> - [https://security-nanglam.tistory.com/24?category=800892](https://security-nanglam.tistory.com/24?category=800892)
> - [https://peemangit.tistory.com/52](https://peemangit.tistory.com/52)

<br>

### REST와-RESTful
[REST](./img/rest.png)

- REST이란?
	- 분산 시스템 설계를 위한 **아키텍처 스타일** (아키텍처 = **제약 조건의 집합**)
- RESTfUL이란?
	- 제약 조건의 집합(아키텍처 스타일, 아키텍처 원칙)을 **모두 만족하는 것**
	- RESTful API는 REST 아키텍처 원칙을 모두 만족하는 API라는 뜻
	- RESTful API를 이용해서 하나의 큰 서비스 앱을 여러 모듈화된 마이크로 서비스들로 나눌 수 있게 됨
- REST가 필요한 이유
	1. 분산 시스템의 도입
	- 거대한 애플리케이션을 모듈, 기능별로 분리하기 쉬워졌다.
	- RESTful API를 서비스하기만 하면 다른 module 또는 Application들이라도 RESTful API를 통해 상호 간에 통신 가능
	2. 멀티 플랫폼 클라이언트 지원
	- WEB을 위한 HTML 및 이미지 등을 보내던 것과 달리 데이터만 보내면 여러 클라이언트에서 데이터를 적절히 보여줘야 한다.
	- RESTful API를 사용하면서 데이터만 주고 받기 때문에 여러 클라이언트가 자유롭고 부담없이 데이터 이용 가능
	- 서버도 요청한 데이터만 깔끔하게 보내주면되기 때문에 가벼워지고 유지보수성도 좋아졌다.
- REST의 구성 요소
	1. HTTP URI = 자원 (Resource)
	2. HTTP Method = 행위
	3. MIME Type = 표현 방식
```  javascript
	GET /100 HTTP/1.1
	Host : www.naver.com 
```
- 요청 메시지의 예시 (REST 방식을 이용한 Request 예제)
	- 위와 같은 Request 메시지가 있으면 URI 자원은 "/100"이고, HTTP Method는 "GET"이다.
	- MIME 타입은 보통 Response HTTP header 메시지에 Content-type으로 쓰인다.
	- 즉, www.naver.com 서버에 /100 이라는 자원을 GET(조회)하고 싶다는 요청으로 해성 가능하다.
```  javascript
	HTTP/1.1 200 OK
	Content-Type : application/json-patch+json
	
	[{"title" : "helloworld"}]
```
- 응답 메시지의 예시
	- 이런 Response가 왔다고 가정했을 때
	- Content-Type을 보고 클라이언트는 IANA라는 타입들의 명세를 모아놓은 사이트에 방문해
	- application/json-patch+json 이라는 타입의 명세를 확인하고 아래 Body의 내용이 json타입임을 알 수 있는 것
- 제약 조건의 의미
	1. Client/Server 방식
	2. Stateless : 각 요청에 클라이언트의 context가 서버에 저장되어서는 안된다.
		- 즉, HTTP Sesstion과 같은 컨텍스트 저장소에 상태 정보를 저장하지 않는다.
		- 상태 정보를 저장하지 않으면 각 API 서버는 들어오는 요청만을 들어오는 메시지로만 처리
		- 세션과 같은 컨텍스트 정보를 신경쓸 필요가 없기 때문에 구현이 단순해진다.
	3. Cacheable : 클라이언트는 응답을 캐싱할 수 있어야 한다.
	4. Layered System : 클라이언트는 서버에 직접 연결되었는지 미들웨어에 연결되었는지 알 필요가 없어야 한다.
	5. Code on demand (option) : 서버는 코드를 클라이언트에게 보내서 실행하게 할 수 있어야 한다.
	6. **Uniform Interface** : RESTful 하기 위해서 가장 잘 지켜져야 하는 요소
		- 자원은 유일하게 식별가능해야 한다.
		- HTTP Method로 표현을 담아야 한다.
		- 메시지는 스스로 설명(self-descrptive)해야 한다.
		- 하이퍼링크를 통해서 애플리케이션의 상태가 전이(HATEOAS)되어야 한다.
			- HATEOAS : Link라는 HTTP 헤더에 다른 리소스를 가리켜 주는 값을 넣는 방법으로 해결
			- Spring에는 spring-data-rest, spring hateoas, spring-rest-doc으로
			- 두 제약을 지키기위해 사용할 수 있는 라이브러리가 존재
```  javascript
	HTTP/1.1 200 OK
	Content-Type : application/json
	Link : </Spring/1>; rel="previous"
	
	[{"title" : "helloworld"}]
```
- HATEOAS의 예시
	- 위와 같이 해당 정보에서 다른 정보로 넘어갈 수 있는 하이퍼링크를 명시해주면 된다.
- REST API의 장단점
	- **장점**
		- 메시지를 단순하게 표현할 수 있고 WEB의 원칙인 확장에 유연하다. (멀티플랫폼)
		- 별도의 장비나 프로토콜이 필요없이 기존의 HTTP 인프라를 이용할 수 있다. (사용이 용이함)
		- Server, Client를 완전히 독립적으로 구현할 수 있다.
	- **단점**
		- 표준, 스키마가 없다. 결국 API 문서가 만들어지는 이유이다.
		- 행위에 대한 메소드가 제한적이다. (GET, POST, PUT, DELETE, HEAD, ... )
- URI와 URL의 차이점
	- **URI : Uniform Resource Identifier**
		- REST에서는 모든 것을 리소스로 표현한다. 그리고 그 자원은 유일한 것을 나타낸다. (Identifier, 식별자)
		- 파일 뿐만 아니라 여러 자원들 까지도 포함하는 개념
	- **URL : Uniform Resource Locator**
		- 반면에 과거의 웹에서는 html같은 파일들을 주고 받았기 때문에 Identifier의 개념이 따로 필요없었다.
		- 파일의 위치를 가리키는 Locator를 썼다.
<br></br>
> - [https://jeong-pro.tistory.com/180](https://jeong-pro.tistory.com/180)

<br>

### Socket이란
- 소켓(Socket)의 개념
	- 네트워크상에서 동작하는 프로그램 간 통신의 종착점(Endpoint)
	- 두 프로그램이 네트워크를 통해 서로 통신을 할 수 있도록 양쪽에 생성되는 링크의 단자   
	= 서로 다른 프로세스끼리 데이터 전달이 가능
- 소켓(Socket)과 포트(Port)의 차이
	- 포트(Port) : 네트워크를 통해 데이터를 주고 받는 프로세스를 식별하기 위해 호스트 내부적으로 프로세스가 할당받는 고유한 값
	- 소켓(Socket) : 프로세스가 네트워크를 통해서 데이터를 주고 받으려면 반드시 열어야 하는 창구   
	* 보내는 쪽도 받는 쪽도 동일하게 소켓을 열어야 한다.
- 소켓 통신의 Workflow  
![워크플로우](https://t1.daumcdn.net/cfile/tistory/993EAD4E5C66371C2B)
	- **서버 (Server)**
		1. **socket()** 함수를 이용하여 소켓을 생성
		2. **bind()** 함수로 ip와 port 번호를 설정
		3. **listen()** 함수로 클라이언트의 접근 요청에 수신 대기열을 만들어 몇 개의 클라이언트를 대기 시킬지 결정
		4. **accept()** 함수를 사용하여 클라이언트와의 연결을 기다림
	- **클라이언트 (Client)**
		1. socket() 함수로 가장 먼저 소켓을 엶.
		2. connect() 함수를 이용하여 통신 할 서버의 설정된 ip와 port 번호에 통신을 시도
		3. 통신을 시도 시, 서버가 accept() 함수를 이용하여 클라이언트의 socket descriptor를 반환
		4. 이를 통해 클라이언트와 서버가 서로 read(), write()를 하며 통신 (이 과정이 반복)
<br></br>
> - [https://song-yoshiii.tistory.com/3](https://song-yoshiii.tistory.com/3)

<br>

### 양방향-통신
- 웹 브라우저에서 양방향 통신 (Socket.io와 WebSocket)
> http 프로토콜은 요청/응답 패러다임이기에 클라이언트에서 요청을 보내야만 그에 대한 응답을 받는다.  
그러나, 클라이언트가 요청을 보내지 않아도 서버에서 클라이언트쪽으로 데이터를 보내야 하는 경우가 생기게 된다.  
http 프로토콜로 통신하는 경우 연결이 유지되지 않기 때문에 먼저 요청을 보내는 것이 불가능하기 떄문에  
이러한 문제를 해결하기 위해 양방향 통신의 개념이 고안이 되었다.

- Websocket의 개념
![웹소켓](http://www.secmem.org/assets/images/websocket-socketio/websocket.png)
	- TCP 프로토콜에서 양방향 커뮤니케이션을 가능하게 하는 통신 프로토콜
	- HTTP의 프록시를 지원할 수 있게 하기 위해서 HTTP 포트 80, 443에서 동작하도록 설계 때문에 HTTP와 호환 가능
	- 웹 소켓을 사용하기 위해서는 HTTP Upgrade header를 사용해   
	  HTTP 프로토콜에서 웹 소켓 프로토콜로 전환 ( = 웹 소켓 핸드 쉐이크)
	
- Websocket의 특징
	- 양방향으로 원할 때 요청을 보낼 수 있으며 stateless한 HTTP에 비해 오버헤드가 적어서 유용
	- 클라이언트가 먼저 서버에게 요청하지 않고, 서버가 클라이언트에게 contents를 보내고 이 연결이 계속 열린채로 유지됨
- Websocket의 간단한 예제
```JAVA
if ('WebSocket' in window) {  
    var oSocket = new WebSocket(“ws://localhost:80”);

    oSocket.onmessage = function (e) { 
        console.log(e.data); 
    };

    oSocket.onopen = function (e) {
        console.log(“open”);
    };

    oSocket.onclose = function (e) {
        console.log(“close”);
    };

    oSocket.send(“message”);
    oSocket.close();
}
```
1. ws:// : WebSocket 프로토콜을 나타내며 URI 스키마를 사용한다. 암호화 소켓은 https:// 처럼 wss://를 사용
2. 먼저 new WebSocket() 메서드로 웹서버와 연결한다.
3. 생성된 WebSocket 인스턴스를 이용하여 소켓에 연결할 때(onopen), 소켓 연결을 종료할 때(onclose),   
	메시지를 받았을 때(onmessage) 등의 이벤트를 각각 정의할 수 있다.
4. 서버에 메시지를 보내고 싶을 때에는 send() 메서드를 이용한다.
<br></br>
- Socket.io의 개념
	- JavaScript를 이용하여 브라우저 종류에 상관없이 실시간 웹을 구현할 수 있도록 한 기술
	- 브라우저와 웹 서버의 종류와 버전을 파악해 가장 적합한 기술을 선택해 실시간 웹을 구현할 수 있도록 해주는 기술
		- 예를 들어 브라우저에 Flash Socket을 지언하는 Flash Plugin v10.0.0 이상이 있으면   
			FlashSocket을 이용 없으면 Ajax Long Polling 방식을 이용
	- Web socket과 달리 node.js의 모듈이다.
- Socket.io를 활용한 다양한 예제
	- https://socket.io/
	
<br></br>
> - [https://velog.io/@imacoolgirlyo/web-socket과-socket.io](https://velog.io/@imacoolgirlyo/web-socket과-socket.io)
> - [https://d2.naver.com/helloworld/1336](https://d2.naver.com/helloworld/1336)

<br>

### Frame-Packet-Segment-Datagram
- 프로토콜 데이터 단위 (PDU / Protocol Data Unit)
	- 데이터 통신에서 상위 계층이 전달한 데이터에 붙이는 제어정보
	- 모든 계층에서 우리가 전송하는 데이터 자체는 동일하지만   
	  각 레이어를 거치면서 헤더 정보가 추가되며 이름이 달라진다.
	- 동일 레이어 내에서 데이터 단위를 부르는 이름
- 서비스 데이터 단위 (SDU / Service Data Unit)
	- 상향 / 하향 통신 레이어 간에 전달되는 실제 정보
	- 즉, PDU = PCI(프로토콜 통신 유닛의 줄인말로 쉽게 말하면 헤더 정보) + SDU
![프로토콜 링크](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/OSI_Model_v1.svg/870px-OSI_Model_v1.svg.png)

> 쉽게 생각하면 사용자는 Data라 부르고,   
TCP는 Segment라고 부르고, IP는 Packet이라고 부르고,   
데이터 링크는 Frame, 컴퓨터 하드웨어는 그것을 Bit로 연산하고 다루게 되는 것

1. **데이터 그램**
- 사용자의 순수한 message를 다르게 부르는 말

2. **세그먼트 (전송 계층)**
- 상위 계층에서 데이터를 전달받은 전송계층에서는 아래의 정보들을 추가해 그룹화 한다. 이때부터는 데이터가 아닌 세그먼트라 불림
	- 발신지 포트 : 발싱하는 application의 포트
	- 목적지 포트 : 수신해야 할 application의 포트
	- 순서 번호 : 순차적 전송할 경우 순서를 붙이며, 순서가 어긋나면 목적지 프로토콜이 이를 바로 잡는다.
	- 오류검출코드 : 발신지와 목적지 프로토콜은 세그먼트를 연산하여 오류 검출 코드를 각각 만든다.   
> 만약 발신지에서 전송한 세그먼트에 포함된 오류 검출 코드와 목적지에서 만든 오류 검출 코드가 다르다면   
전송되는 과정에서 오류가 발생한 것이다. 이 경우, 수신측은 그 세그먼트를 폐기하고 복구 절차를 밟는다.   
오류검출코드는 체크섬, 프레임 체크 시퀀스라고도 부른다.

3. **패킷 (네트워크 계층)**
- 전송 계층으로부터 전달받은 세그먼트는 네트워크 계층의 정보를 포함해 패킷이라고 불리게 된다.
	- 발신지 컴퓨터 주소(Destination IP) : 패킷의 발신자 주소
	- 목적지 컴퓨터 주소(Source IP) : 패킷의 수신자 주소
	- 서비스 요청 : 네트워크 접속 프로토콜은 우선 순위와 같은 서브 네트워크의 사용을 요청할 수 있다.   

> **전반적인 네트워킹에서의 packet의 의미**  
Circuit switching vs Packet switching의 개념으로 회선 교환에 대비되는 모든 데이터 교환을 일컬어 packet switching이라고 한다.   
ATM cell ATM cell switching은 물론이고, Ethernet frame switching, Frame Relay frame switching, 등등   
회선교환이 아닌 모든 데이터 단위가 packet의 범주에 들어간다.

4. **프레임 (데이터 링크 계층)**
- 최종적으로 데이터 전송을 하기 전 패킷헤더에 Mac Address와 CRC를 위한   
  Trailer(데이터가 정확하게 수신 할 수 있는가에 대한 무결성을 검증하기 위한 FCS)를 붙인 단위
	- CRC : 순환 중복 검사 (데이터를 전송할 때 전송된 데이터에 오류가 있는지 확인하기 위한 체크값을 결정하는 방식)
	- FCS : 프레임 검사 시퀀스 (Frame Check Sequence)
		
<br></br>
> - [https://velog.io/@hidaehyunlee/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8-%ED%8C%A8%ED%82%B7-%ED%97%B7%EA%B0%88%EB%A6%B4-%EB%95%90-PDU%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90](https://velog.io/@imacoolgirlyo/web-socket과-socket.io)

<br>
