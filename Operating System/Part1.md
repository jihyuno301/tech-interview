# 3. Operating System - Part1
### :book: Contents
* [프로세스와 스레드의 차이(Process vs Thread)](#프로세스와-스레드의-차이)
* [멀티 프로세스 대신 멀티 스레드를 사용하는 이유](#멀티-프로세스-대신-멀티-스레드를-사용하는-이유)
* [Thread-safe](#thread-safe)
* [동기화 객체의 종류](#동기화-객체의-종류)
* [뮤텍스와 세마포어의 차이](#뮤텍스와-세마포어의-차이)
* [CPU 스케줄링](#CPU-스케줄링)


---
### 프로세스와 스레드의 차이
* 프로그램(Program) 이란
  * 사전적 의미: 어떤 작업을 위해 실행할 수 있는 파일
* 프로세스(Process) 란
    * 메모리에 올라와 **실행되고 있는 프로그램의 인스턴스(독립적인 개체)**
    * 운영체제로부터 시스템 자원을 할당받는 작업의 단위
    * 즉, 동적인 개념으로는 실행중인 프로그램을 의미한다.
  * 할당받는 시스템 자원의 예
    * CPU 시간
    * 운영되기 위해 필요한 주소 공간
    * Code, Data, Stack, Heap의 구조로 되어 있는 독립된 메모리 영역
  * 특징
    * ![](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/process.png)
    * 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받는다.
      * Code : 코드 자체를 구성하는 메모리 영역(프로그램 명령)
      * Data : 전역변수, 정적변수, 배열 등 (초기화된 데이터)
      * Heap : 동적 할당 시 사용 (new(), mallock() 등)
      * Stack : 지역변수, 매개변수, 리턴 값 (임시 메모리 영역)
    * 기본적으로 프로세스당 최소 1개의 스레드(메인 스레드)를 가지고 있다.
    * 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
    * 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용해야 한다. (Ex. 파이프, 파일, 소켓 등을 이용한 통신 방법 이용)
* 스레드(Thread) 란
    * 프로세스 안에서 실행되는 여러 흐름의 단위
    * **프로세스의 특정한 수행 경로**
    * 프로세스가 할당받은 자원을 이용하는 실행의 단위
  * 특징
    * ![](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/thread.png)
    * 스레드는 프로세스 내에서 각각 Stack만 따로 할당받고 Code, Data, Heap 영역은 공유한다.
    * 스레드는 한 프로세스 내에서 동작되는 여러 실행의 흐름으로, 프로세스 내의 주소 공간이나 자원들(힙 공간 등)을 같은 프로세스 내에 스레드끼리 공유하면서 실행된다.
    * 같은 프로세스 안에 있는 여러 스레드들은 같은 힙 공간을 공유한다. 반면에 프로세스는 다른 프로세스의 메모리에 직접 접근할 수 없다.
    * 각각의 스레드는 별도의 레지스터와 스택을 갖고 있지만, 힙 메모리는 서로 읽고 쓸 수 있다.
    * 한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드(sibling thread)도 그 변경 결과를 즉시 볼 수 있다.
* 자바 스레드(Java Thread) 란
  * 일반 스레드와 거의 차이가 없으며, JVM가 운영체제의 역할을 한다.
  * 자바에는 프로세스가 존재하지 않고 스레드만 존재하며, 자바 스레드는 JVM에 의해 스케줄되는 실행 단위 코드 블록이다.
  * 자바에서 스레드 스케줄링은 전적으로 JVM에 의해 이루어진다.
  * 아래와 같은 스레드와 관련된 많은 정보들도 JVM이 관리한다.
    * 스레드가 몇 개 존재하는지
    * 스레드로 실행되는 프로그램 코드의 메모리 위치는 어디인지
    * 스레드의 상태는 무엇인지
    * 스레드 우선순위는 얼마인지
  * 즉, 개발자는 자바 스레드로 작동할 스레드 코드를 작성하고, 스레드 코드가 생명을 가지고 실행을 시작하도록 JVM에 요청하는 일 뿐이다.
  
**💡 프로세스는 자신만의 고유 공간과 자원을 할당받아 사용하는데 반해, 스레드는 다른 스레드와 공간, 자원을 공유하면서 사용하는 차이가 존재함**


> - [https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html](https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html)
> - [https://brunch.co.kr/@kd4/3](https://brunch.co.kr/@kd4/3)
> - [https://magi82.github.io/process-thread/](https://magi82.github.io/process-thread/)
> - [https://jaybdev.net/2017/06/05/Java-3/](https://jaybdev.net/2017/06/05/Java-3/)
> - [http://includestdio.tistory.com/6](http://includestdio.tistory.com/6)
> - [https://lalwr.blogspot.com/2016/02/process-thread.html](https://lalwr.blogspot.com/2016/02/process-thread.html)

### 멀티 프로세스 대신 멀티 스레드를 사용하는 이유

* **멀티 프로세스**
> 하나의 컴퓨터에 여러 CPU 장착 → 하나 이상의 프로세스들을 동시에 처리(병렬)
  * 장점 : 안전성 (메모리 침범 문제를 OS 차원에서 해결)
  * 단점 : 각각 독립된 메모리 영역을 갖고 있어, 작업량 많을 수록 오버헤드 발생. Context Switching으로 인한 성능 저하
    * Context Switching
      * 프로세스의 상태 정보를 저장하고 복원하는 일련의 과정
      * 즉, 동작 중인 프로세스가 대기하면서 해당 프로세스의 상태를 보관하고, 대기하고 있던 다음 순번의 프로세스가 동작하면서 이전에 보관했던 프로세스 상태를 복구하는 과정을 말함
      * 프로세스는 각 독립된 메모리 영역을 할당받아 사용되므로, 캐시 메모리 초기화와 같은 무거운 작업이 진행되었을 때 오버헤드가 발생할 문제가 존재함
        * 오버헤드(overhead) : 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간/메모리 등을 말한다.
        * 예) A를 단순하게 처리하면 10초 걸리는데, 안전성을 고려하고 부가적인 B를 처리한 결과 처리시간이 15초 걸렸다면 오버헤드는 5초가 된다. 만약 B를 처리하는 것을 개선해 12초가 
        걸렸다면 오버헤드가 3초 단축되었다고 한다.
* **멀티 스레드**
> 하나의 응용 프로그램에서 여러 스레드를 구성해 각 스레드가 하나의 작업을 처리하는 것
> 스레드들이 공유 메모리를 통해 다수의 작업을 동시에 처리하도록 해줌

  * 장점 : 독립적인 프로세스에 비해 공유 메모리만큼의 시간, 자원 손실이 감소. 전역 변수와 정적 변수에 대한 자료 공유 가능
  * 단점 : 안전성 문제. 하나의 스레드의 데이터 공간이 망가지면, 모든 스레드가 작동 불능 상태가 된다. (공유 메모리를 갖기 때문)
    * 안전성에 대한 단점은 Critical Section 기법을 통해 대비함
      * 하나의 스레드가 공유 데이터 값을 변경하는 시점에 다른 스레드가 그 값을 읽으려할 때 발생하는 문제를 해결하기 위한 **동기화**과정
      * 단, 상호 배제, 진행, 한정된 대기를 충족해야 함.
<br>

💡 **멀티 프로세스 대신 멀티 스레드를 사용하는 이유** 
<br>
 프로그램을 여러 개 키는 것보다 하나의 프로그램 안에서 여러 작업을 해결하는 것이 더 낫기 때문이다.

  * ![](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/multi-thread.png)

  1. 자원의 효율성 증대
      * 멀티 프로세스로 실행되는 작업을 멀티 스레드로 실행할 경우, **프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어** 자원을 효율적으로 관리할 수 있다.
          * 프로세스 간의 Context Switching시 단순히 CPU 레지스터 교체 뿐만 아니라 RAM과 CPU 사이의 캐시 메모리에 대한 데이터까지 초기화되므로 오버헤드가 크기 때문
      * 스레드는 프로세스 내의 메모리를 공유하기 때문에 독립적인 프로세스와 달리 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어들게 된다.
  2. 처리 비용 감소 및 응답 시간 단축
      * 또한 프로세스 간의 통신(IPC)보다 스레드 간의 통신의 비용이 적으므로 작업들 간의 통신의 부담이 줄어든다.
          * 스레드는 Stack 영역을 제외한 모든 메모리를 공유하기 때문
      * 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 빠르다.
          * Context Switching시 스레드는 Stack 영역만 처리하기 때문

* ***주의할 점!***
  * **동기화 문제**
  * 스레드 간의 자원 공유는 전역 변수(데이터 세그먼트)를 이용하므로 함께 상용할 때 충돌이 발생할 수 있다.

> [http://you9010.tistory.com/136](http://you9010.tistory.com/136)


### Thread-safe
- **Thread-safe란?**
    - 멀티스레드 환경에서 **여러 스레드**가 동시에 <u>하나의 객체 및 변수(공유 자원)에 접근</u>할 때, 의도한 대로 동작하는 것을 말한다.
    - 이러한 상황을 "Thead-safe하다" 라고 표현한다.
- **Thread-safe하게 구현하기**
    - Thread-safe하기 위해서는 공유 자원에 접근하는 임계영역(critical section)을 동기화 기법으로 제어해줘야 한다.
        - 이를 '상호배제'라고 한다.
    - 동기화 기법으로는 Mutex나 Semaphore 등이 있다.
- **Reentrant**
    - Reentrant는 **재진입성**이라는 의미로, 어떤 함수가 Reentrant하다는 것은 여러 스레드가 동시에 접근해도 언제나 같은 실행 결과를 보장한다는 의미이다.
    - 이를 만족하기 위해서 해당 서브루틴에서는 공유자원을 사용하지 않으면 된다.
        - 예를들어 정적(전역) 변수를 사용하거나 반환하면 안 되고 호출 시 제공된 매개변수만으로 동작해야한다.
    - 따라서, Reentrant하다면 Thread-safe하지만 그 역은 성립하지 않는다.

> - [위키백과 - 재진입성](https://ko.wikipedia.org/wiki/%EC%9E%AC%EC%A7%84%EC%9E%85%EC%84%B1)
> - [[OS] Thread Safe란? - 곰팡](gompangs.tistory.com/7)
> - [스레드-안전(Thread-safe)과 재진입가능(Reentrant)의 차이 - 커피한잔의 여유와 코딩](sjava.net/tag/thread-safe/)

### 동기화 객체의 종류
* 스레드 동기화 방법
    1. 실행 순서의 동기화
        * 스레드의 실행순서를 정의하고, 이 순서에 반드시 따르도록 하는 것
    2. 메모리 접근에 대한 동기화
        * 메모리 접근에 있어서 동시접근을 막는 것
        * 실행의 순서가 중요한 상황이 아니고, 한 순간에 하나의 스레드만 접근하면 되는 상황을 의미
* 동기화 기법의 종류
    1. 유저 모드 동기화
        * 커널의 힘을 빌리지 않는(커널 코드가 실행되지 않는) 동기화 기법
        * 성능상 이점, 기능상의 제한(라이브러리를 이용)
        * **Ex) 크리티컬 섹션 기반의 동기화, 인터락 함수 기반의 동기화**
    2. 커널 모드 동기화
        * 커널에서 제공하는 동기화 기능을 활용하는 방법
        * 기능상 우수, 속도 떨어짐. 각 프로세스들 안에 있는 쓰레드끼리의 동기화도 가능하다.(근데 잘 안 쓸 수도 있다)
        * 커널 모드로의 변경이 필요하고 이는 성능 저하로 이어짐, 다양한 기능 활용 가능
        * **Ex) 뮤텍스 기반의 동기화, 세마포어 기반의 동기화, 이름있는 뮤텍스 기반의 프로세스 동기화, 이벤트 기반의 동기화**

> - [http://christin2.tistory.com/entry/Chapter-13-%EC%93%B0%EB%A0%88%EB%93%9C-%EB%8F%99%EA%B8%B0%ED%99%94-%EA%B8%B0%EB%B2%95-1](http://christin2.tistory.com/entry/Chapter-13-%EC%93%B0%EB%A0%88%EB%93%9C-%EB%8F%99%EA%B8%B0%ED%99%94-%EA%B8%B0%EB%B2%95-1)
> - [https://m.blog.naver.com/PostView.nhn?blogId=smuoon4680&logNo=50127179815&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F](https://m.blog.naver.com/PostView.nhn?blogId=smuoon4680&logNo=50127179815&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)


### 뮤텍스와 세마포어의 차이

* 프로세스 간 메시지를 전송하거나, 공유메모리를 통해 특정 데이터를 공유하게 되는 경우 문제가 발생할 수 있다. 즉, 공유된 자원에 여러개의 프로세스가 동시에 접근하면서 문제가 발생하는 것으로써 공유된 자원 속 하나의 데이터는 한 번에 하나의 프로세스만 접근할 수 있도록 제한해 두어야 한다.

* 이를 위하여 고안된 것이 바로 **Semaphore 세마포어**다.

* **세마포어** : 멀티프로그래밍 환경에서 공유 자원에 대한 접근을 제한하는 방법

* **임계 구역(Critical Section)**
  * 여러 프로세스가 데이터를 공유하며 수행될 때, 각 프로세스에서 공유 데이터를 접근하는 프로그램 코드 부분. 공유 데이터를 여러 프로세스가 동시에 접근할 때 잘못된 결과를 만들 수 있기 때문에, 한 프로세스가 임계 구역을 수행할 때는 다른 프로세스가 접근하지 못하도록 해야 한다.

* 뮤텍스(Mutex)
    * 공유된 자원의 데이터를 **여러 스레드가** 접근하는 것을 막는 것
    * 상호배제라고도 하며, Critical Section(=임계구역)을 가진 스레드의 Running time이 서로 겹치지 않도록 각각 단독으로 실행하게 하는 기술이다.
    * 해당 접근을 조율하기 위해 lock과 unlock을 사용한다.
      * lock : 현재 임계구역에 들어갈 권한을 얻어옴(만약 다른 프로세스/스레드가 임계 구역 수행 중이면 종료할 때까지 대기)
      * unlock : 현재 임계 구역을 모두 사용했음을 알림. (대기 중인 다른 프로세스/스레드가 임계 구역에 진입할 수 있음)
    * 뮤텍스는 상태가 0, 1로 **이진 세마포어**로 부르기도 한다.
* 세마포어(Semaphore)
   * 공유된 자원의 데이터를 **여러 프로세스가** 접근하는 것을 막는 것
   * 리소스 상태를 나타내는 간단한 **카운터**로 생각할 수 있다.
        * 운영체제 또는 커널의 한 지정된 저장장치 내의 값이다.
        * 일반적으로 비교적 긴 시간을 확보하는 리소스에 대해 이용한다.
        * 유닉스 시스템 프로그래밍에서 세마포어는 운영체제의 리소스를 경쟁적으로 사용하는 다중 프로세스에서 행동을 조정하거나 또는 동기화 시키는 기술이다.
   * 공유 리소스에 접근할 수 있는 프로세스의 최대 허용치만큼 동시에 사용자가 접근하여 사용할 수 있다.
   * 각 프로세스는 세마포어 값을 확인하고 변경할 수 있다.
        * 사용 중이지 않는 자원의 경우 그 프로세스가 즉시 자원을 사용할 수 있다.
        * 이미 다른 프로세스에 의해 사용 중이라는 사실을 알게 되면 재시도하기 전에 일정 시간을 기다려야 한다.
        * 세마포어를 사용하는 프로세스는 그 값을 확인하고, 자원을 사용하는 동안에는 그 값을 변경함으로써 다른 세마포어 사용자들이 기다리도록 해야한다.
    * 세마포어는 이진수 (0 또는 1)를 사용하거나, 또는 **추가적인 값을 가질 수도 있다.**
* 차이
    1. 가장 큰 차이점은 관리하는 **동기화 대상의 개수**
        * Mutex는 동기화 대상이 오직 하나뿐일 때, Semaphore는 동기화 대상이 하나 이상일 때 사용한다.
    2. Semaphore는 Mutex가 될 수 있지만 Mutex는 Semaphore가 될 수 없다.
        * Mutex는 상태가 0, 1 두 개 뿐인 binary Semaphore
        * Semaphore는 이진수 (0 또는 1)를 사용하거나, 또는 추가적인 값을 가질 수도 있다.
    3. Semaphore는 소유할 수 없는 반면, Mutex는 소유가 가능하며 소유주가 이에 대한 책임을 가진다.
        * Mutex 의 경우 상태가 두개 뿐인 lock 이므로 lock 을 가질 수 있다.
    4. Mutex의 경우 Mutex를 소유하고 있는 스레드가 이 Mutex를 해제할 수 있다. 하지만 Semaphore의 경우 이러한 Semaphore를 소유하지 않는 스레드가 Semaphore를 해제할 수 있다.
    5. Semaphore는 시스템 범위에 걸쳐있고 파일시스템상의 파일 형태로 존재하는 반면 Mutex는 프로세스 범위를 가지며 프로세스가 종료될 때 자동으로 Clean up 된다.

> [http://jwprogramming.tistory.com/13](http://jwprogramming.tistory.com/13)

### CPU 스케줄링

* CPU가 하나의 프로세스 작업이 끝나면 다음 프로세스 작업을 수행해야 한다. 이때 어떤 프로세스를 다음에 처리할 지 선택하는 알고리즘을 CPU Scheduling 알고리즘이라고 한다. 따라서 상황에 맞게 CPU를 어떤 프로세스에 배정하여 효율적으로 처리하는가가 관건이다.

* 선점 vs 비선점
   * Preemptive(선점)
      * 프로세스가 CPU를 점유하고 있는 동안 I/O나 인터럽트가 발생하지 않았음에도 다른 프로세스가 해당 CPU를 강제로 점유할 수 있다. 
      * 즉, 프로세스가 정상적으로 수행중인 동안 다른 프로세스가 CPU를 강제로 점유하여 실행할 수 있다.
   * Non-Preemptive(비선점)
      * 한 프로세스가 CPU를 점유했다면 I/O나 인터럽트 발생 또는 프로세스가 종료될 때까지 다른 프로세스가 CPU를 점유하지 못하는 것이다. 
* CPU 스케줄링의 종류
   * 선점형 스케줄링
     * Priority Scheduling
        - 정적/동적으로 우선순위를 부여하여 우선순위가 높은 순서대로 처리
        - 우선 순위가 낮은 프로세스가 무한정 기다리는 Starvation 이 생길 수 있음
        - Aging 방법으로 Starvation 문제 해결 가능
     * SRT(Shortest Remaining Time) 스케줄링
        - 짧은 시간 순서대로 프로세스를 수행한다.
        - 현재 CPU에서 실행 중인 프로세스의 남은 CPU 버스트 시간보다 더 짧은 CPU 버스트 시간을 가지는 프로세스가 도착하면 CPU가 선점된다.
     * Round Robin 스케줄링
        - 시분할 시스템의 성질을 활용한 방법
        - 일정 시간을 정하여 하나의 프로세스가 이 시간동안 수행하고 다시 대기 상태로 돌아간다.
        - 그리고 다음 프로세스 역시 같은 시간동안 수행한 후, 대기한다. 이러한 작업을 모든 프로세스가 돌아가면서 진행하며, 마지막 프로세스가 끝나면 다시 처음 프로세스로 돌아와서 작업을 반복한다.
       - 일정 시간을 Time Quantum(Time Slice)라고 부른다. 일반적으로 10 ~ 100msec 사이의 범위를 갖는다.
       - 한 프로세스가 종료되기 전에 time quantum이 끝나면 다른 프로세스에게 CPU를 넘겨주기 때문에 선점형 스케줄링의 대표적인 예시다.
     * Multi-level Queue 스케줄링
       - 프로세스를 그룹으로 나누어, 각 그룹에 따라 Ready Queue(준비 큐)를 여러 개 두며, 각 큐마다 다른 규칙을 지정할 수도 있다.(ex. 우선순위, CPU 시간 등)
       - 즉, 준비 큐를 여러 개로 분할해 관리하는 스케줄링 방법이다.
       - 프로세스들이 CPU를 기다리기 위해 한 줄로 서는 게 아니라 여러 줄로 선다.
       - ![multi-level](https://user-images.githubusercontent.com/34755287/53879673-5e979880-4052-11e9-9f9b-e8bfec7c9be6.png)

     * Multi-level feedback Queue 스케줄링
       - 기본 개념은 Multi-level Queue와 동일하나, 프로세스가 하나의 큐에서 다른 큐로 이동 가능하다는 점이 다르다.
       - 모든 프로세스는 가장 위의 큐에서 CPU의 점유를 대기한다. 이 상태로 진행하다가 이 큐에서 기다리는 시간이 너무 오래 걸린다면 **아래의 큐로 프로세스를 옮긴다.** 이와 같은 방식으로 대기 시간을 조정할 수 있다. 
       - 만약, 우선순위 순으로 큐를 사용하는 상황에서 우선순위가 낮은 아래의 큐에 있는 프로세스에서 starvation 상태가 발생하면 이를 우선순위가 높은 위의 큐로 옮길 수도 있다.
       - 대부분의 상용 운영체제는 여러 개의 큐를 사용하고 각 큐마다 다른 스케줄링 방식을 사용한다. 프로세스의 성격에 맞는 스케줄링 방식을 사용하여 최대한 효율을 높일 수 있는 방법을 선택한다.
       -![multi-level feedback Queue](https://user-images.githubusercontent.com/34755287/53879675-5f302f00-4052-11e9-86a2-c02ee03bac64.png)
      
   * 비선점형 스케줄링
       1. FCFS (First Come First Served)
          - 큐에 도착한 순서대로 CPU 할당
            *  Convoy Effect 발생 : 소요 시간이 긴 프로세스가 짧은 프로세스보다 먼저 도착해서 뒤에 프로세스들이 오래 기다려야 하는 현상
          - 실행 시간이 짧은 게 뒤로 가면 평균 대기 시간이 길어짐
       2. SJF (Shortest Job First)
          - 수행시간이 가장 짧다고 판단되는 작업을 먼저 수행
          - FCFS 보다 평균 대기 시간 감소, 짧은 작업에 유리

* CPU 스케줄링 척도
  * Response Time
    - 작업이 처음 실행되기까지 걸린 시간
  * Turnaround Time
    - 실행 시간과 대기 시간을 모두 합한 시간으로 작업이 완료될 때 까지 걸린 시간

> - [운영체제(OS) 6. CPU 스케줄링](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-6.-CPU-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81)
> - [https://github.com/WooVictory/Ready-For-Tech-Interview](https://github.com/WooVictory/Ready-For-Tech-Interview)