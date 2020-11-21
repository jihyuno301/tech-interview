# 9. Spring
**:book: Contents**
* [스프링 프레임워크란](#스프링-프레임워크란)
* [Spring과 Spring Boot의 차이](#spring과-spring-boot의-차이)
* [Container란](#container란)
* [IOC(Inversion of Control, 제어의 역전)란](#ioc란)
* [Bean이란](#bean이란)
* [MVC 패턴이란](#mvc-패턴이란)
* [MVC1과 MVC2 패턴차이](#MVC1과-MVC2-패턴차이)
* [JSP와 서블릿 비교](#JSP와-서블릿-비교)
* [Dispatcher Servlet](#Dispatcher-Servlet)
* [Spring MVC 구조 흐름](#Spring-MVC-구조-흐름)
* [DI(Dependency Injection, 의존성 주입)란](#di란)
* [AOP(Aspect Oriented Programming)란](#aop란)
* [POJO](#pojo)
* [DAO와 DTO의 차이](#dao와-dto의-차이)
* [Spring JDBC를 이용한 데이터 접근](#spring-jdbc를-이용한-데이터-접근)
* [Annotation이란](#Annotation이란)
* [Filter와 Interceptor 차이](#filter와-interceptor-차이)

---

### 스프링 프레임워크란
* 스프링은 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크이다.
* Java SE로 된 자바 객체 POJO를 자바 EE에 의존적이지 않게 연결해주는 역할을 한다. 
* 스프링의 특징으로는 크기와 부하 측면에서 경량시킨 것과 IOC 기술로 애플리케이션의 느슨한 결합을 도모시킨 것이 있다. 
* 대한민국 공공기관의 웹 서비스 개발 시 사용을 권장하고 있는 전자 정부 표준 프레임워크의 기반 기술이다.

### Spring과 Spring Boot의 차이
스프링 부트는 스프링에서 사용하는 프로젝트를 간편하게 셋업할 수 있는 서브 프로젝트이다. 독립 컨테이너에서 동작할 수 있기 때문에 임베디드 톰갯이 자동으로 실행되고, 임베디드 컨테이너에서 애플리케이션을 실행시키기에는 다소 불안전해서 큰 프로젝트로는 사용하지 않는다. 

### Container란
- 컨테이너(Container)는 보통 **인스턴스의 생명주기를 관리**하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록하는 것이라 할 수 있다. 다시말해, 컨테이너란 당신이 작성한 코드의 처리과정을 위임받은 독립적인 존재라고 생각하면 된다. 컨테이너는 적절한 설정만 되어있다면 누구의 도움없이도 프로그래머가 작성한 코드를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.

- Spring 프레임워크는 다른 프레임워크들과 달리 컨테이너 기능을 제공하고 있다. 이와 같은 컨테이너 기능을 제공하는 것이 가능하도록 하는 것이 IoC 패턴이다.

> - [http://limmmee.tistory.com/13](http://limmmee.tistory.com/13)
> - [http://wiki.javajigi.net/pages/viewpage.action?pageId=281](http://wiki.javajigi.net/pages/viewpage.action?pageId=281)  

### IoC란
- 한줄 요약 : IOC란, 인스턴스의 생성부터 소멸까지 개발자가 아닌 컨테이너가 대신 관리해준느 것을 말한다. 인스턴스 생성의 제어를 서블릿과 같은 bean을 관리해주는 컨테이너가 관리합니다.
- IoC(Inversion of Control, 제어의 역전)란
    - 객체의 생성에서부터 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀐 것을 의미, 또는 제어 권한을 자신이 아닌 다른 대상에게 위임하는 것이다.
    - 이 방식은 대부분의 프레임워크에서 사용하는 방법으로, 개발자는 필요한 부분을 개발해서 끼워 넣기의 형태로 개발하고 실행하게 된다. 프레임워크가 이러한 구조를 가지기 때문에 개발자는 프레임워크에 필요한 부품을 개발하고 조립하는 방식의 개발을 하게 된다.
    - 이렇게 조립된 코드의 최종 호출은 개발자에 의해서 제어되는 것이 아니라 프레임워크의 내부에서 결정된 대로 이뤄지게 되는데, 이러한 현상을 "제어의 역전"이라고 표현한다.
- Spring에서의 IoC
    - Spring 프레임워크에서 지원하는 Ioc Container는 우리들이 흔히 개발하고 사용해왔던 일반 POJO(Plain Old Java Object)의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능들을 제공한다.
- 라이브러리와 프레임워크의 차이
    - IoC의 개념이 적용되었나의 차이
    - 라이브러리를 사용하는 애플리케이션 코드는 애플리케이션 흐름을 직접 제어한다. 단지 동작히는 중에 필요한 기능이 있을 때 능동적으로 라이브러리를 시용할 뿐이다.
    - 반면에 프레임워크는 거꾸로 애플리케이션 코드가 프레임워크에 의해 사용된다. 보통 프레임워크 위에 개발한 클래스를 등록해두고, 프레임워크가 흐름을 주도히는 중에 개발자가 만든 애플리케이션 코드를 시용하도록 만드는 방식이다.

> - [http://wiki.javajigi.net/pages/viewpage.action?pageId=3664](http://wiki.javajigi.net/pages/viewpage.action?pageId=3664)
> - [http://limmmee.tistory.com/13](http://limmmee.tistory.com/13)
> - [http://wiki.javajigi.net/pages/viewpage.action?pageId=281](http://wiki.javajigi.net/pages/viewpage.action?pageId=281)
> - [https://gyoogle.dev/blog/interview/%EC%9B%B9.html](https://gyoogle.dev/blog/interview/%EC%9B%B9.html)

### Bean이란
* 컨테이너 안에 들어있는 객체
* 컨테이너에 담겨있으며, 필요할 때 컨테이너에서 가져와서 사용
* @Bean 을 사용하거나 xml 설정을 통해 일반 객체를 Bean으로 등록할 수 있고, Bean으로 등록된 객체는 쉽게 주입하여 사용 가능

#### Bean 생명주기
- 객체 생성 -> 의존 설정 -> 초기화 -> 사용 -> 소멸
- 스프링 컨테이너에 의해 생명주기 관리
- 스프링 컨테이너 초기화 시 빈 객체 생성, 의존 객체 주입 및 초기화
- 스프링 컨테이너 종료 시 빈 객체 소멸

#### Bean 초기화 방법 3가지
1. 빈 초기화 메소드에 ```@PostConstruct``` 사용
  - 빈 정의 xml에 ```<context:annotation-config></context:annotation-config>``` 추가
2. ```InitializingBean``` 인터페이스의 ```afterPropertiesSet()``` 메소드 오버라이드
3. 커스텀 init() 메소드 정의
  - 빈 정의 xml에 ```init-method``` 속성으로 메소드 이름 지정
  - 또는 빈 초기화 메소드에 ```@Bean(init-method="init")``` 지정

#### Bean 소멸 방법 3가지
1. 빈 소멸 메소드에 ```@PreDestroy``` 사용
  - 빈 정의 xml에 ```<context:annotation-config></context:annotation-config>``` 추가
2. ```DisposableBean``` 인터페이스의 ```destroy()``` 메소드 오버라이드
3. 커스텀 destroy() 메소드 정의
  - 빈 정의 xml에 ```destroy-method``` 속성으로 메소드 이름 지정

##### 권장하는 방법
- 1번 방법 (권장)
  - 사용 방법이 간결하며 코드에서 초기화 메소드가 존재함을 쉽게 파악 가능하여 xml 설정 방법보다 직관적
- 2번 방법 (지양)
  - 빈 코드에 스프링 인터페이스가 노출되어 권장하지 않으며 간결하지 않은 방법
- 3번 방법
  - 빈 코드에 스프링 인터페이스는 노출되지 않지만, 코드만으로 초기화 메소드 호출 여부를 알 수 없는 단점

#### Bean Scope
* **singleton (default)**
  * 애플리케이션에서 Bean 등록 시 singleton scope로 등록
  * Spring IoC 컨테이너 당 한 개의 인스턴스만 생성
  * 컨테이너가 Bean 가져다 주입할 때 항상 같은 객체 사용
  * 메모리나 성능 최적화에 유리
* **prototype**
  * 컨테이너에서 Bean 가져다 쓸 때 항상 다른 인스턴스 사용
  * 모든 요청에서 새로운 객체 생성
  * gc에 의해 Bean 제거
* request
  * Bean 등록 시 하나의 HTTP request 생명주기 안에 단 하나의 Bean만 존재
  * 각각의 HTTP 요청은 고유 Bean 객체 보유
  * Spring MVC Web Application에서 사용
* session
  * 하나의 HTTP Session 생명주기 안에 단 하나의 Bean만 존재
  * Spring MVC Web Application에서 사용
* global session
  * 하나의 global HTTP Session 생명주기 안에 한 개의 Bean 지정
  * Spring MVC Web Application에서 사용
* application
  * ServletContext 생명주기 안에 한 개의 Bean 지정
  * Spring MVC Web Application에서 사용
  
> - [[Spring] IOC(Inversion Of Control): 제어 역전](https://velog.io/@max9106/Spring-IOC%EB%AF%B8%EC%99%84)
> - [[Spring] Spring Bean의 개념과 Bean Scope 종류](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)
> - [Spring 빈/컨테이너 생명주기 (Lifecycle)](https://flowarc.tistory.com/entry/Spring-%EB%B9%88%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0-Lifecycle)
> - [SpringMVC :: 스프링 컨테이너의 생명주기, 빈의 생명주기 (Life cycle), InitialzingBean, DisposableBean, @PreDestroy, @PostConstruct](https://hongku.tistory.com/106)
> - [[Spring] 빈(bean)생명주기 메소드](https://cornswrold.tistory.com/100)



### MVC 패턴이란
- MVC 패턴은 코드의 재사용에 유용하며, 사용자 인터페이스와 응용 프로그램 개발에 소요되는 시간을 줄여주는 효과적인 설계 방식을 말한다.
- 구성요소로는 Model, View, Controller가 있다. Model은 핵심적인 비즈니스 로직을 담당하여 데이터베이스를 관리하는 부분이고, View는 사용자에게 보여주는 화면, Controller는 모델과 뷰 사이에서 정보 교환을 할 수 있도록 연결해주는 역할을 한다. 

> - [https://gyoogle.dev/blog/interview/%EC%9B%B9.html](https://gyoogle.dev/blog/interview/%EC%9B%B9.html)

### MVC1과 MVC2 패턴차이

![MVC1](https://media.vlpt.us/images/aquarius1997/post/17220321-434a-4077-b17c-eeb0d3db2710/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-03-09%20%EC%98%A4%ED%9B%84%206.23.19.png)

- MVC1 패턴의 경우 View와 Controller를 모두 JSP가 담당하는 형태를 가진다. JSP 페이지 안에서 로직 처리를 위해 자바 코드와 함께 사용이 되고, 브라우저에서 JSP에 요청이 오면 직접 자바빈이나 클래스를 이용해 작업을 처리하고, 이를 클라이언트에 출력해준다. 단순한 프로젝트에서는 괜찮지만 큰 프로젝트에서는 조금 사용하기 어려운 패턴이다. JSP 하나에서 MVC가 모두 이루어지다보니 재사용성도 매우 떨어지고, 코드 가독성도 떨어진다. 또한 유지보수가 어렵다. 

![MVC2](https://media.vlpt.us/images/aquarius1997/post/d70dd070-4aa3-46da-b57a-54a9b0f3e10e/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-03-09%20%EC%98%A4%ED%9B%84%206.32.36.png)

- MVC2 패턴은 이와는 다르게 모든 처리를 JSP에서만 담당하는 것이 아니라 서블릿을 만들어 역할 분담을 하는 패턴이다. 요청 결과를 출력해주는 뷰만 JSP가 담당하고, 흐름을 제어해주고 비즈니스 로직에 해당하는 컨트롤러 역할을 서블릿이 담당하게 된다. 이처럼 역할을 분담하면서 유지보수가 용이해지는 장점이 있지만 습득하기 힘들고 구조가 복잡해지는 단점도 있다. 
- 스프링 프레임워크가 MVC2 패턴

> - [https://velog.io/@aquarius1997/MVC-%ED%8C%A8%ED%84%B4](https://velog.io/@aquarius1997/MVC-%ED%8C%A8%ED%84%B4)

### JSP와 서블릿 비교
1. 서블릿(Servlet)
    - 서버에서 웹페이지 등을 동적으로 생성하거나 데이터 처리를 수행하기 위해 자바로 작성된 프로그램이다.
    - 서블릿은 자바코드 안에 HTML태그가 삽입되며 자바언어로 되어있다. .java가 확장자이다.
    - 클라이언트 요청을 처리하고 그 결과를 다시 클라이언트에게 전송하는 서블릿 클래스의 구현 규칙을 지킨 자바 프로그램이다. 
    - Data Processing(Controller)에 좋다.
    - 즉, DB와의 통신, 비즈니스 로직 호출, 데이터를 읽고 확인하는 작업 등에 유용하다. 
    - 서블릿이 수정된 경우 Java 코드를 컴파일(.class 파일 생성)한 후 동적인 페이지를 처리하기 때문에 전체 코드를 업데이트하고 다시 컴파일한 후 재배포하는 작업이 필요하다.(개발 생산성 저하)

2. JSP
    - HTML을 코딩하기가 너무 어렵고 불펴해서 HTML 내부에 Java 코드를 삽입하는 형식이다. 서블릿의 단점을 보완하고자 만든 서블릿 기반의 스크립트 기술이다.
    - 서블릿을 사용하게 되면 웹프로그래밍을 할 수 있지만 자바에 대한 지식이 필요하며 화면 인터페이스 구현에 너무 많은 코드를 필요로 하는 등 비효율적인 측면이 있다. 
    - 때문에 서블릿을 작성하지 않고도 간편하게 웹 프로그래밍을 구현하게 만든 기술이 JSP(Java Server Pages)이다. 
    - 서블릿 기바의 '서버 스크립트 기술'이다.
        - 스크립트 기술 : ASP, PHP 처럼 미리 약속된 규정에 따라 **간단한 키워드**를 조합하여 입력하면, 실행 시점에 각각의 키워드에 매핑이 되어 있는 어떤 코드로 변환 후에 실행되는 형태
    
3. 서블릿과 JSP의 역할
    - 웹 기반의 동적인 처리를 하는 것은 둘 다 같다. 하지만 주로하는 역할의 차이가 있다.
    - JSP는 JSP 기술의 장점을 최대한 활용할 수 있는 웹애플리케이션 구조에서 사용자에게 결과를 보여주는 프레젠테이션 층을 담당
    - Servlet은 Servlet 기술의 장점을 최대한 활용할 수 있는 사용자의 요청을 받아 분석하고 비즈니스 층과 통신하여 처리하고 처리한 결과를 다시 사용자에게 응답하는 Controller 층을 담당한다. 

> - [https://m.blog.naver.com/acornedu/221128616501](https://m.blog.naver.com/acornedu/221128616501)
> - [https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html](https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html)

### Dispatcher Servlet
- 서블릿 컨테이너에서 HTTP 프로토콜을 통해 들어오는 모든 요청을 제일 앞에서 처리해주는 프론트 컨트롤러를 말한다.
- 따라서 서버가 받기 전에, 공통처리 작업을 디스패치 서블릿이 처리해주고 적절한 세부 컨트롤러 작업을 위임해준다.
- 디스패치 서블릿이 처리하는 URL 패턴을 지정해줘야 하는데, 일반적으로는 .mvc와 같은 패턴으로 처리하라고 미리 지정해준다.
- 디스패치 서블릿으로 인해 web.xml이 가진 역할이 상당히 축소되었다. 기존에는 모든 서블릿을 URL 매핑 활용을 위해 모두 web.xml에 등록해 주었지만, 디스패치 서블릿은 그 전에 모든 요청을 핸들링해주면서 작업을 편리하게 할 수 있도록 도와준다. 또한 이 서블릿을 통해 MVC를 사용할 수 있기 때문에 웹 개발 시 큰 장점을 가져다 준다. 
    - web.xml
        - Deploy할 때 Servlet의 정보를 설정해준다.
        - 브라우저가 Java Servlet에 접근하기 위해서는 WAS(Ex. Tomcat)에 필요한 정보를 알려줘야 해당하는 Servlet을 호출할 수 있다.  
            - 정보 1) 배포할 Servlet이 무엇인지
            - 정보 2) 해당 Servlet이 어떤 URL에 매핑되는지 

> - [https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html](https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html)

### Spring MVC 구조 흐름

- 우선, 디스패치 서블릿이 클라이언트로부터 요청을 받으면, 이를 요청할 핸들러 이름을 알기 위해 핸들러맵핑에게 물어본다.
- 핸들러맵핑은 요청 URL을 보고 핸들러 이름을 디스패치 서블릿에게 알려준다. 이때 핸들러를 실행하기 전/후에 처리할 것들을 **인터셉터**로 만들어 준다.
- 디스패처 서블릿은 해당 핸들러에게 제어권을 넘겨주고, 이 핸들러는 응답에 필요한 서비스를 호출하고 렌더링해야 하는 뷰 이름을 판단하여 디스패처 서블릿에게 전송해줍니다.
- 디스패처 서블릿은 받은 뷰 이름을 뷰 리졸버에게 전달해 응답에 필요한 뷰를 만들라고 명령합니다.
- 이때 해당하는 뷰는 디스패처 서블릿에게 받은 모델과 컨트롤러를 활용해 원하는 응답을 생성해서 다시 보내줍니다.
- 디스패처 서블릿은 뷰로부터 받은 것을 클라이언트에게 응답해줍니다.


### DI란
- DI?
    - Dependency Injection, 의존성 주입
    - Dependency Injection은 Spring 프레임워크에서 지원하는 IoC의 형태이다. 
    - DI는 클래스 사이의 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동적으로 연결해주는 것을 말한다. 개발자들은 제어를 담당할 필요없이 빈 설정 파일에 의존관계가 필요하다는 정보만 추가해주면 된다.
        - 컨테이너가 실행 흐름의 주체가 되어 애플리케이션 코드에 의존관계를 주입해주는 것.
    - 객체는 직접 의존하고 있는 객체를 생성하거나 검색할 필요가 없으므로 코드 관리가 쉬워지는 장점이 있다.

- 의존성(Dependency)
    - 현재 객체가 다른 객체와 상호작용(참조)하고 있다면 다른 객체들을 현재 객체의 의존이라 한다.
- 의존성이 위험한 이유
    - 하나의 모듈이 바뀌면 의존한 다른 모듈까지 변경되야 한다.
    - 테스트 가능한 어플을 만들 때 의존성이 있으면 유닛테스트 작성이 어렵다.
    - 유닛테스트의 목적 자체가 다른 모듈로부터 독립적으로 테스트하는 것을 요구한다.
- DI의 특징
    - ‘new’를 사용해 모듈 내에서 다른 모듈을 초기화하지 않으려면 객체 생성은 다른 곳에서 하고, 생성된 객체를 참조하면 된다.
    - 의존성 주입은 Inversion of Control 개념을 바탕으로 한다. 클래스가 외부로부터 의존성을 가져야한다.
- DI가 필요한 이유(DI의 장점)
    - 클래스를 재사용 할 가능성을 높이고, 다른 클래스와 독립적으로 클래스를 테스트 할 수 있다.
    - 비즈니스 로직의 특정 구현이 아닌 클래스를 생성하는데 매우 효과적
- DI의 세가지 방법
    - Contructor Injection : 생성자 삽입
    - Method(Setter) Injection : 메소드 매개 변수 삽입
    - Field Injection : 멤버 변수 삽입

> - [http://www.nextree.co.kr/p11247/](http://www.nextree.co.kr/p11247/)
> - [http://wiki.javajigi.net/pages/viewpage.action?pageId=281](http://wiki.javajigi.net/pages/viewpage.action?pageId=281)
> - [http://tony-programming.tistory.com/entry/Dependency-의존성-이란](http://tony-programming.tistory.com/entry/Dependency-의존성-이란) 

### AOP란
* AOP(Aspect Oriented Programming)란
  - Aspect Oriented Programming, 관점 지향 프로그래밍
  - 어떤 로직을 기준으로 핵심 관점과 부가 관점을 나누고, 관점을 기준으로 모듈화하는 것
  - 핵심 관점은 주로 핵심 비즈니스 로직
  - 부가 관점은 핵심 로직을 실행하기 위한 데이터베이스 연결, 로깅, 파일 입출력 등
  
* AOP 목적
  - 소스 코드에서 여러 번 반복해서 쓰는 코드(= 흩어진 관심사, Concern)를 Aspect로 모듈화하여 핵심 로직에서 분리 및 재사용
  - 개발자가 핵심 로직에 집중할 수 있게 하기 위함
  - 주로 부가 기능을 모듈화
  
* AOP 주요 용어
  - **Aspect**
    - 흩어진 관심사를 모듈화 한 것
    - Advice + PointCut
  - **Target**
    - Aspect를 적용하는 곳(클래스, 메소드 등)
  - **Advice**
    - 실질적으로 수행해야 하는 기능을 담은 구현체
  - **JoinPoint**
    - Advice가 적용될 위치
    - 끼어들 수 있는 지점
    - ex. 메소드 진입 시, 생성자 호출 시, 필드에서 값 꺼낼 때 등
  - **PointCut**
    - JoinPoint의 상세 스펙 정의
    - 더욱 구체적으로 Advice가 실행될 지점 지정
  - **Weaving**
    - PointCut에 의해 결정된 Target의 JoinPoint에 Advice를 삽입하는 과정
  
* AOP 적용 방법
  1. 컴파일 시 적용
      - AspectJ가 사용하는 방법
      - 자바 파일을 클래스 파일로 만들 때 Advice 소스가 추가되어 조작된 바이트 코드 생성하는 방법
  2. 로드 시 적용
      - AspectJ가 사용하는 방법
      - 컴파일 후 컴파일 된 클래스를 로딩하는 시점에 Advice 소스를 끼워넣는 방법
  3. 런타임 시 적용
      - **Spring AOP**가 사용하는 방법
      - 스프링은 런타임 시 Bean 생성
      - A라는 Bean 만들 때 A라는 타입의 프록시 Bean도 생성하고, 프록시 Bean이 A의 메소드 호출 직전에 Advice 소스를 호출한 후 A의 메소드 호출

* Spring AOP 특징
  - [프록시 패턴](https://velog.io/@max9106/Spring-%ED%94%84%EB%A1%9D%EC%8B%9C-AOP-xwk5zy57ee) 기반의 AOP 구현체
    - 프록시 객체는 원래 객체를 감싸고 있는 객체이다. 원래 객체와 타입은 동일하다. 프록시 객체가 원래 객체를 감싸서 client의 요청을 처리하게 하는 패턴이다. 프록시 패턴을 쓰는 이유는 접근을 제어하고 싶거나, 부가 기능을 추가하고 싶을 때 사용한다.
    - Target 객체에 대한 프록시를 만들어 제공
    - Target을 감싸는 프록시는 런타임 시 생성
    - 접근 제어 및 부가 기능 추가를 위해 프록시 객체 사용
  - 프록시가 Target 객체의 호출을 가로채 Advice 수행 전/후 핵심 로직 호출
  - 스프링 Bean에만 AOP 적용 가능
    - 메소드 조인 포인트만 지원하여 메소드가 호출되는 런타임 시점에만 Advice 적용 가능
  - 모든 AOP기능을 제공하지는 않으며 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션의 각종 문제(중복 코드, 프록시 클래스 작성의 번거로움, 객체 간 관계 복잡도 증가)에 대한 해결책 지원 목적

> - [[Spring] 스프링 AOP (Spring AOP) 총정리 : 개념, 프록시 기반 AOP, @AOP](https://engkimbs.tistory.com/746)
> - [[Spring] AOP란?](https://velog.io/@max9106/Spring-AOP%EB%9E%80-93k5zjsm95)
> - [Spring AOP, Aspect 개념 특징, AOP 용어 정리](https://shlee0882.tistory.com/206)

### POJO

Plain Old Java Objet(평범한 구식 자바 객체)

- POJO란 단어가 생긴 배경 설명
  - EJB(Enterprise JavaBean)와 엔터프라이즈 서비스
    - 기업업무처리의 IT 시스템에 대한 의존도가 높아지면서 시스템이 다뤄야 하는 비즈니스 로직 자체가 점차 복잡해졌다. 
    - 애플리케이션 로직의 복잡도와 상세 기술의 복작함을 개발자들이 한 번에 다룬다는 것은 쉬운 일이 아니다. 예를 들어, 한 개발자가 보험업무와 관련된 계산 로직을 자바로 어떻게 구현해야 하는지에 집중하면서 동시에 시스템 레벨에서 멀티DB로 확장 가능한 트랜잭션 처리와 보안 기능을 멀티쓰레드에 세이프하게 만드는 것에 신경쓰는 것은 부담이 매우 크다
    - 많은 사용자의 처리요구를 빠르게 안정적이면서 확장 가능한 형태로 유지하기 위해 필요한 **Low Level의 기술적인(트랜잭션 처리, 상태관리, 멀티쓰레딩, 리소스풀링, 보안 등) 처리**가 요구됐다.
    - 이에 EJB가 등장, 애플리케이션 개발자는 Low Level의 기술들에 관심을 가질 필요가 없다. **BUT** 결론적으로 불필요한만큼 **과도한 엔지니어링**으로 실패한 대표적인 케이스가 되었다. 
    - 마틴 파울러는 **EJB와 같은 잘못 설계된 과도한 기술을 피하고, 객체지향 원리에 따라 만들어진 자바 언어의 기본에 충실하게 비즈니스 로직을 구현하는 일명 POJO 방식으로 돌아서야 한다**고 지적했다. 

- 특징
    - Java에서 제공하는 API 외에 종속되지 않음
    - 특정 규약, 환경에 종속되지 않음
- 환경에 종속되지 않는 것의 장점
    - 코드의 간결함 (비즈니스 로직과 특정 환경/low 레벨 종속적인 코드를 분리하므로 단순)
    - 비즈니스 로직과 특정 환경이 분리되므로 단순함
    - 자동화 테스트에 유리 (환경 종속적인 코드는 자동화 테스트가 어렵지만, POJO는 테스트가 매우 유연)
    - 객체지향 설계의 자유로운 사용
> - [http://limmmee.tistory.com/8?category=654011](http://limmmee.tistory.com/8?category=654011)

### DAO와 DTO의 차이
- **DAO(Data Access Object)**
    - DB의 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 객체를 말한다.
    - DB에 접근을 하기위한 로직과 비즈니스 로직을 분리하기 위해서 사용 한다.
- **DTO(Data Transfer Object)**
    - 계층간 데이터 교환을 위한 자바빈즈를 말한다.
        - 여기서 말하는 계층은 Controller, View, Business Layer, Persistent Layer 이다.
    - 일반적인 DTO는 **로직을 갖고 있지 않는 순수한 데이터 객체**이며, 속성과 그 속성에 접근하기 위한 getter, setter 메소드만 가진 클래스이다.
    - VO(Value Object) 라고도 불린다.
        - DTO와 동일한 개념이지만 read only 속성을 가진다.
    -  프로퍼티(Property)
        - setter/getter에서 set과 get이후에 나오는 단어가 property라고 약속한다. 
        - setAge의 프로퍼티는 age, getName에서의 프로퍼티는 name 
        - 중요한 점은 프로퍼티는 멤버변수 name, age로 결정되는 것이 아니라 getter/setter에서의 name과 age임을 명심해야 한다. 
        - 자바는 다양한 프레임워크에서 **데이터 자동화 처리**를 위해 리플렉션 기법을 사용한다. 데이터 자동화 처리에서 제일 중요한 것은 **표준규격**이다. 
        - 우리가 setter 요청을 하면 프레임워크단에서 setter가 실행된다. 그로 인하여, Layer(서버코딩->view코딩)에 데이터를 넘길 때  DTO를 쓰면 편하다. 
        - 데이터가 자동적으로 클래스화 된다는 것이다. 데이터 자동화 처리된 DTO로 변환되어 우리는 손쉽게 데이터가 셋팅된 오브젝트를 받을 수 있다. 
        - 프로퍼티 규격만 잘 지킨다면, 얼마든지 편하게 DTO로 받을 수 있다. 
    
> - [https://jungwoon.github.io/common%20sense/2017/11/16/DAO-VO-DTO/](https://jungwoon.github.io/common%20sense/2017/11/16/DAO-VO-DTO/)

### Spring JDBC를 이용한 데이터 접근

- 데이터베이스 테이블과, 자바 객체 사이의 단순한 매핑을 간단한 설정을 통해 처리하는 것
- 기존의 JDBC에서는 구현하고 싶은 로직마다 필요한 SQL문이 모두 달랐고, 이에 필요한 Connection, PrepareStatement, ResultSet 등을 생성하고 Exception 처리도 모두 해야하는 번거러움이 존재했습니다.
- Spring에서는 JDBC와 ORM 프레임워크를 직접 지원하기 때문에 따로 작성하지 않아도 모두 다 처리해주는 장점이 있습니다.

### Annotation이란

- 소스코드에 @어노테이션의 형태로 표현하며 클래스, 필드, 메소드의 선언부에 적용할 수 있는 특정기능이 부여된 표현법을 말합니다.
- 애플리케이션 규모가 커질수록, xml 환경설정이 매우 복잡해지는데 이러한 어려움을 개선시키기 위해 자바 파일에 어노테이션을 적용해서 개발자가 설정 파일 작업을 할 때 발생시키는 오류를 최소화해주는 역할을 합니다.
- 어노테이션 사용으로 소스 코드에 메타데이터를 보관할 수 있고, 컴파일 타임의 체크뿐 아니라 어노테이션 API를 사용해 코드 가독성도 높여줍니다.
    - @Controller : dispatcher-servlet.xml에서 bean 태그로 정의하는 것과 같음.
    - @RequestMapping : 특정 메소드에서 요청되는 URL과 매칭시키는 어노테이션
    - @Autowired : 자동으로 의존성 주입하기 위한 어노테이션
    - @Service : 비즈니스 로직 처리하는 서비스 클래스에 등록
    - @Repository : DAO에 등록

### Filter와 Interceptor 차이

#### Filter, Interceptor
* 애플리케이션에서 자주 사용되는 기능(공통 부분)을 분리하여 관리할 수 있도록 Spring이 제공하는 기능

#### Filter, Interceptor 흐름

![filterInterceptor](./images/filterInterceptor.jpg)
1. 서버 실행 시 Servlet이 올라오는 동안 init 후 doFilter 실행
2. Dispatcher Servlet을 지나쳐 Interceptor의 preHandler 실행
3. 컨트롤러를 거쳐 내부 로직 수행 후, Interceptor의 postHandler 실행
4. doFilter 실행
5. Servlet 종료 시 destroy

#### Filter 특징
* Dispatcher Servlet 이전에 수행되고, 응답 처리에 대해서도 변경 및 조작 수행 가능
  * WAS 내의 ApplicationContext에서 등록된 필터가 실행
* WAS 구동 시 FilterMap이라는 배열에 등록되고, 실행 시 Filter chain을 구성하여 순차적으로 실행
* Spring Context 외부에 존재하여 Spring과 무관한 자원에 대해 동작
* 일반적으로 web.xml에 설정
* 예외 발생 시 Web Application에서 예외 처리
* ex. 인코딩 변환, XSS 방어 등
* 실행 메소드
  * init() : 필터 인스턴스 초기화
  * doFilter() : 실제 처리 로직
  * destroy() : 필터 인스턴스 종료

#### Interceptor 특징
* Dispatcher Servlet 이후 Controller 호출 전, 후에 끼어들어 기능 수행
* Spring Context 내부에서 Controller의 요청과 응답에 관여하며 모든 Bean에 접근 가능
* 일반적으로 servlet-context.xml에 설정
* 예외 발생 시 @ControllerAdvice에서 @ExceptionHandler를 사용해 예외 처리
* ex. 로그인 체크, 권한 체크, 로그 확인 등
* 실행 메소드
  * preHandler() : Controller 실행 전
  * postHandler() : Controller 실행 후
  * afterCompletion() : view Rendering 후

##### Filter, Interceptor 차이점 요약
* 필터와 인터셉터는 실행되는 시점에서 차이가 있다. 필터는 웹 애플리케이션에 등록을 하고, 인터셉터는 스프링의 context에 등록을 한다. 따라서 컨트롤러에 들어가기 전 작업을 처리하기 위해 사용하는 공통점이 있지만, 호출되는 시점에서 차이가 존재한다.
* Filter는 WAS단에 설정되어 Spring과 무관한 자원에 대해 동작하고, Interceptor는 Spring Context 내부에 설정되어 컨트롤러 접근 전, 후에 가로채서 기능 동작
* Filter는 doFilter() 메소드만 있지만, Interceptor는 pre와 post로 명확하게 분리
* Interceptor의 경우 AOP 흉내 가능
  * handlerMethod(@RequestMapping을 사용해 매핑 된 @Controller의 메소드)를 파라미터로 제공하여 메소드 시그니처 등 추가 정보를 파악해 로직 실행 여부 판단 가능

> - [[Spring] Filter, Interceptor, AOP 차이 및 정리](https://goddaehee.tistory.com/154)
> - [[Spring] Filter, Interceptor, AOP 차이](https://velog.io/@sa833591/Spring-Filter-Interceptor-AOP-%EC%B0%A8%EC%9D%B4-yvmv4k96)
> - [Spring Filter와 Interceptor](https://jaehun2841.github.io/2018/08/25/2018-08-18-spring-filter-interceptor/#spring-request-flow)
> - [(Spring)Filter와 Interceptor의 차이](https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/)

---

## Reference
> - []()


## :house: [Home](https://github.com/WeareSoft/tech-interview)
