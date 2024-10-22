# Web
**:book: Contents**
* [Ajax란](#Ajax란)
* [JSON과 XML차이](#JSON과-XML차이)
* [REST API](#REST-API)
* [쿠키와 세션](#쿠키와-세션)
* [JWT](#JWT)
* [쿠키와 세션과 JWT 차이](#쿠키와-세션과-JWT-차이)
---

### Ajax란

* 기본적으로 HTTP 프로토콜은 클라이언트쪽에서 서버에 Request를 보내고 서버쪽에서 Response를 받으면 이어졌던 연결이 끊기게 된다.
* 그래서 화면의 내용을 갱신하기 위해서는 다시 Request를 하고 Response를 하면서 **페이지 전체**를 갱신하게 된다. 
* 하지만 페이지의 일부분만 갱신할 경우에는 페이지 전체를 다시 다시 로드해야하기 때문에 엄청난 자원낭비와 시간낭비를 초래할 수 있다.
* 이에 따라 Ajax 기술을 활용하여 HTML 페이지 전체가 아닌 일부분만 갱신할 수 있도록 XMLHTTPRequest 객체를 통해 서버에 Request 한다.
* 이 경우에는 JSON이나 XML 형태로 필요한 데이터만 받아 갱신하기 때문에 그만큼의 자원과 시간을 아낄 수 있다.

* 장점
  * 웹 페이지 속도 향상
  * 서버의 처리가 완료될 때까지 기다리지 않고 처리 가능하다.
  * 서버에서 데이터만 전송해오면 되기 때문에 전체적이 코딩의 양이 줄어든다.
  * 사진의 제목이나 태그를 페이지 리로드 없이 수정할 수 있다.
* 단점
  * 히스토리 관리가 안된다(보안에 좀 더 신경을 써야한다)
  * 연속으로 데이터를 요청하면 서버부하가 증가할 수 있다
  * XML HttpRequest를 통해 통신을 하는 경우 사용자에게 아무런 진행 정보가 주어지지 않는다. 그래서 아직 요청이 완료되지 않았는데 
  사용자가 페이지를 떠나거나 오작동할 우려가 발생하게 된다. 


### JSON과 XML차이
1. JSON이란
    * JavaScript Object Notation의 약자
    * JSON은 XML의 대안으로서 좀 더 쉽게 데이터를 교환하고 저장하기 위하여 만들어진 **텍스트 기반의 데이터 교환 표준**이다.
    * 텍스트 기반이기 때문에 어떠한 프로그래밍 언어에서도 JSON 데이터를 읽고 사용할 수 있다.
    * 사람과 기계 모두 읽기 편하도록 고안되었다. 그로 인해 브라우저 영역에서도 쉽고 빠르게 그 의미를 해석할 수 있다. 
2. XML이란
    * XML은 HTML과 매우 비슷한 문자 기반의 마크업 언어(text-based markup language)이다.
    * 이 언어는 사람과 기계가 동시에 읽기 편한 구조로 되어 있다. 
    * HTML처럼 데이터를 보여주는 보여주는 목적이 아닌, 데이터를 저장하고 전달할 목적으로만 만들어졌다.
 
3. 공통점
    * 둘 다 데이터를 저장하고 전달하기 위해 고안되었다.
    * 둘 다 기계뿐만 아니라 사람도 쉽게 읽을 수 있다.
    * 계층적인 구조를 가진다.
    * 다양한 프로그래밍 언어에 의해 파싱될 수 있다.
    * XMLHttpRequest 객체를 이용하여 서버로부터 데이터를 전송받을 수 있다. 
4. 차이점
    * JSON의 구문이 XML 구문보다 더 짧다
    * JSON 데이터가 XML 데이터보다 더 빨리 읽고 쓸 수 있다.
    * XML은 배열을 사용할 수 없지만, JSON은 배열을 사용할 수 있다.
    * **But**  JSON은 전송받은 **데이터의 무결성**을 사용자가 직접 검증해야 한다. 따라서 데이터의 검증이 필요한 곳에서는 스키마를 사용하여
    데이터의 무결성을 검증할 수 있는 XML이 아직도 많이 사용되고 있다.

> - [http://www.tcpschool.com/json/json_intro_xml](http://www.tcpschool.com/json/json_intro_xml)


### REST API

1. REST의 Method : POST(CREATE), GET(SELECT), PUT(UPDATE), DELETE(DELETE)
2. 리소스
    * http://myweb/users와 같은 URI
    * 모든 것을 Resource로 식별하고, 세부 Resource에는 id를 붙인다. 
3. 메시지
    * JSON, XML과 같은 형태가 있다.
4. REST의 특징
    * HTTP 표준만 맞는다면, 어떠 기술도 가능한 Interface 스타일이다.
    * API 메시지만 보고, API를 이해할 수 있는 구조이다. (Resource, Method를 이용해 무슨 행위를 하는지 직관적으로 이해할 수 있다)
    * HTTP Session과 같은 컨텍스트 저장소에 상태 정보 저장 안함 => REST API 실행중 실패가 발생한 경우, Transaction 복구를 위해 기존의 상태를 저장할 필요가 있다. (POST Method 제외)
> - [https://gyoogle.dev/blog/web-knowledge/REST%20API.html](https://gyoogle.dev/blog/web-knowledge/REST%20API.html)

### 쿠키와 세션
1. 인증 방식 순서
    1. 사용자가 로그인을 한다.
    2. 서버에서는 계정정보를 읽어 사용자를 확인한 후, 사용자의 고유한 ID값을 부여하여 세션 저장소에 저장한 후, 이와 연결되는 세션ID를 발행합니다.
    3 사용자는 서버에서 해당 세션ID를 받아 쿠키에 저장을 한 후, 인증이 필요한 요청마다 쿠키를 헤더에 실어 보냅니다.
    4. 서버에서는 쿠키를 받아 세션 저장소에서 대조를 한 후 대응되는 정보를 가져옵니다.
    5. 인증이 완료되고 서버는 사용자에 맞는 데이터를 보내줍니다.
세션 쿠키 방식의 인증은 기본적으로 세션 저장소를 필요로 한다. 세션 저장소는 로그인을 했을 때 사용자의 정보를 저장하고 열쇠가 되는 세션 ID값을 만든다. 그리고 HTTP 헤더에 실어 사용자에게 돌려보낸다. 그러면 사용자는 쿠키로 보관하고 있다가 인증이 필요한 요청에 쿠키(세션ID)를 넣어 보낸다. 
* 세션ID를 쿠키라고 봐도 동일, 쿠키가 사용자 개념에서 더 큰 범주. 세션ID를 쿠키로 저장.


2. 장점
    * 세션/쿠키 방식은 기본적으로 쿠키를 매개로 인증을 거칩니다. 여기서 쿠키는 세션 저장소에 담긴 유저 정보를 얻기 위한 열쇠라고 보시면 됩니다. 
    * 따라서 쿠키가 담긴 HTTP 요청이 도중에 노출되더라도 쿠키 자체(세션 ID)는 유의미한 값을 갖고있지 않습니다(중요 정보는 서버 세션에) 이는 위의 계정정보를 담아 인증을 거치는 것보단 안전해 보입니다. 

 - 쿠키에 담긴 HTTP 요청이 도중에 노출이 되더라도 쿠키 자체(세션 ID)는 유의미한 값을 갖고 있지 않기 때문에 안전합니다. 
 - 일일이 회원정보를 확인할 필요가 없어 바로 어떤 회원인지를 확인할 수 있어서 서버의 자원에 접근하기 용이합니다. 


3. 단점
    * 쿠키 탈취 자체는 안전하지만, 만약에 HTTP 요청을 해커가 가로챈다면 그 안에 있는 쿠키를 가로채서 HTTP 요청을 보낼 수 있습니다. 
    * (세션 하이재킹 공격) => HTTPS 사용, 세션에 유효기간을 넣어주기.
    * 서버에서 세션 저장소를 사용한다고 하기에 추가적인 저장공간을 필요로 하게되고 자연스럽게 부하도 높아질 것이다. 

### JWT

* 인증에 필요한 정보들을 암호화시킨 토큰을 말한다. Access Token을 HTTP 헤더에 실어 서버로 보내게 된다.
* JWT 구성요소
  1. Header : 위 3가지 정보를 암호화할 방식(alg), 타입(type) 등이 들어갑니다. (agl:HS256, type:jwt)
  2. payload : 서버에 보내질 데이터. 유저 아이디, 유효기간  등이 들어갑니다.
  3. Verify Signature : Base64로 인코딩한 Header와 Payload, SECRET KEY

* 장점
  1. 간편합니다. 세션/쿠키는 별도의 저장소의 관리가 필요합니다. 그러나 JWT는 발급한 후 검증만 하면 되기 때문에 추가 저장소가 필요 없습니다.
  이는 Stateless 한 서버를 만드는 입장에서는 큰 강점입니다. 여기서 Stateless는 어떠한 별도의 저장소도 사용하지 않는, 즉 상태를 저장하지 않는 것을 의미합니다. 
  이는 서버를 확장하거나 유지,보수하는데 유리합니다.
  
  2. 확장성이 뛰어납니다. 토큰 기반으로 하는 다른 인증 시스템에 접근이 가능합니다. 예를 들어 Facebook 로그인, Google 로그인 등은 모두 토큰을 기반으로 인증을 합니다.
  이에 선택적으로 이름이나 이메일 등을 받을 수 있는 권한도 받을 수 있습니다. 

* 단점
  1. 이미 발급된 JWT에 대해서는 돌이킬 수 없습니다. 세션/쿠키의 경우 만일 쿠키가 악의적으로 이용된다면, 해당하는 세션을 지워버리면 됩니다. 하지만 JWT는 한 번 발급되면 유효기간이 완료될 때 까지는 계속 사용이 가능합니다. 따라서 악의적인 사용자는 유효기간이 지나기 전까지 정보 탈취가 가능합니다.
-> 해결책 : 기존의 Access Token의 유효기간을 짧게 하고 Refresh Token이라는 새로운 토큰을 발급합니다. 그렇게 되면 Access Token을 탈취당해도 상대적으로 피해를 줄일 수 있습니다. 

  2. Payload 정보가 제한적입니다. 위에서 언급했다시피 Payload는 따로 암호화되지 않기 때문에 디코딩하면 누구나 정보를 확인할 수 있습니다. (세션/쿠키 방식에서는 유저의 정보가 전부 서버의 저장소에 안전하게 보관됩니다) 따라서 유저의 중요한 정보들은 Payload에 넣을 수 없습니다.
  3. JWT의 길이입니다. 세션/쿠키 방식에 비해 JWT의 길이는 깁니다. 따라서 인증이 필요한 요청이 많아질 수록 서버의 자원낭비가 발생하게 됩니다.
 
 
### 쿠키와 세션과 JWT 차이

세션/쿠키 방식과 가장 큰 차이점은 세션/쿠키는 세션 저장소에 유저의 정보를 넣는 반면, JWT는 토큰 안에 유저의 정보들이 넣는다는 점입니다. 물론 클라이언트 입장에서는 HTTP 헤더에 세션ID나 토큰을 실어서 보내준다는 점에서는 동일하나, 서버 측에서는 인증을 위해 암호화를 하냐, 별도의 저장소를 이용하냐는 차이가 발생합니다.

> -[https://tansfil.tistory.com/58](https://tansfil.tistory.com/58)
