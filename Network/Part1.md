# 2. Network
**:book: Contents**
* [OSI 7계층](#OSI-7계층)
* [TCP/IP의 개념](#TCPIP의-개념)
* [TCP와 UDP](#TCP와-UDP)
* [TCP(흐름제어/혼잡제어)](#TCP흐름제어-혼잡제어)
* [TCP와 UDP의 헤더 분석](#TCP와-UDP의-헤더-분석)
* [TCP의 3-way-handshake와 4-way-handshake](#TCP의-3-way-handshake와-4-way-handshake)


---

### OSI 7계층

**OSI(Open Systems Interconnection Reference Model)란**

![OSI 7계층](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/osi-7-layer.png)

* 국제표준화기구(ISO)에서 개발한 모델로, **컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명**한 것이다.
* 이 모델은 프로토콜을 기능별로 나눈 것이다.
* 각 계층은 하위 계층의 기능만을 이용하고, 상위 계층에게 기능을 제공한다.
* 일반적으로 하위 계층들은 하드웨어로, 상위 계층들은 소프트웨어로 구현된다. 

**7계층은 왜 나눌까?**

통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문이다.

**데이터의 계층별 이름(PDU, 데이터의 전송 단위)**
* Application / Presentation / Session : Data
* Transport : Segment
* Network : Packet
* Data Linke : Frame
* Physical : Bit

**1) 물리(Physical)**

> 리피터, 케이블, 허브 등
단지 데이터 전기적인 신호로 변환해서 주고받는 기능을 진행하는 공간

즉, 데이터를 전송하는 역할만 진행한다.



**2) 데이터 링크(Data Link)**

> 브릿지, 스위치 등
물리 계층으로 송수신되는 정보를 관리하여 안전하게 전달되도록 도와주는 역할

Mac 주소를 통해 통신한다. 프레임에 Mac 주소를 부여하고 에러검출, 재전송, 흐름제어를 진행한다.


**3) 네트워크(Network)**

> 라우터, IP
데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당한다.

라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달해준다.

라우팅, 흐름 제어, 오류 제어, 세그먼테이션 등을 수행한다.


**4) 전송(Transport)**


> TCP, UDP
TCP와 UDP 프로토콜을 통해 통신을 활성화한다. 포트를 열어두고, 프로그램들이 전송을 할 수 있도록 제공해준다.

TCP : 신뢰성, 연결지향적

UDP : 비신뢰성, 비연결성, 실시간


**5) 세션(Session)**

> API, Socket
데이터가 통신하기 위한 논리적 연결을 담당한다. TCP/IP 세션을 만들고 없애는 책임을 지니고 있다.


**6) 표현(Presentation)**

> JPEG, MPEG 등
데이터 표현에 대한 독립성을 제공하고 암호화하는 역할을 담당한다.

파일 인코딩, 명령어를 포장, 압축, 암호화한다.


**7) 응용(Application)**

> HTTP, FTP, DNS 등
최종 목적지로, 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.

사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스를 제공한다.



### TCP/IP의 개념

- 프로토콜
컴퓨터 간의 주고받는 메시지를 전송할 때 데이터 통신을 원활하게 하기 위해 필요한 통신 규약

- TCP/IP 프로토콜
  - 개방형 프로토콜의 표준. 특정 하드웨어나 OS에 독립적으로 사용하는 것이 가능. 
  - 인터넷에서 서로 다른 시스템을 가진 컴퓨터들을 서로 연결하고, 데이터를 전송하는데 사용하는 통신 프로토콜로 근거리 및 원거리 모두에 사용할 수 있음
  - 인터넷 프로토콜 중 가장 중요한 역할을 하는 TCP와 IP의 합성어
  - 인터넷 동작의 중심이 되는 통신규약
  - 데이터의 흐름 관리, 데이터의 정확성 확인(TCP의 역할), 패킷을 목적지까지 전송하는 역할(IP역할)을 담당
    - TCP : 전체 데이터가 잘 전송될 수 있도록 데이터의 흐름을 조절하고 성공적으로 상대편 컴퓨터에 도착할 수 있도록
    보장하는 역할
    - IP : 데이터를 한 장소에서 다른 장소로 정확하게 옮겨주는 역할
