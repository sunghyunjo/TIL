# 싱글톤 패턴 (Singleton Pattern)

- 싱글톤 패턴은 전체 시스템에서 하나의 인스턴스만 존재하도록 보장하는 객체 생성패턴이다.
- 따라서 객체 리터럴도 싱글톤 패턴이라고 할 수 있다.

```js
var singletonObj = {
	a : '값',
	b : function () {
	}
}
```
그러나, 객체 리터럴로는 비공개 상태 및 함수를 정의할 수는 없다. 때문에 규모가 큰 라이브러리에서는 클로저를 통해 비공개 멤버를 가진 싱글톤 객체를 생성한다.

다음은 클로저를 이용한 싱글톤 패턴을 구현하는 예제이다.

```js
var Singleton = (function() {
	// 비공개 변수, 메서드 정의
	var instantiaed;
	
	function init() {
		// 싱글톤 객체 정의
		return {
			// 공개 메서드 정의
			publicMethod: function () {
				return 'hello Singleton Pattern!!!';
			},
			// 공개 프로퍼티 정의
			publicProp: 'single value'
		}
	}
	
	// 공개 메서드인 getInstance()를 정의한 객체.
	// 렉시컬 특성으로 인해 비공개 변수, 메소드에 접근 가능(클로저)
	return {
		getInstance: function() {
			if (!instantiaed) {
				instantiaed = init();
			}
			return instantiaed;
		}
	}
})();

// 싱글톤 객체 생성하여 publicMethod 호출 가능해짐.
var first = Singleton.getInstance();
first.publicMethod();
console.log(first.publicMethod()); // hello Singleton Pattern!!!

var second = Singleton.getInstance();
second.publicMethod();
console.log(second.publicMethod()); // hello Singleton Pattern!!!

console.log(first === second); // true
```

싱글톤 객체는 프로그램 전체에서 하나만 존재하게 된다. 
외부에 공개되는 익명함수의 return 문에서는 싱글톤 객체를 구하는 공개 메서드인 getInstance가 포함된 객체를 반환한다.
getInstance메서드에서는 내부 변수로 정의된 instantiaed 변수의 값을 확인해서 아직 싱글톤 객체가 생성되지 않았다고 판단하면 내부 함수인 init을 호출하여 싱글톤 객체를 생성해서 반환하게 된다. 
그런 다음 instantiaed변수에 싱글톤 객체를 할당하게 된다.

이렇게 일반적으로 싱글톤 패턴에서는 이미 객체가 생성됐는지 여부를 알려주는 instantiaed 와 같은 내부 변수가 필요하다.
그리고 싱글톤 패턴에서는 내부 변수에 접근할 수 있는 객체를 반환하는 클로저를 이용해야 한다.

위 코드에서는 변수의 렉시컬한 특성으로 인해 내부의 getInstance함수에서 비공개 변수인 instantiaed에 접근할 수 있다는 것과 getInstance() 호출이 끝나더라도 instantiaed 값은 계속 유지되는 특성(클로저)를 이용해 publicMethod(), publicProp이 포함된  객체를 유일하게 생성한다.

그래서 `Singleton.getInstance();`을 몇 번 호출하더라도 결과로 얻는 객체는 모두 동일한 싱글톤 객체를 가리키게 된다.

[참고문서](https://webclub.tistory.com/150#recentEntries)
