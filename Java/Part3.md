# 2. Java - Part3
### :book: Contents
- [java의 가비지 컬렉션(Garbage Collection) 처리 방법](#java의-가비지-컬렉션-처리-방법)
- [java 직렬화(Serialization)와 역직렬화(Deserialization)란 무엇인가](#직렬화와-역직렬화)
- [클래스, 객체, 인스턴스의 차이](#클래스-객체-인스턴스의-차이)
- [객체(Object)란 무엇인가](#객체란-무엇인가)
- [오버로딩과 오버라이딩의 차이(Overloading vs Overriding)](#오버로딩과-오버라이딩의-차이)
- [Call by Reference와 Call by Value의 차이](#Call-by-Reference와-Call-by-Value의-차이)
- [인터페이스와 추상 클래스의 차이(Interface vs Abstract Class)](#인터페이스와-추상-클래스의-차이)

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
![JAVA 직렬화](https://data-flair.training/blogs/wp-content/uploads/sites/2/2018/02/serialize-deserialize-java-01.jpg)
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
1. **Class란?**
- 개념
  - **Object를 만들어 내기 위한 설계도 혹은 틀**
   - 연관되어 있는 Variable과 Method의 집합
2. **Object란?**
- 개념
  - 소프트웨어 세계에 구현할 대상
  - **Class에 선언 된 모양 그대로 생성된 실체**
- 특징
  - 'Class의 Instance'라고도 부른다.
  - Object는 모든 Instance를 대표하는 포괄적인 의미를 갖는다.
  - OOP의 관점에서 **Class의 타입으로 선언되었을 때 'Object'**라고 부른다.
3. **Instance란?**
- 개념
  - 설계도를 바탕으로 **소프트웨어 세계에 구현된 구체적인 실체**
  - 즉, 객체를 소프트웨어에 실체화 하면 그것을 'Instance'라고 부른다.
  - 실체화된 Instance는 메모리에 할당된다.
- 특징
  - Instance는 Object에 포함된다고 볼 수 있다.
  - OOP의 관점에서 **Object가 메모리에 할당되어 실제 사용될 때 'instance'**라고 부른다.
  - 추상적인 개념(또는 명세)과 구체적인 Object 사이의 관계에 초점을 맞출 경우에 사용한다.
  - '~의 instance'의 형태로 사용된다.
  - Object는 Class의 Instance이다.
  - Object 간의 링크는 Class 간의 연관 관계의 Instance이다.
  - 실행 프로세스는 프로그램의 Instance이다.
  - 즉, Instance라는 용어는 반드시 Class와 Object 사이의 관계로 한정지어 사용할 필요는 없다.
  - 인스턴스는 어떤 원본(추상적인 개념)으로부터 '생성된 복제본'을 의미한다.
```java
/* Animal Class */
public class Animal {
 ...
}
/* Object와 Instance */
public class Main {
 public static void main(String[] args) {
  Animal cat, dog; // 'Object'
// Instance화
  cat = new Animal(); // cat은 Animal Class의 'Instance'(Object를 메모리에 할당)
  dog = new Animal(); // dog은 Animal Class의 'Instance'(Object를 메모리에 할당)
 }
}
```
4. Class, Object, Instance의 차이
- Class VS Object
  - **Class는 설계도, Object는 설계도로 구현한 모든 대상**을 의미한다.
-  Object VS Instance
   - **Class의 타입으로 선언될 때, Object**라 부르고, 그 **Object가 메모리에 할당되어 실제 사용될 때 Instance**라고 부름
   - Object는 현실 세계에 가깝고, Instance는 소프트웨어 세계에 가깝다.
   - Object는 실체, Instance는 관계에 초점을 둔다. (= Object를 Class의 Instance라고도 부른다.)
   - **Instance화하여 레퍼런스를 할당한 Object를 Instance**라고 말하지만,   
     이는 원본(추상적인 개념)으로부터 생성되었다는 것에 의미를 부여하는 것일 뿐 둘을 엄격하게 구분하긴 어렵다.
<br></br>
> - [https://geonlee.tistory.com/11](https://geonlee.tistory.com/11)

<br>
  
### 객체란-무엇인가
- **객체(Object)란?**
  - 물리적으로 존재하거나 추상적으로 생각할 수 있는 것 중에서 자신의 속성을 가지고 있고 다른 것과 식별 가능한 것
  - 속성과 동작으로 구성되어 있으며 자바에서는 이를 각각 **필드(Field)와 메소드(Method)**라고 부른다.
- **객체 모델링(Object Modeling)**
  - **현실세계의 객체를 소프트웨어 객체로 설계하는 것**
  - 현실 객체의 속성과 동작을 추려내서 소프트웨어 객체의 필드와 메소드로 정의하는 과정
  <br>
- 객체 지향 프로그래밍의 특징
1. **캡슐화 (Encapsultaion)**
   - 객체의 필드, 메소드를 하나로 묶고, 실제 구현 내용을 감추는 것
   - 외부 객체는 객체내부의 구조를 알지 못하며 객체가 노출해서 제공하는 필드와 메소드만 이용할 수 있다.
> cf.) 캡슐화를 하는 이유?   
외부의 잘못된 사용으로 인해 객체가 속상되지 않도록 하기 위함   
자바는 캡슐화된 멤버를 노출시킬지, 숨길지 결정하기 위해 접근 제한자를 사용. 이를 통해 필드, 메소드의 사용범위를 제한한다.
2. **상속 (Inheritance)**
   - OOP에서 부모 역할의 상위 객체가 자기가 가지고 있는 필드와 메소드를   
     자식 역할의 하위 객체에게 물려주어 하위 객체가 사용할 수 있도록 해주는 것
   - 상위 객체를 재사용함으로써 하위 객체를 쉽고 빨리 설계할 수 있도록 도와줌
   - 이미 잘 개발된 객체를 재사용해서 새로운 객체를 만들기 때문에 반복된 코드의 중복을 줄여준다.
3. **다형성 (Polymorphism)**
   - 같은 타입이지만 실행결과가 다양한 객체를 이용할 수 있는 성질
   - 하나의 타입에 여러 객체를 대입함으로써 다양한 기능을 이용할 수 있게 도와준다.
<br></br>
> - [https://jwprogramming.tistory.com/121](https://jwprogramming.tistory.com/121)

<br>

### 오버로딩과-오버라이딩의-차이
1. **오버로딩(Overloading)**
  - 두 메소드가 같은 이름을 갖고 있으나 **인자의 수나 자료형**이 다른 경우
  
1-1. **오버로딩의 특징**
   1) 메소드의 이름이 같아야 한다.
   2) 리턴형이 같아도 되고 달라도 된다.
   3) 파라미터 개수가 달라야 한다.
   4) 파라미터 개수가 같을 경우, 데이터 타입이 달라야 한다.   
   \* 리턴타입은 시그니처에 포함되지 않기 때문에 주의해야 한다. 컴파일 에러가 발생함
  - 예시
  ``` java
       public double computeArea(Circle c) { ... }
       public double computeArea(Circle c1, Circle c2) { ... }
       public double computeArea(Square c) { ... }
  ```
    
1-2. **오버로딩의 장점**
  - 한 클래스 내에, 여러 개의 같은 이름의 메소드를 정의함으로써 프로그램의 가독성을 증가시킬 수 있다.

2. **오버라이딩(Overriding)**
  - **상위 클래스의 메소드와 이름과 시그니처(인자의 수, 자료형)가 같은 함수를 하위 클래스에 재정의하는 것**을 말한다.
  - 즉, 상속 관계에 있는 클래스 간에 **같은 이름의 메소드를 재정의**하는 것을 말한다.
  - 오버라이딩을 할 때 개발자의 실수를 방지하기 위해 메소드 위에 관례적으로 @Override를 기입한다.
  
2-1. **오버라이딩의 특징**
   1) 오버라이드 하고자 하는 메소드가 **상위 클래스에 존재**해야 한다.
   2) 메소드 이름이 같아야 한다.
   3) 메소드 파라미터 개수, 파라미터의 자료형이 같아야 한다.
   4) 메소드 리턴형이 같아야 한다.  
   5) 상위 메소드와 동일하거나 내용이 추가되어야 한다.
  - 예시
  ``` java
       public class Circle extends Shape {
          private double rad = 5;
          @Override // 개발자의 실수를 방지하기 위해 @Override(annotation) 쓰는 것을 권장
          public void printMe() { System.out.println("Circle"); }
          public double computeArea() { return rad * rad * 3.15; }
       }
  ```