- TCP/IP 4계층

  ![TCP/IP Protocol Suite](https://mblogthumb-phinf.pstatic.net/20161012_16/mes194_1476236745637fKg86_PNG/%C4%C4%B3%D71.PNG?type=w800)
  - 응용 계층(Application Layer)
     - 사용자 응용 프로그램으로부터 요청을 받아서 이를 적절한 메시지로 변환하고 하위계층으로 전달하는 역할
     - TCP/UDP 기반의 응용 프로그램을 구현할 때 사용
     > 프로토콜 : FTP, HTTP, SSH
  - 전송 계층(Transport Layer)
    - IP에 의해 전달되는 패킷의 오류를 검사하고 재전송을 요구하는 등의 제어를 담당하는 계층
    - 통신 노드 간의 연결을 제어하고, 신뢰성 있는 데이터 전송을 담당
    > 프로토콜 : TCP, UDP
  - 인터넷 계층(Internet Layer)
    - 통신 노드 간의 IP패킷을 전송하는 기능과 라우팅 기능을 담당
      - 라우팅 : 어떤 네트워크 안에서 통신 데이터를 보낼 때 최적의 경로를 선택하는 과정
    - 전송 계층에서 받은 패킷을 목적지까지 효율적으로 전달하는 것만 고려.
    > 프로토콜 : IP, ARP, RARP
  - 네트워크 인터페이스 계층(Network Interface Layer)
    - OSI 7계층의 물리계층과 데이터 링크 계층에 해당
    - 물리적인 주소로 MAC을 사용
    - 특정 프로토콜을 규정하지 않고, 모든 표준과 기술적인 프로토콜을 지원하는 계층으로서 **프레임**을 물리적인 회선에 올리거나 내려받는 역할을 담당
    - LAN, 패킷망 등에 사용된다. 
    

### TCP와 UDP     
  
* 네트워크 계층 중 **전송 계층에서 사용하는 프로토콜**
* TCP(Transmission Control Protocol)  
    ![가상회선 방식](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/tcp-virtual-circuit.png)
    * 인터넷 상에서 데이터를 메세지의 형태(**세그먼트** 라는 블록 단위)로 보내기 위해 IP와 함께 사용하는 프로토콜이다.
    * TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리한다.
    * **연결형 서비스로** 가상 회선 패킷 교환 방식을 제공한다.
        * 가상회선 : 데이터를 전송하기 전에 논리적 연결이 설정(연결 지향형)
        * 패킷에는 가상회선 식별 번호(VCI)가 포함되고, 모든 패킷을 전송하면 가상회선이 해제되고 
        패킷은 전송된 순서대로 도착한다. 
        * 3-way handshaking과정을 통해 연결을 설정하고, 4-way handshaking을 통해 연결을 해제한다.
    * 흐름제어 및 혼잡제어를 제공한다.
        * 흐름제어
            * 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것
            * 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 막는다.
        * 혼잡제어
            * 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것
            * 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막는다.
    * 높은 신뢰성을 보장한다.
    * UDP보다 속도가 느리다.
    * 전이중(Full-Duplex), 점대점(Point to Point) 방식이다.
        * 전이중
            * 전송이 양방향으로 동시에 일어날 수 있다.
        * 점대점
            * 각 연결이 정확히 2개의 종단점을 가지고 있다.
        * 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.
          * 브로드캐스팅 : 불특정 다수를 대상을 데이터를 전송
          * 멀티캐스팅 : 수신 대상이 정해져 있음
          * 공통점 : 동시에 많은 사람들에게 똑같은 데이터를 전송
          * 차이점 : 특정 다수, 불특정 다수의 차이 
    * 연속성보다 신뢰성있는 전송이 중요할 때에 사용된다.
* UDP(User Datagram Protocol)  
    ![데이터그램 패킷 교환 방식](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/udp-datagram.png)
    * 데이터를 **데이터그램** 단위로 처리하는 프로토콜이다.
    * **비연결형 서비스로** 데이터그램 방식을 제공한다.
        * 연결을 위해 할당되는 논리적인 경로가 없다.
        * 그렇기 때문에 각각의 패킷은 다른 경로로 전송되고, 각각의 패킷은 독립적인 관계를 지니게 된다.
        * 이렇게 데이터를 서로 다른 경로로 독립적으로 처리한다.
    * 송신 측에서 전송한 순서와 수신 측에서 도착한 순서가 다를 수 있다.
    * 정보를 주고 받을 때 정보를 보내거나 받는다는 신호절차를 거치지 않는다.
    * UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
    * 신뢰성이 낮다.
    * TCP보다 속도가 빠르다.
    * 신뢰성보다는 연속성이 중요한 서비스, 예를 들면 실시간 서비스(streaming)에 사용된다.
* 참고
  * UDP와 TCP는 각각 별도의 포트 주소 공간을 관리하므로 같은 포트 번호를 사용해도 무방하다. 즉, 두 프로토콜에서 동일한 포트 번호를 할당해도 서로 다른 포트로 간주한다.
  * 또한 같은 모듈(UDP or TCP) 내에서도 클라이언트 프로그램에서 동시에 여러 커넥션을 확립한 경우에는 서로 다른 포트 번호를 동적으로 할당한다. (동적할당에 사용되는 포트번호는 49,152~65,535이다.)
* 가상회선 방식 vs 데이터그램 방식 
   * 정해진 시간 안이나 다량의 데이터를 **연속**으로 보낼 때는 가상회선 방식이 적합하다.
   * 짧은 메시지의 일시적인 전송에는 데이터그램 방식이 적합하다
   * 네트워크 내의 한 노드가 다운되면 데이터그램 방식은 다른 경로를 새로 설정하지만,
   * 가상회선 방식은 그 노드를 지나는 모든 가상회선을 잃게 된다. 
* Ref
  * [데이터그램 패킷 교환 vs 가상회선 패킷 교환](https://gotwo.tistory.com/107)
  * [데이터그램 & 가상회선(Virtual Circuit)](https://security-nanglam.tistory.com/179)

### TCP흐름제어 혼잡제어
- TCP 통신이란?
  - 네트워크 통신에서 신뢰적인 연결방식
  - TCP는 기본적으로 unreliable network에서, reliable network를 보장할 수 있도록 하는 프로토콜
  - TCP는 network congestion avoidance algorithm을 사용
- reliable network를 보장한다는 것은 4가지 문제점 존재
  - 손실 : packet이 손실될 수 있는 문제
  - 순서 바뀜 : packet의 순서가 바뀌는 문제
  - Congestion : 네트워크가 혼잡한 문제
  - Overload : receiver가 overload 되는 문제
- 흐름제어/혼잡제어란?
  - 흐름제어 (endsystem 대 endsystem)
    - 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
    - Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것
    - 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점
  - 혼잡제어 : 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법
- 전송의 전체 과정
  - Application layer : sender application layer가 socket에 data를 씀.
  - Transport layer : data를 segment에 감싼다. 그리고 network layer에 넘겨줌.
  - 그러면 아랫단에서 어쨋든 receiving node로 전송이 됨. 이 때,  sender의 send buffer에 data를 저장하고, receiver는 receive buffer에 data를 저장함.
  - application에서 준비가 되면 이 buffer에 있는 것을 읽기 시작함.
  - 따라서 flow control의 핵심은 이 receiver buffer가 넘치지 않게 하는 것임.
  - 따라서 receiver는 RWND(Receive WiNDow) : receive buffer의 남은 공간을 홍보함

#### 1. 흐름제어 (Flow Control)

- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제없지만, 송신측의 속도가 빠를 경우 문제가 생긴다.

- 수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번히 발생한다.

- 이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야한다.

- 해결방법

  - Stop and Wait : 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

    - <img src='https://t1.daumcdn.net/cfile/tistory/263B7D4E5715ECEB32'>

  - Sliding Window (Go Back N ARQ) 

    - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어기법

    - 목적 : 전송은 되었지만, acked를 받지 못한 byte의 숫자를 파악하기 위해 사용하는 protocol

      LastByteSent - LastByteAcked <= ReceivecWindowAdvertised

      (마지막에 보내진 바이트 - 마지막에 확인된 바이트 <= 남아있는 공간) ==

      (현재 공중에 떠있는 패킷 수 <= sliding window)

  - 동작방식 : 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송

    - <img src='https://t1.daumcdn.net/cfile/tistory/253F7E485715ED5F27'>

  - Window : TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 '3 way handshaking'을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 된다.

  - 세부구조

    1. 송신 버퍼
       - <img src='https://t1.daumcdn.net/cfile/tistory/22532F485715EDF218'>- 
       - 200 이전의 바이트는 이미 전송되었고, 확인응답을 받은 상태
       - 200 ~ 202 바이트는 전송되었으나 확인응답을 받지 못한 상태
       - 203 ~ 211 바이트는 아직 전송이 되지 않은 상태
    2. 수신 윈도우
       - <img src='https://t1.daumcdn.net/cfile/tistory/25403A485715EE362B'>
    3. 송신 윈도우
       - <img src='https://t1.daumcdn.net/cfile/tistory/2520244B5715EE6A14'>
       - 수신 윈도우보다 작거나 같은 크기로 송신 윈도우를 지정하게되면 흐름제어가 가능하다.
    4. 송신 윈도우 이동
       - <img src='https://t1.daumcdn.net/cfile/tistory/227DC8505715EEBA0A'>
       -  Before : 203 ~ 204를 전송하면 수신측에서는 확인 응답 203을 보내고, 송신측은 이를 받아 after 상태와 같이 수신 윈도우를 203 ~ 209 범위로 이동
       - after : 205 ~ 209가 전송 가능한 상태
    5. Selected Repeat

<br>

#### 2. 혼잡제어 (Congestion Control)

- 송신측의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달된다. 만약 한 라우터에 데이터가 몰릴 경우, 자신에게 온 데이터를 모두 처리할 수 없게 된다. 이런 경우 호스트들은 또 다시 재전송을 하게되고 결국 혼잡만 가중시켜 오버플로우나 데이터 손실을 발생시키게 된다. 따라서 이러한 네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송속도를 강제로 줄이게 되는데, 이러한 작업을 혼잡제어라고 한다.
- 또한 네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라 하며, 혼잡 현상을 방지하거나 제거하는 기능을 혼잡제어라고 한다.
- 흐름제어가 송신측과 수신측 사이의 전송속도를 다루는데 반해, 혼잡제어는 호스트와 라우터를 포함한 보다 넓은 관점에서 전송 문제를 다루게 된다.
- 해결 방법
  - <img src='https://t1.daumcdn.net/cfile/tistory/256E39425715F10103'>
  - AIMD(Additive Increase / Multicative Decrease)
    - 처음에 패킷을 하나씩 보내고 이것이 문제없이 도착하면 window 크기(단위 시간 내에 보내는 패킷의 수)를 1씩 증가시켜가며 전송하는 방법
    - 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷의 보내는 속도를 절반으로 줄인다.
    - 공평한 방식으로, 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형상태로 수렴하게 되는 특징이 있다.
    - 문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지하지 못한다. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식이다.
  - Slow Start (느린 시작)
    - AIMD 방식이 네트워크의 수용량 주변에서는 효율적으로 작동하지만, 처음에 전송 속도를 올리는데 시간이 오래 걸리는 단점이 존재했다.
    - Slow Start 방식은 AIMD와 마찬가지로 패킷을 하나씩 보내면서 시작하고, 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size를 1씩 늘려준다. 즉, 한 주기가 지나면 window size가 2배로 된다. 
    - 전송속도는 AIMD에 반해 지수 함수 꼴로 증가한다. 대신에 혼잡 현상이 발생하면 window size를 1로 떨어뜨리게 된다.
    - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느 정도 예상할 수 있다. 
    - 그러므로 혼잡 현상이 발생하였던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후부터는 완만하게 1씩 증가시킨다.
  - Fast Retransmit (빠른 재전송)
    - 빠른 재전송은 TCP의 혼잡 조절에 추가된 정책이다. 
    - 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보내게 된다. 
    - 단, 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보내게 되므로, 중간에 하나가 손실되게 되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 된다. 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있다.
    - 중복된 순번의 패킷을 3개 받으면 재전송을 하게 된다. 약간 혼잡한 상황이 일어난 것이므로 혼잡을 감지하고 window size를 줄이게 된다.
  - Fast Recovery (빠른 회복)
    - 혼잡한 상태가 되면 window size를 1로 줄이지 않고 반으로 줄이고 선형증가시키는 방법이다. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 된다.

<br>

[ref]<br>

- <https://www.brianstorti.com/tcp-flow-control/>
- <https://www.brianstorti.com/tcp-flow-control/>

### TCP와 UDP의 헤더 분석

  * TCP 헤더 구조(TCP는 양방향성)
  * ![TCP 헤더](https://idchowto.com/wp-content/uploads/2015/11/TCP%ED%8C%A8%ED%82%B7.png)
  
  * UDP 헤더 구조
  * ![UDP 헤더](https://idchowto.com/wp-content/uploads/2015/11/UDP.png)

  > - [https://idchowto.com/?p=18352](https://idchowto.com/?p=18352)
  > - [https://m.blog.naver.com/PostView.nhn?blogId=koromoon&logNo=120162515270&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F](https://m.blog.naver.com/PostView.nhn?blogId=koromoon&logNo=120162515270&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)

### TCP의 3 way handshake와 4 way handshake
* TCP는 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여 연결을 설정하여 **신뢰성을 보장하는 연결형 서비스** 이다.
* 3-way handshake 란
  * TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 **연결을 설정(Connection Establish)** 하는 과정
  * 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
  * 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.
      * A 프로세스(Client)가 B 프로세스(Server)에 연결을 요청
        ![3 way handshake](https://t1.daumcdn.net/cfile/tistory/267C5D39537EDD2C17)
        먼저 Server에서 열려있는 포트는 LISTEN 상태이고 Client에서는 Closed 상태이다.
        1. Client에서 Server에 연결 요청을 하기위해 SYN 데이터를 보낸다.
        2. Server에서 해당 포트는 LISTEN 상태에서 SYN 데이터를 받고 SYN_RCV로 상태가 변경된다.
        그리고 요청을 정상적으로 받았다는 대답(ACK)와 Client도 포트를 열어달라는 SYN 을 같이 보낸다.
        3. Client에서는 SYN+ACK 를 받고 ESTABLISHED로 상태를 변경하고 서버에 요청을 잘 받았다는 ACK 를 전송한다.
        ACK를 받은 서버는 상태가 ESTABLSHED로 변경된다.
        
        위와 같이 3번의 통신이 정상적으로 이루어지면, 서로의 서로의 포트가 ESTABLISHED 되면서 연결이 되게 된다.
        
      * Status
        * Closed : 닫힌 상태
        * LISTEN : 포트가 열린 상태로 연결 요청 대기 중
        * SYN_RCV : SYNC 요청을 받고 상대방의 응답을 기다리는 중
        *  ESTABLISHED : 포트 연결 상태


* 4-way handshake 란
  * TCP의 **연결을 해제(Connection Termination)** 하는 과정
  * TCP 연결을 위해 3 Way handshake를 통해 ESTBLISHED 하는 것과 달리 프로세스 간 연결을 종료할 때는 4 Way
  handshake를 수행한다.
  * 아무래도 새로 연결하는 것보다, 연결되어 있는 상황에서 종료할 때 예기치 못한 상황이 많기 때문에, 확인 과정이 더 까다롭다.
  ![4 way handshake](https://t1.daumcdn.net/cfile/tistory/2678E035537EEE9126)
    * 최초에는 서로 통신 상태이기 때문에 양쪽이 ESTABLISHED 상태이다.
     1. 통신을 종료하고자 하는 Client가 서버에게 FIN 패킷을 보내고 자신은 FIN_WAIT_1 상태로 대기한다.
     2. FIN 패킷을 받은 서버는 해당 포트를 CLOSE_WAIT으로 바꾸고 잘 받았다는 ACK 를 Client에게 전하고 ACK를 받은 Client는 상태를 FIN_WAIT_2로 변경한다.
     그와 동시에 Server에서는 해당 포트에 연결되어 있는 Application에게 Close()를 요청한다.
     3. Close() 요청을 받은 Application은 종료 프로세스를 진행시켜 최종적으로 close()가 되고 server는 FIN 패킷을 Client에게 전송 후 자신은 LAST_ACK 로 상태를 바꾼다.
     4. FIN_WAIT_2 에서 Server가 연결을 종료했다는 신호를 기다리다가 FIN 을 받으면 받았다는 ACK를 Server에 전송하고 자신은 TIME_WAIT 으로 상태를 바꾼다. (TIME_WAIT 에서 일정 시간이 지나면 CLOSED 되게 된다.)
     최종 ACK를 받은 서버는 자신의 포트도 CLOSED로 닫게 된다.

     TCP 연결 종료는 연결보다 더 복잡하고 예기치 못한 상황들이 많이 일어난다.
    * 비정상 종료 상황
    다양한 상황에 따른 연결의 종료를 적절하게 처리하지 못하면, FIN_WAIT_1, FIN_WAIT_2, CLOSE_WAIT 상태로 남아 계속 기다리는 상황이 올 수 있다.
      - CLOSE_WAIT 상태 : 어플리케이션에서 close()를 적절하게 처리해주지 못하면, TCP 포트는 CLOSE_WAIT 상태로 계속 기다리게 된다. 이렇게 CLOSE_WAIT 상태가 statement에 많아지게 되면, Hang 이 걸려 더이상 연결을 하지 못하는 경우가 생기기도 한다. 따라서 어플리케이션 개발시 여러 상황에 따라 close() 처리를 잘 해줘야 한다.
      - FIN_WAIT_1 상태 : FIN_WAIT_1 상태라는 것은 상대방측에 커넥션 종료 요청을 했는데, ACK를 받지 못한 상태로 기다리고 있는 것이다. 이것은 아마 서버를 찾을 수 없는 것으로, 네트워크 및 방화벽의 문제일 수 있다.
      (FIN_WAIT_1 의 상태는 일정 시간이 지나 Time Out이 되면 자동으로 닫는다.)
      - FIN_WAIT_2 상태 : FIN_WAIT_2 상태는 클라이언트가 서버에 종료를 요청한 후 서버에서 요청을 접수했다고 ACK를 받았지만, 서버에서 종료를 완료했다는 FIN 을 받지 못하고 기다리고 있는 상태이다. 이상태는 양방의 두번의 통신이 이루어졌기 때문에 네트워크의 문제는 아닌 것으로 판단되며,(FIN 을 보내는 순간에 순단이 있어 못받은 것일 수도 있다.) 서버측에서 CLOSE를 처리하지 못하는 경우일 수도 있다. FIN_WAIT_2 역시 일정시간 후 Time Out이 되면 스스로 Closed 하게 된다.
      - 어떠한 이유에서 FIN_WAIT_1과 FIN_WAIT_2 상태인 연결이 많이 남아있다면, 문제가 발생할 수 있다. 물론 일정 시간이 지나 Time Out이 되면 연결이 자동으로 종료되긴 하지만, 이 Time Out이 길어서 많은 수의 소켓이 늘어만 난다면, 메모리 부족으로 더 이상 소켓을 오픈하지 못하는 경우가 발생한다.
      (이 경우는 네트워크나 방화벽 또는 어플리케이션에서 close() 처리 등에 대한 문제등으로 발생할 수 있으며, 원인을 찾기가 쉽지 않다.)
      - 이러한 문제 해결을 위해서 FIN_WAIT_1과 FIN_WAIT_2 의 Time Out 시간을 적절히 조절할 필요가 있다.

  * 참고 - ***플래그 정보***
    * TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
      * 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
    * SYN(Synchronize Sequence Number) / 000010
      * 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
    * ACK(Acknowledgement) / 010000
      * 응답 확인. 패킷을 받았다는 것을 의미한다.
      * Acknowledgement Number 필드가 유효한지를 나타낸다.
      * 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
    * FIN(Finish) / 000001
      * 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.
  * Ref : [4-way-handshake](https://hyeonstorage.tistory.com/287?category=583737)

#### :question:TCP 관련 질문 1
* Q. TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유?
  * A. Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있기 때문에 일단 FIN에 대한 ACK만 보내고, 데이터를 모두 전송한 후에 자신도 FIN 메시지를 보내기 때문이다.
  * [관련 Reference](http://ddooooki.tistory.com/21)

#### :question:TCP 관련 질문 2
* Q. 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?
  * A. 이러한 현상에 대비하여 Client는 Server로부터 FIN 플래그를 수신하더라도 일정시간(Default: 240sec)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. (TIME_WAIT 과정)
  * [관련 Reference](http://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake)

#### :question:TCP 관련 질문 3
* Q. 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?
  * A. Connection을 맺을 때 사용하는 포트(Port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다. 서버 측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순처적인 Number가 전송된다면 이전의 Connection으로부터 오는 패킷으로 인식할 수 있다. 이런 문제가 발생할 가능성을 줄이기 위해서 난수로 ISN을 설정한다.
  * [관련 Reference](http://asfirstalways.tistory.com/356)
