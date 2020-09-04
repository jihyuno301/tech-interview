# 2. Java - Part2
### :book: Contents
* [OOP의 4가지 특징](#OOP의-4가지-특징)
* [OOP의 5대 원칙 (SOLID)](#OOP의-5대-원칙-(SOLID))
* [객체지향 프로그래밍과 절차지향 프로그래밍의 차이](#객체지향-프로그래밍과-절차지향-프로그래밍의-차이)
* [객체지향(Object-Oriented)이란](#객체지향(Object-Oriented)이란)
* [java의 non-static 멤버와 static 멤버의 차이](#java의-non-static-멤버와-static-멤버의-차이)
  * [Q. java의 main 메서드가 static인 이유](#)
* [java의 final 키워드 (final/finally/finalize)](#java의-final-키워드-(final/finally/finalize))
* [java의 제네릭(Generic)과 c++의 템플릿(Template)의 차이](#java의-제네릭(Generic)과-c++의-템플릿(Template)의-차이)

---

### OOP의 4가지 특징
1. 추상화(Abstraction)
    * 구체적인 사물들의 공통적인 특징을 파악해서 이를 하나의 개념(집합)으로 다루는 것
    * 각 객체의 구체적인 개념에 의존하지 말고 추상적 개념에 의존해야 설계를 유연하게 변경할 수 있다.
    * ![추상화image](https://gmlwjd9405.github.io/images/oop-features/abstract.png)
2. 캡슐화(Encapsulation)
    * **참고** SW 공학에서 요구사항 변경에 대처하는 고전적인 설계 원리
    * 높은 응집도와 낮은 결합도를 유지할 수 있도록 설계해야 요구사항을 변경할 때 유연하게 대처할 수 있다.
      1. 응집도(Cohesion)
         * 클래스나 모듈 안의 요소들이 얼마나 밀접하게 관련되어 있는지를 나타낸다.
      2. 결합도(Coupling)
         * 어떤 기능을 실행하는 데 다른 클래스나 모듈들에 얼마나 의존적인지를 나타낸다. 
    * 캡슐화는 **정보 은닉**을 통해 **높은 응집도와 낮은 결합도**를 갖도록 한다.
      * 정보 은닉(information hiding)
        * 필요가 없는 정보는 외부에서 접근하지 못하도록 제한하는 것
        * private 키워드
      * 정보 은닉이 왜 필요할까?
        * SW는 결합이 많을수록 문제가 많이 발생한다.
        * 한 클래스가 변경이 발생하면 변경된 클래스의 비밀에 의존하는 다른 클래스들도 변경해야 할 가능성이 커진다는 뜻이다. 
3. 상속(Inheritance)
    * 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것이다.
    * 보다 적은 코드로 새로운 클래스를 작성할 수 있고, 코드를 공통적으로 관리할 수 있기 대문에 코드의 추가 및 변경이 매우 용이하다.
    * 이러한 특징은 코드의 재사용성을 높이고 코드의 중복을 제거하여 프로그램의 생산성과 유지보수에 크게 기여한다.
4. 다형성(Polymorphism)
    * 서로 다른 클래스의 객체가 같은 메시지를 받았을 때 각자의 방식으로 동작하는 능력
    * 같은 이름의 메소드를 여러개 정의(오버로딩)
    * **오버로딩**의 장점 : 하나의 이름을 정의함으로써 하나의 이름만 기억하면 되므로 기억하기도 쉽고 같은 기능을 한다는 것을 쉽게 예측할 수 있다.
> [https://gmlwjd9405.github.io/2018/07/05/oop-features.html](https://gmlwjd9405.github.io/2018/07/05/oop-features.html)

### OOP의 5대 원칙
"**SOLID**" 원칙
* **S**: 단일 책임 원칙(SRP, Single Responsibility Principle)
  * 객체는 단 하나의 책임만 가져야 한다.
* **O**: 개방-폐쇄 원칙(OCP, Open Closed Principle)
  * 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다.
* **L**: 리스코프 치환 원칙(LSP, Liskov Substitution Principle)
  * 일반화 관계에 대한 이야기며, 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다.
* **I**: 인터페이스 분리 원칙(ISP, Interface Segregation Principle)
  * 인터페이스를 클라이언트에 특화되도록 분리시키라는 설계 원칙이다.
* **D**: 의존 역전 원칙(DIP, Dependency Inversion Principle)
  * 의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는 것에 의존하라는 것이다.
> [https://gmlwjd9405.github.io/2018/07/05/oop-solid.html](https://gmlwjd9405.github.io/2018/07/05/oop-solid.html)
