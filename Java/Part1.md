# 2. Java - Part1
### :book: Contents
* [java 프로그래밍이란](#java-프로그래밍이란)
* [Java SE와 Java EE 애플리케이션 차이](#java-se와-java-ee-애플리케이션-차이)
* [java와 c/c++의 차이점](#java와-C-C쁠쁠의-차이점)
* [java 언어의 장단점](#java-언어의-장단점)
* [java의 접근 제어자의 종류와 특징](#java의-접근-제어자의-종류와-특징)
* [java의 데이터 타입](#java의-데이터-타입)
* [Wrapper class](#wrapper-class)

---

### java 프로그래밍이란
* 자바란?

  * 1995년 제임스 고슬링(James Gosling)에 의해서 탄생
  * 썬 마이크로시스템즈(Sun Micrsystems)에서 발표
  * 오크(Oak)언어에서 시작해서 Java 언어로 발전
  * 가전제품에서 탑재할 수 있는 프로그램을 개발하기 위한 목적으로 탄생
  * 자바 언어 등장 당시에는 C언어에 비해 효율성이 떨어진다는 지적이 있었으나 메모리 및 CPU의 발전으로 해결되었다. 
  
* 자바의 특징
  * 이식성이 높다 : 자바 언어로 개발된 프로그램은 자바실행환경(JRE: Java Runtime Environment)이 설치된 모든 운영체제에서 실행 가능
  * 객체 지향 언어 : OOP(Object Oriented Programming), 캡슐화, 상속, 다형성
  * 함수적 스타일 코딩 지원 : 대용량 데이터의 병렬처리, 람다식 지원(자바1.8)으로 컬렉션의 요소를 필터링, 매핑, 집계하기 쉬워지고 코드 
  자체도 간결해짐
  * 메모리 자동으로 관리 : Garbage Collector
  * 웹, 모바일 등의 다양한 애플리케이션 개발 가능
  
### Java SE와 Java EE 애플리케이션 차이

자바 프로그래밍 언어의 플랫폼은 **4가지**가 존재(Java SE, Java EE, Java ME, JavaFX)

* Java SE (Standard Edition)
  * 대부분의 사람들이 자바 프로그래밍 언어를 떠올릴때 플랫폼
  * Java SE의 API는 자바 프로그래밍 언어의 핵심 기능들을 제공
  * 기초적인 타입부터 네트워킹, 보안, 데이터베이스 처리, GUI 개발은 물론 XML 파싱에 이르는 고수준의 클래스들을 
  모두 다룰 수 있다.
  * 가상 머신, 개발 도구, 배포 기술 그리고 자바 기술을 사용하는 어플리케이션에서 일반적으로 사용되는 부가적인 클래스
  라이브러리들과 툴킷까지 제공하고 있다.
* Java EE (Enterprise Edition)
  * Java SE 플랫폼을 기반으로 그 위에 탑재된다. 
  * 대규모, 다계층, 확장성, 신뢰성, 그리고 보안 네트워킹 어플리케이션의 개발 과 실행을 위한 API 및 환경을 제공하고 있다.
  
  
  > https://www.ibm.com/support/knowledgecenter/ko/SSQP76_8.9.1/com.ibm.odm.dserver.rules.res.managing/topics/con_javase_javaee_applis.html


### java와 c c쁠쁠의 차이점
1. **메모리관리**
  - C/C++은 메모리 관리를 직접한다. 코드로 하드웨어를 제어 가능 
  - JAVA는 GarbageCollector라는 별도 프로그램이 돌면서 메모리관리를 함

2. **속도차이 because 실행환경**
  - C언어가 JAVA보다 상대적으로 처리속도가 빠름.
  - Why?
  - C코드는 컴파일 된 프로세스를 수행 함. 컴파일 과정이 오래걸릴 뿐 수행시간은 빠름.
  - Java코드는 실행하면서 JVM인터프리터에의해 바이트코드를 기계코드로 변환 함.
  - 또, 자바의 자동 메모리 관리는 대부분의 환경에서 유용하지만 제한된 메모리 리소스를 최적으로 사용해야 하는 프로그램에는 C가 더 좋음.

3. **객체지향과 절차지향** ( C와 JAVA의 차이점 )
  - 절차지향은 대부분 코드가 main메소드에 한방에 정의 됨.
  - 흐름을 읽기는 좋지만 부분수정할때도 전체코드를 컴파일 해야 됨.
  - 객체지향은 여러 Class로 나누어 개발이 가능 함.
  - 필요한 Class만 수정해서 컴파일 할 수 있어 유지보수 측면에서 유리 함.

4. C/C++과 Java의 **컴파일 과정 차이점**
  - C코드는 [ Editor -> Source File --(컴파일)--> Object File --(링크)--> 실행 File -> 프로그램 실행 ] 순서로 진행
    * 컴파일 : 프로그래밍언어로 작성한 원시코드파일을 기계어로 번역하여 목적코드파일에 저장
    * 링크 : 목적코드 파일과 라이브러리 파일을 하나로 합처 실행파일 생성
  - Java코드는 링크과정이 없음.
  - 컴파일러가 바로 바이트코드를 생성.


### java 언어의 장단점
- 장점
  - **운영체제에 독립적이다.**
    - JVM에서 동작하기 때문에, 특정 운영체제에 종속되지 않는다.
  - **객체지향 언어이다.**
    - 객체지향적으로 프로그래밍 하기 위해 여러 언어적 지원을 하고있다. (캡슐화, 상속, 추상화, 다형성 등)
    - 객체지향 패러다임의 특성상 비교적 이해하고 배우기 쉽다.
  - **자동으로 메모리 관리를 해준다.**
    - JVM에서 Garbage Collector라고 불리는 데몬 쓰레드에 의해 GC(Garbage Collection)가 일어난다. GC로 인해 별도의 메모리 관리가 필요 없으며 비지니스 로직에 집중할 수 있다. [(참고)](http://www.jpstory.net/2013/12/15/garbage-collection-in-java/)
  - **오픈소스이다.**
    - *정확히 말하면 OpenJDK가 오픈소스이다. OracleJDK는 사용 목적에 따라서 유료가 될 수 있다.*
      - OracleJDK의 유료화 이슈는 다음을 참고. [(참고)](https://okky.kr/article/490213)
    - 많은 Java 개발자가 존재하고 생태계가 잘 구축되어있다. 덕분에 오픈소스 라이브러리가 풍부하며 잘 활용한다면 짧은 개발 시간 내에 안정적인 애플리케이션을 쉽게 구현할 수 있다.
  - **멀티스레드를 쉽게 구현할 수 있다.**
    - 자바는 스레드 생성 및 제어와 관련된 라이브러리 API를 제공하고 있기 때문에 실행되는 운영체제에 상관없이 멀티 스레드를 쉽게 구현할 수 있다.
  - **동적 로딩(Dynamic Loading)을 지원한다**
    - 애플리케이션이 실행될 때 모든 객체가 생성되지 않고, 각 객체가 필요한 시점에 클래스를 동적 로딩해서 생성한다. 또한 유지보수 시 해당 클래스만 수정하면 되기 때문에 전체 애플리케이션을 다시 컴파일할 필요가 없다. 따라서 유지보수가 쉽고 빠르다.
- 단점
  - **비교적 속도가 느리다.**
    - 자바는 한 번의 컴파일링으로 실행 가능한 기계어가 만들어지지 않고 JVM에 의해 기계어로 번역되고 실행하는 과정을 거치기 때문에 C나 C++의 컴파일 단계에서 만들어지는 완전한 기계어보다는 속도가 느리다. 그러나 하드웨어의 성능 향상과 바이트 코드를 기계어로 변환해주는 JIT 컴파일러 같은 기술 적용으로 JVM의 기능이 향상되어 속도의 격차가 많이 줄어들었다.
  - **예외처리가 불편하다.**
    - 프로그래머 검사가 필요한 예외가 등장한다면 무조건 프로그래머가 선언을 해줘야 한다.
    
> - [http://yolojeb.tistory.com/17](http://yolojeb.tistory.com/17)
> - [http://huhghiza.tistory.com/7](http://huhghiza.tistory.com/7)

### java의 접근 제어자의 종류와 특징

**접근 제어자(access modifier)**

>객체 지향에서 정보 은닉(data hiding)이란 사용자가 굳이 알 필요가 없는 정보는 사용자로부터 숨겨야 한다는 개념이다. <br>
>그렇게 함으로써 사용자는 언제나 최소한의 정보만으로 프로그램을 손쉽게 사용할 수있게 된다. <br>
>자바에서는 이러한 정보 은닉을 위해 접근 제어자(access modifier)라는 기능을 제공하고 있다. <br> 
>접근 제어자를 사용하면 클래스 외부에서의 직접적인 접근을 허용하지 않는 멤버를 설정하여 정보 은닉을 구체화할 수 있다. <br>

* private
  * private 접근 제어자를 사용하여 선언된 클래스 멤버는 외부에 공개되지 않으며, 외부에서는 직접 접근할 수 없다.
  * 즉, 자바 프로그램은 private 멤버에 직접 접근할 수 없으며, 해당 객체의 public 메소드를 통해서만 접근할 수 있다.
  * 따라서 private 멤버는 public 인터페이스를 직접 구성하지 않고, 클래스 내부의 세부적인 동작을 구현하는 데 사용된다. 
* public
  * public 접근 제어자를 사용하여 선언된 클래스 멤버는 외부로 공개되며, 해당 객체를 사용하는 프로그램 어디에서나 직접 접근할 수 있다.
  * 자바 프로그램은 public 메소드를 통해서만 해당 객체의 private 멤버에 접근할 수 있다.
  * 따라서 public 메소드는 private 멤버와 프로그램 사이의 인터페이스(interface) 역할을 수행한다고 할 수 있다.
* default
  * 자바에서는 클래스 및 클래스 멤버의 접근 제어의 기본값으로 default 접근 제어를 별도로 명시하고 있다.
  * 이러한 default를 위한 접근 제어자는 따로 존재하지 않으며, 접근 제어자가 지정되지 않으면 자동적으로 default 접근 제어를 가지게 된다.
  * default 접근 제어를 가지는 멤버는 같은 클래스의 멤버와 같은 패키지에 속하는 멤버에서만 접근할 수 있다.
* protected
  * 자바 클래스는 private 멤버로 정보를 은닉하고, public 멤버로 사용자나 프로그램과의 인터페이스를 구축한다.
  * 여기에 부모 클래스(parent class)와 관련된 접근 제어자가 하나 더 존재한다.
  * protected 멤버는 부모 클래스에 대해서는 public 멤버처럼 취급되며, 외부에서는 private 멤버처럼 취급됩니다.
  * 클래스의 protected 멤버에 접근할 수 있는 영역은 다음과 같다.
    1. 이 멤버를 선언한 클래스의 멤버
    2. 이 멤버를 선언한 클래스가 속한 패키지의 멤버
    3. 이 멤버를 선언한 클래스를 상속받은 자식 클래스(child class)의 멤버
    
    ![접근 제어자 범위](https://t1.daumcdn.net/cfile/tistory/22703F5057E60F262E)


### java의 데이터 타입
1. 기본 데이터 타입(Primitive Data Type)
    * 기본 타입의 종류는 byte, short, char, int, float, double, boolean이 있다.
        * 정수형 : byte, short, int, long
        * 실수형 : float, double
        * 논리형 : boolean(true/false)
        * 문자형 : char  
    * 기본 타입의 크기가 작고 고정적이기 때문에 메모리의 **Stack** 영역에 저장된다.
2. 참조 타입(Reference Data Type)
    * 참조 타입의 종류는 class, array, interface, Enumeration이 있다.
        * 기본형을 제외하고는 모두 참조형이다.
        * new 키워드를 이용하여 객체를 생성하여 데이터가 생성된 주소를 참조하는 타입이다.
        * String, StringBuffer, List, 개인이 만든 클래스 등
        * String과 배열은 참조 타입과 달리 new 없이 생성이 가능하지만 기본 타입이 아닌 참조 타입이다.
    * 참조 타입의 데이터의 크기가 가변적, 동적이기 때문에 동적으로 관리되는 **Heap** 영역에 저장된다.
    * 더 이상 참조하는 변수가 없을 때 가비지 컬렉션에 의해 파괴된다.
    * 참조 타입은 값이 저장된 곳의 주소를 저장하는 공간으로 객체의 주소를 저장한다. (Call-By-Value)
> - [http://robyncloud.tistory.com/entry/](http://robyncloud.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%97%90%EC%84%9C-%EA%BC%AD-%EC%9D%B4%ED%95%B4%ED%95%B4%EC%95%BC-%EB%90%98%EB%8A%94%EA%B0%9C%EB%85%90-1%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B0%9D%EC%B2%B4-%EC%B0%B8%EC%A1%B0%EB%B3%80%EC%88%98)
> - [https://m.blog.naver.com/PostView.nhn?blogId=roropoly1&logNo=220649338545&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F](https://m.blog.naver.com/PostView.nhn?blogId=roropoly1&logNo=220649338545&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)

### Wrapper class
1. 래퍼 클래스(Wrapper Class)
  * 프로그램에 따라 기본 타입의 데이터를 객체로 취급해야 하는 경우가 있다.
  * 예를 들어, 메소드의 인수로 객체 타입만이 요구되면, 기본 타입의 데이터를 그대로 사용할 수 없다.
  * 이때에는 기본 타입의 데이터를 먼저 객체로 변환한 후 작업을 수행해야 한다
  * 이렇게 8개의 기본 타입(byte, short, int, long, float, double, char, boolean)에 해당하는 데이터를 객체로 포장해주는
  클래스를 래퍼클래스(Wrapper Class)라고 한다.
  * 래퍼 클래스는 모두 java.lang 패키지에 포함되어 제공된다.
  
2. 박싱(Boxing)과 언박싱(UnBoxing)
  * Boxing : 기본 타입의 데이터를 래퍼 클래스의 인스턴스로 변환하는 과정
  * UnBoxing : 래퍼 클래스의 인스턴스에 저장된 값을 다시 기본 타입의 데이터로 꺼내는 과정
  ![boxing / unboxing](http://tcpschool.com/lectures/img_java_boxing_unboxing.png)
3. 오토 박싱(AutoBoxing)과 오토 언박싱(Auto UnBoxing)
  * JDK 1.5부터는 박싱과 언박싱이 필요한 상황에서 자바 컴파일러가 이를 자동으로 처리해준다. [참고](http://tcpschool.com/java/java_api_wrapper)
  
  
> - [Wrapper Class](http://tcpschool.com/java/java_api_wrapper)