2-2. **오버라이딩의 장점**
  - 부모로부터 받은 메소드의 로직(내부)을 입맛에 맞게 변경함으로써 객체 지향 언어의 특징인 다형성을 살려 프로그래밍을 할 수 있다.
  
<br></br>
> - [https://brunch.co.kr/@kimkm4726/2](https://brunch.co.kr/@kimkm4726/2)

<br>

### Call-by-Reference와-Call-by-Value의-차이
> **메소드(함수) 호출 방식**   
프로그래밍 언어에서는 변수를 다른 함수의 인자로 넘겨줄 수 있다.     
이 때 이 변수의 '값'을 넘겨 주는 호출 방식을 Call by Value,   
이 변수의 '참조값' (혹은 주소, 포인터)를 넘겨주는 방식을 Call by Reference라고 한다.   
자바는 Call by Value 방식으로 동작하게 된다.

- **자바의 메소드 호출 방식** ( = call by value)   
  - 예시
  ``` java
        public static void main(String[] args) {
        int a = 1;
        int b = 2;
        swap(a, b);

        System.out.println(a); //출력결과 1
        System.out.println(b); //출력결과 2
        }

        static void swap(int a, int b) {
            int tmp = a;
            a = b;
            b = tmp;
       }
  ```
 
> **자바의 함수 호출 방식이 Call by Value**이기 때문에 인자로 넘겨주었던 변수들의 값이 변경 되지 않고, 그대로 출력된다.

1. **Call-by-Value의 개념**
    - **값에 의한 호출**
    - 메소드 호출 시에 사용되는 인자의 **메모리에 저장되어 있는 값(Value)를 복사하여 보낸다.**
    - 앞선 예시와 동일한 방식으로 swap() 메소드 호출 시에 사용한 인자 a,b와   
    swap() 메소드 내의 매개변수 x,y가 서로 다르기 때문에 둘의 값이 변경되지 않는다.
![Call-by-Value](https://t1.daumcdn.net/cfile/tistory/26190835592FBA1032)

<br>

2. **Call-by-Reference의 개념**
    - **참조에 의한 호출**
    - 인자를 값이 아닌 주소(Address)로 넘겨줌으로써, 주소를 참조하여 데이터를 변경할 수 있다.
    - 메소드 호출시 사용되는 인자 값의 **메모리에 저장되어 있는 주소(Address)를 복사하여 보낸다.**
    
2-1. **Call-by-Reference의 예시**   
    - 예시
  ``` java
        Class CallByReference{
            int value;

            CallByReference(int value) {
                this.value = value;
            }

            public static void swap(CallByReference x, CallByReference y) {
                int temp = x.value;
                x.value = y.value;
                y.value = temp;
            }

            public static void main(String[] args) {
                CallByReference a = new CallByReference(10);
                CallByReference b = new CallByReference(20);

                System.out.println("swap() 호출 전 : a = " + a.value + ", b = " + b.value);

                swap(a, b);

                System.out.println("swap() 호출 전 : a = " + a.value + ", b = " + b.value);
            }
        }
  ```

> 원하는 대로 a와 b의 값이 잘 바뀌어서 출력이 된다. (호출 후 : a = 20, b = 10)

![Call-by-Reference](https://t1.daumcdn.net/cfile/tistory/2463F137592FBADE0A)
  
<br></br>
> - [https://re-build.tistory.com/3](https://re-build.tistory.com/3)

<br>

### 인터페이스와-추상-클래스의-차이
1. **추상 클래스(Abstract Class)의 개념**
    - 클래스 구현부 내부에 **추상 메소드가 하나 이상 포함되거나 abstract로 정의**된 경우를 말한다.
    - **동일한 부모를 가지는 클래스를 묶는 개념으로 상속을 받아서 기능을 확장시키는 것이 목적**이다.
  
1-1. **추상 클래스(Abstract Class)의 특징**
   - 추상 클래스는 **new 연산자를 사용하여 객체를 생성할 수 없다.**
   - 추상 클래스(부모)와 일반 클래스(자식)는 상속의 관계에 놓여있다.
   - 추상 클래스는 **새로운 일반 클래스를 위한 부모 클래스의 용도**로만 사용된다.
   - 일반 클래스들의 **필드와 메소드를 통일**하여 일반 클래스 작성 시 시간을 절약할 수 있다.
   - 추상 클래스는 단일 상속만 가능하며 일반 변수를 가질 수 있다.
    
<br>

2. **인터페이스(Interface)의 개념**
    - 모든 메소드가 추상 메소드인 경우
    - 추상 클래스보다 한 단계 더 추상화된 클래스
    - 구현 객체가 같은 동작을 한다는 걸 보장하는 것이 목적이다.
  
2-1. **인터페이스(Interface)의 특징** 
   - 인터페이스에 적는 모든 메서드들은 추상 메서드로 간주되기 때문에 abstract를 적지 않는다.
   - default 예약어를 통해 일반 메소드 구현이 가능하다.
   - static final 필드만 가질 수 있다. (public static final이 생략되어있다고 생각)
   - new 연산자를 사용해 객체를 생성할 수 없다.
   - 다중 상속이 가능하다.
  
> cf.) public static final을 사용하는 이유?   
구현 객체의 같은 동작을 보장하기 위한 목적으로 인터페이스의 변수는 스스로 초기화 될 권한이 없어서  
아무 인스턴스도 존재하지 않는 시점이기 때문이다.
 
<br>
 
3. **클래스와 인터페이스 사이 관계 이해하기**   
![상속 키워드](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLPs83%2Fbtqw0nb8nO1%2FSKpKurnKKuCeTUbjWqRLf0%2Fimg.png)
    - 클래스끼리, 인터페이스끼리 상속을 할 때는 extends, 클래스가 인터페이스를 상속받을 때는 implements 키워드를 사용

4. **추상 클래스와 인터페이스의 공통점과 차이점**
    - 공통점
        - 선언만 있고 구현 내용은 없는 클래스이다.
        - 인터페이스화(객체를 생성) 할 수 없다.
        - 추상 클래스를 extends로 상속받은 자식들과 인터페이스를 implements하고 구현한 자식들만 객체를 생성할 수 있다.
    - 차이점
        - 추상 클래스 (단일 상속) / 인터페이스 (다중 상속)
        - 추상 클래스의 목적은 상속을 받아서 기능을 확장시키는 것 (부모의 유전자를 물려받는다.)
        - 인터페이스의 목적은 구현하는 모든 클래스에 대해 특정한 메소드가 반드시 존재하도록 강제하는 역할   
            (유전자를 물려받는 것이 아니라 사교적으로 필요에 따라 결합하는 관계)   
            즉, 구현 객체가 같은 동작을 한다는 것을 보장하기 위함이다.
  
<br></br>
> - [https://cbw1030.tistory.com/47](https://cbw1030.tistory.com/47)

<br>            
