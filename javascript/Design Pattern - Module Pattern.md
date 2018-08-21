# JavaScript Module Pattern
특정 구성요소를 다른 구성요소와 독립적으로 유지하는데 가장 널리 사용되는 디자인 패턴
- 모듈 : JavaScript의 '클래스' (ES6에서는 클래스를 지원)
- 모듈 패턴은 public 및 private접근 권한 설정을 가능하게 한다.
- 특정 부분의 전역 스코프로부터 보호하는 것이 가능해진다. -> 즉, 한 스크립트 파일에서 함수이름이나 변수가 충돌하는 것을 예방할 수 있다.

### IIFE(Immediately-Invoked-Function-Expressions) 형태
모듈은 변수와 메소드를 보호하는 클로저(*함수 대신 객체를 반환해야 한다)처럼 private범위를 허용하는 IIFE어야 한다.

```js
(function() {
  // private 변수들과 함수들을 선언
  
  return {
    // public 변수들과 함수들을 선언
  }
})();
```
이처럼 구현한다면, 클로저 외부에 있는 코드는 동일한 범위에 있지 않으므로 private변수에 접근할 수 없다.
  
좀 더 자세한 예제를 들어보자.

```js
var module = (function() {
  var counter = 0;
  
  return {
    increment: function() {
      return counter++;
    },
    
    init: function() {
      counter = 0;
    }
  }
})();

module.increment();
module.init();
```
위 코드의 핵심은 increment(), init() 메소드를 절대로 직접접근할 수 없다는 것이다.
그리고 counter변수는 전역 스코프로부터 보호를 받고 있다. 즉, 내부의 모든 리소스는 module을 통해서만 접근이 가능하다.

  
