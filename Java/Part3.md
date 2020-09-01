# 2. Java - Part3
### :book: Contents
- [java의 가비지 컬렉션(Garbage Collection) 처리 방법](#java의-가비지-컬렉션-처리-방법)
- [java 직렬화(Serialization)와 역직렬화(Deserialization)란 무엇인가](#직렬화와-역직렬화)
- [클래스, 객체, 인스턴스의 차이](#클래스-객체-인스턴스의-차이)
- [객체(Object)란 무엇인가]()
- [오버로딩과 오버라이딩의 차이(Overloading vs Overriding)]()
- [Call by Reference와 Call by Value의 차이]()
- [인터페이스와 추상 클래스의 차이(Interface vs Abstract Class)]()

<br>

---

<br>

### java의-가비지-컬렉션-처리-방법
![JAVA 가비지 컬렉션](https://cdn.educba.com/academy/wp-content/uploads/2019/10/What-is-Java-Garbage-Collector.png)
- **가비지 콜렉터(Garbage Collector)란?**
> 가비지 : '정리되지 않은 메모리', '유효하지 않은 메모리 주소'   
 　  　  　= 주소를 잃어버려서 사용할 수 없는 메모리 혹은 앞으로 사용하지 않고 메모리를 가지고 있는 객체
        
&nbsp;\- 메모리가 부족할 때 이런 가비지들을 메모리에서 해제 시켜 다른 용도로 사용할 수 있게 해주는 프로그램이 가비지 콜렉터    
&nbsp;\- C++와 같은 다른 언어에서는 사용하지 않을 객체의 메모리를 직접 해제해주어야 함   
> Stop-the-world : GC를 실행하기 위해 **JVM이 애플리케이션 실행을 멈추는 것**으로   
 　  　  　 　  　  stop-the-world가 발생하면 GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춤   
 　  　  　 　  　  GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다. (어떤 GC알고리즘을 사용해도 발생!)

- **가비지 컬렉터의 역할**   
  1. 힙(Heap) 내의 객체 중에서 가비지(Garbage)를 찾아낸다.   
  2. 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다.   

- **가비지 컬렉터의 과정 (Mark and Sweep)**   
  1. Mark
      - GC가 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 어떤 객체를 참조하고 있는지 찾는 과정   
      - 이 과정에서 Stop and world가 발생!   
  2. Sweep
      - 이후 Mark 되어 있지 않은 객체들을 힙에서 제거하는 과정   
  
- **Minor GC와 Major GC**
> JVM의 **Heap은 Young, Old, Perm 세 영역**으로 나뉜다.   
**Young 영역에서 발생한 GC를 Minor GC**, **나머지 두 영역에서 발생한 GC를 Major GC(Full GC)**라고 한다.
1. **Young 영역**   
 : 새롭게 생성한 객체가 위치,   
 대부분의 객체가 금방 unreachable 상태가 되기 떄문에 많은 객체가 Young 영역에 생성되었다 사라짐
2. **Old 영역**   
 : Young 영역에서 reachable 상태를 유지해 살아남은객체가 여기로 복사된다.   
   대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다.
3. **Perm 영역**   
 : Method Area라고도 한다. 클래스와 메소드 정보와 같이 자바 언어 레벨에서는 거의 사용되지 않는다.

- **Reachability**
> **Java의 GC가 가비지 객체를 판별하기 위해 도입한 개념**   
어떤 객체에 유효한 참조가 있으면 'reachable', 없으면 'unreachable'로 구별하고 이것을 가비지로 간주한다.   
즉, 객체에 대한 Reachability을 제어할 수 있다면 코드를 통해 Java GC에 일부 관여하는 것이 가능하다.
  - java에서는 이를 위해 java.lang.ref 패키지에 SoftReference, WeakReference 등을 제공
  
- **Strong Reference**
  - 기본적으로 new로 할당되는 메모리들은 모두 Strong Reference를 가지기 때문에, 캐시와 같은 것을 만든다고 할 때 메모리 누수에 조심해야 함
  - 캐시의 키가 원래 데이터에서 삭제가 된다면 캐시 내부의 키와 값은 더 이상 의미가 없는 데이터, 즉 가비지가 된다.
  - 그럼에도 GC는 삭제된 캐시의 키를 가비지로 인식하지 못함.
  - 이는 캐시에 넣어준 데이터가 Strong Reference로 독자적인 Reachability를 가지기 때문이다.
  - 따라서 캐시에 데이터를 넣어 줄 때, 원래 데이터에 Weak Reference를 넣어준다면 이러한 문제를 방지할 수 있다.
  
- **Weak Reference**
  - new로 할당된 객체의 유효 참조를 인위적으로 설정할 수 있게 해준다.
  - 원래의 데이터가 삭제되면 이 객체에 Weak Reference가 걸려있는 객체들은 모두 가비지로 인식된다.
  - 위와 같은 이유로 캐시를 만들 때 WeakHashMap을 사용하는 것을 권장
<br></br>
> - [https://velog.io/@litien/%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0GC](https://velog.io/@litien/%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0GC)
> - [https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)

<br>

### 직렬화와-역직렬화
- **직렬화(Serialize)란?**
  - 자바 내부에서 사용되는 Object, Data를 외부 자바 시스템에서도 사용할 수 있도록 **byte 형태로 데이터를 변환**하는 기술
  - JVM의 메모리에 상주되어 있는 객체 데이터를 바이트 형태로 변환하는 기술
- **역직렬화(Deserialize)란?**
  - byte로 변환된 Data를 원래대로 Object나 Data로 변환하는 기술
  - 직렬화된 바이트 형태의 데이터를 객체로 변환해서 JVM으로 상주시키는 형태
- 자바 직렬화 조건   
  1. **자바 기본(Primiptive) 타입**
  2. **java.io.Serializable 인터페이스를 상속받은 객체**는 직렬화 할 수 있는 기본 조건을 가짐
- 직렬화 방법   
자바 직렬화 방법은 java.io.ObjectOutputStream 객체를 이용한다.
```JAVA
    Member member = new Member("김배민", "deliverykim@baemin.com", 25);
    byte[] serializedMember;
    try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
        try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
            oos.writeObject(member);
            // serializedMember -> 직렬화된 member 객체 
            serializedMember = baos.toByteArray();
        }
    }
    // 바이트 배열로 생성된 직렬화 데이터를 base64로 변환
    System.out.println(Base64.getEncoder().encodeToString(serializedMember));
}
```
- 역직렬화 조건
  1. 직렬화 대상이 된 객체의 클래스가 클래스 패스에 존재해야 하며 import 되어 있어야 함
     - 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있다.   
       (같은 시스템 내부이라도 소스 버전이 다를 수 있다.)
  2. 자바 직렬화 대상 객체는 동일한 serialVersionUID 를 가지고 있어야 한다.
     - 모델의 버젼간의 호환성을 유지하기 위해서 SUID(serialVersionUID)를 정의한다.
  
- 역직렬화 예제
```JAVA
   // 직렬화 예제에서 생성된 base64 데이터 
    String base64Member = "...생략";
    byte[] serializedMember = Base64.getDecoder().decode(base64Member);
    try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedMember)) {
        try (ObjectInputStream ois = new ObjectInputStream(bais)) {
            // 역직렬화된 Member 객체를 읽어온다.
            Object objectMember = ois.readObject();
            Member member = (Member) objectMember;
            System.out.println(member);
        }
    }
```

- 자바 직렬화가 사용되는 이유
1. 데이터 타입이 자동으로 맞춰진다.
   - 자바 시스템 개발에 최적화되어 있기 때문에 복잡한 데이터 구조의 클래스의 객체라도   
     직렬화 기본 조건을 지키면 큰 작업없이 바로 직렬화가 가능하다.
2. JVM의 메모리에서만 상주되어있는 객체 데이터를 그대로 영속화(Persistence)가 필요할 때 사용된다.
3. 주로 서블릿 세션, 캐시, 자바 RMI(Remote Method Invocation)에 사용된다.

- 주의할 점 **(자바의 직렬화는 타입에 엄격하다!)**
  - 외부 저장소로 저장되는 데이터는 짧은 만료시간의 데이터를 제외하고 자바 직렬화의 사용을 지양한다.
  - 역직렬화시 반드시 예외가 생긴다는 것을 생각하고 개발한다.
  - 자주 변경되는 비즈니스적인 데이터를 자바 직렬화을 사용하지 않는다.
  - 긴 만료 시간을 가지는 데이터는 JSON 등 다른 포맷을 사용하여 저장한다.
<br></br>
> - [https://nesoy.github.io/articles/2018-04/Java-Serialize](https://nesoy.github.io/articles/2018-04/Java-Serialize)
> - [https://woowabros.github.io/experience/2017/10/17/java-serialize.html](https://woowabros.github.io/experience/2017/10/17/java-serialize.html)

<br>

### 클래스-객체-인스턴스의-차이
1. Class란?
- 개념
  - Object를 만들어 내기 위한 설계도 혹은 틀
   - 연관되어 있는 Variable과 Method의 집합
2. Object란?
- 개념
  - 소프트웨어 세계에 구현할 대상
  - Class에 선언 된 모양 그대로 생성된 실체
- 특징
  - 'Class의 Instance'라고도 부른다.
  - Object는 모든 Instance를 대표하는 포괄적인 의미를 갖는다.
  - OOP의 관점에서 Class의 타입으로 선언되었을 때 'Object'라고 부른다.
  
<br></br>
> - [https://geonlee.tistory.com/11](https://geonlee.tistory.com/11)

<br>
  
