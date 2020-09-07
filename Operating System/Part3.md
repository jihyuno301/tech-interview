# 3. Operating System
**:book: Contents**
* [교착상태(데드락, Deadlock)의 개념과 조건](#교착상태의-개념과-조건)
* [사용자 수준 스레드와 커널 수준 스레드](#사용자-수준-스레드,-커널-수준-스레드)
* [외부 단편화와 내부 단편화](#외부-단편화와-내부-단편화)
* [Context Switching](#context-switching)
* [Swapping](#swapping)

### 교착상태의 개념과 조건
* 교착상태(데드락, Deadlock) 란
  * 첫 번째 스레드는 두 번째 스레드가 들고 있는 객체의 락이 풀리기를 기다리고 있고, 두 번째 스레드 역시 첫 번째 스레드가 들고 있는 객체의 락이 풀리기를 기다리는 상황을 일컷는다.
  * 모든 스레드가 락이 풀리기를 기다리고 있기 때문에, 무한 대기 상태에 빠지게 된다. 이런 스레드를 교착상태에 빠졌다고 한다.
* 교착상태의 4가지 조건
  1. 상호 배제(mutual exclusion)
      * 한 번에 한 프로세스만 공유 자원을 사용할 수 있다.
      * 좀 더 정확하게는, 공유 자원에 대한 접근 권한이 제한된다. 자원의 양이 제한되어 있더라도 교착상태는 발생할 수 있다.
  2. 들고 기다리기(hold and wait) = **점유대기**
      * 공유 자원에 대한 접근 권한을 갖고 있는 프로세스가, 그 접근 권한을 양보하지 않은 상태에서 다른 자원에 대한 접근 권한을 요구할 수 있다.
  3. 선취(preemption) 불가능 = **비선점**
      * 한 프로세스가 다른 프로세스의 자원 접근 권한을 강제로 취소할 수 없다.
  4. 대기 상태의 사이클(circular wait) = **순환대기**
      * 두 개 이상의 프로세스가 자원 접근을 기다리는데, 그 관계에 사이클이 존재한다.
* 교착상태 방지
  * 4가지 조건들 가운데 하나를 제거하면 된다.
  * 공유 자원 중 많은 경우가 한 번에 한 프로세스만 사용할 수 있기 때문에(예를 들어, 프린트) 1번 조건은 제거하기 어렵다.
  * 대부분의 교착상태 방지 알고리즘은 4번 조건, 즉 대기 상태의 사이클이 발생하는 일을 막는 데 초점이 맞춰져 있다.

> :arrow_double_up:[Top](#3-operating-system)    :leftwards_arrow_with_hook:[Back](https://github.com/WeareSoft/tech-interview#3-operating-system)    :information_source:[Home](https://github.com/WeareSoft/tech-interview#tech-interview)
> - [코딩 인터뷰 완전 분석, 프로그래밍인사이트](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966263080&OV_REFFER=http://click.linkprice.com/click.php?m=kbbook&a=A100532541&l=9999&l_cd1=0&u_id=jm0gctc7ca029ofs02yqe&l_cd2=0&tu=https%3A%2F%2Fwww.kyobobook.co.kr%2Fproduct%2FdetailViewKor.laf%3FmallGb%3DKOR%26ejkGb%3DKOR%26barcode%3D9788966263080)

### 사용자 수준 스레드와 커널 수준 스레드
> :arrow_double_up:[Top](#3-operating-system)    :leftwards_arrow_with_hook:[Back](https://github.com/WeareSoft/tech-interview#3-operating-system)    :information_source:[Home](https://github.com/WeareSoft/tech-interview#tech-interview)
> - []()

### 외부 단편화와 내부 단편화
> :arrow_double_up:[Top](#3-operating-system)    :leftwards_arrow_with_hook:[Back](https://github.com/WeareSoft/tech-interview#3-operating-system)    :information_source:[Home](https://github.com/WeareSoft/tech-interview#tech-interview)
> - []()

### Context Switching
> :arrow_double_up:[Top](#3-operating-system)    :leftwards_arrow_with_hook:[Back](https://github.com/WeareSoft/tech-interview#3-operating-system)    :information_source:[Home](https://github.com/WeareSoft/tech-interview#tech-interview)

* Context Switching이란?
    * 현재 진행하고 있는 Task(Process, Thread)의 상태를 저장하고 다음 진행할 Task의 상태 값을 읽어 적용하는 과정을 말한다. 
* Context Switching 과정
    1. Task의 대부분 정보는 Register에 저장되고 PCB(Process Control Block)로 관리된다. 
    2. 현재 실행하고 있는 Task의 PCB 정보를 저장한다. (Process Stack, Ready Queue)
    3. 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행할 수 있다.
* Context Switching Cost (Process vs Thread)
    * Process Context Switching 비용 > Thread Context Switching 비용
    * Thread는 Stack 영역을 제외한 모든 메모리를 공유하므로 Context Switching 수행 시 Stack 영역만 변경하면 되기 때문에 비용이 적게 든다. 

> - [Context Switching이란?](https://nesoy.github.io/articles/2018-11/Context-Switching)

### Swapping
> :arrow_double_up:[Top](#3-operating-system)    :leftwards_arrow_with_hook:[Back](https://github.com/WeareSoft/tech-interview#3-operating-system)    :information_source:[Home](https://github.com/WeareSoft/tech-interview#tech-interview)
> - []()

---

## Reference
> - []()


## :house: [Home](https://github.com/WeareSoft/tech-interview)
