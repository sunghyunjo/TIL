# JavaScript Observer Pattern
어플리케이션의 한 부분이 변경되었을 때, 영향받는 다른 부분도 같이 변경되어야 하는 경우 사용한다.
객체가 수정되면 종속 객체에 변경 사항을 알려준다.
 
- 이벤트 핸들러 : 브라우저에서의 옵저버 역할 (이벤트 발생 후, 콜백 함수가 실행되기 전까지의 과정이 옵저버 패턴을 활용한 한 예이다.)
  
### Example
아래의 스토리를 코드로 작성해보자.
- 14학번 대표였던 성현이는 항상 주변 상황과 인물들을 주의깊게 살펴보고 있었다. 그 중, 다혜도 성현이의 관찰 대상 중 하나였다.
- 다혜가 어떤 행동을 할 때마다 성현이는 그에 대한 소식을 들으며, 나름대로 평가를 내리고 있었다.
- 여러가지 소식을 통해 성현이는 다혜가 상당히 괜찮은 사람이라고 생각하게 됐다.
- 그 때 마침, 미디어학과 학생들이 성현이를 과 회장으로 추대한다. 
- 야욕이 없던 성현이는 그 간의 평가를 바탕으로 다혜를 황제로 추대한다.
=> 즉, 다혜의 행동 하나 하나로 성현이에게 점수를 따는 과정을 코드로 만들어 보자.

```javascript
var Dahye = (function() {
  function Dahye() {
    this.subscribers = [];
  }
  
  Dahye.prototype.publish = function() {
    var self = this;
    this.subscribers.every(function (subscriber) {
      subscriber.fire(self);
      return true;
    });
  };
  
  Dahye.prototype.register = function (target) {
    this.subscribers.push(target);
  };
  
  return Sunghyun;
})();

var Sunghyun = (function() {
  function Sunghyun() {
    this.list = [];
  }
  
  Sunghyun.prototype.subscribe = function (target) {
    this.list.push({
      target,
      point: 0,
    });
    target.register(this);
  };
  
  Sunghyun.prototype.unsubscribe = function (target) {
    this.list.filter(function (person) {
      return person.target !== target;
    });
   };
   
   Sunghyun.prototype.fire = function (target) {
    this.list.some(function (person) {
      if (person.target === target) {
        ++ person.point;
        return true;
      }
    });
  };
  return Sunghyun;
})();
```

```
var dahye = new Dahye();
var sunghyun = new Sunghyun();

sunghyun.subscribe(dahye);
dahye.publish();

sunghyun.list; // [{ target: Dahye, point: 1 }]
```
  
코드를 보면, **publish**와 **subscribe** 그리고 **fire** 메소드가 있다.
즉, 먼저 관찰 대상을 `subscribe()`한 후, 관찰 대상은 특정 행동을 할 때 자신의 행동을 `publish()`한다.
`publish()`한 행동은 `subscribe()`한 사람들에게 전달된다.
그 전달 방식이 `fire()`이다. `fire()`부분은 콜백이라고 보면 된다.
성현이가 다혜를 관찰하고 있다가, 다혜가 특정 행동을 하기 때문에 자동으로 성현이에게 그 소식이 전달되는 것이다.

[참고자료](https://www.zerocho.com/category/JavaScript/post/5800b4831dfb250015c38db5)








