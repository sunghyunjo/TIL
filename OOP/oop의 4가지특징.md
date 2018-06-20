# 객체지향의 4가지 특징

## 1. 추상화(Abstraction)
- 객체를 Class로 설계하는 과정을 추상화라고 부른다.
- 객체들의 공통적인 특징(속성, 기능)을 뽑아내는 것이다.
- 즉, 우리가 구현하는 객체들이 가진 공통적인 데이터와 기능을 도출해 내는 것을 의미한다.
  
## 2. 캡슐화(Encapsulation)
- Class의 Logic이나 상태를 외부에서 알 수 없도록 숨기는 과정을 일컫는다. 
- Class의 변수나 메소드에 붙는 private/public keyword는 클래스를 캡슐화시키는 중요한 문법이다. 
- Class간의 Interface를 이용한 느슨한 결합도 캡슐화를 잘 이용하는 하나의 예가 될 수 있다.
- 데이터 구조와 데이터를 다루는 방법을 결합시켜 묶는 것을 말한다.
- 캡슐화는 데이터를 은닉하고, 그 데이터를 접근하는 기능을 노출시키지 않는다는 의미로 사용하기도 한다. 즉, 데이터를 기능이라는 캡슐로 보호한다는 것이다.

## 3. 상속성(Inheritance)
- Class의 변수와 Method를 물려받아 Class를 정의하는 방법을 상속이라 부른다.
- 상위개념의 특징을 하위 개념이 물려받는 것을 말한다.
- 자식 Class는 물려받은 method를 재정의 할 수 있는데 이러한 기능을 overwriting이라고 부른다.

## 4. 다형성(Polymorphism)
Class의 같은 Method를 호출해도 각기 다른 method가 호출되는 특징을 다형성의 특징이라 부른다.
- method의 이름은 같지만, mothod의 파라미터의 타입, 파라미터의 개수, return type에 따라 실제로 다른 메소드가 호출될 수 있도록 구현할 수 있는데 이것을 Overloading이라고 부른다.
- Instance에 따라서도 다른 메소드가 호출될 수 있다.

[참고문서1](http://ssup2.tistory.com/entry/OOPObject-Oriented-Programming-4%EA%B0%80%EC%A7%80-%ED%8A%B9%EC%A7%95)
[참고문서2](http://sesok808.tistory.com/31)