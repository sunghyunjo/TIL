# Closure (클로저)
자바스크립트의 [Scope Chain](https://github.com/sunghyunjo/TIL/blob/master/javascript/ScopeChain.md)을 이용하여 이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 방법이다. 외부함수가 종료되더라도 내부함수가 실행되는 상태라면, 내부함수에서 참조하는 외부함수는 닫히지 못하고, 내부함수에 의해 닫히게 되어 Closure(클로저)라고 불린다.
> 따라서, 클로저란 외부에서 내부 변수에 접근할 수 있도록 하는 함수이다.

   
다음 예시들을 통해 클로저를 정확하게 파악해보자.
```
function foo() {
	var color = 'blue';
	function bar() {
		console.log(color);
	}
	bar();
}

foo();
```

과연 **bar**함수는 클로저일까?
일단, **bar**는 **foo**안에 속하기 때문에 **foo**스코프를 outer lexical environment(외부 스코프)참조로 저장한다. 
그리고, **bar**함수는 자신의 렉시컬 스코프 체인을 통해 **foo**의 **color**를 참조할 것이다.

하지만, 그렇다고 클로저라고 부를 수 있는 것은 아니다. **bar**는 **foo**안에서 정의되고 실행되었기만할 뿐, **foo**밖으로 나오진 않았기 때문에 클로저라고 부르진 않는다.
  

아래의 코드는 우리가 부르는 클로저의 형태를 띄고 있다.
```
var color = 'red';
function foo() {
	var color = 'blue';
	function bar() {
		console.log(color);
	}
	return bar;
}
var baz = foo();
baz();
```
코드를 자세히 살펴보자면,
**bar**함수는 **outer environment(외부 스코프)**의 참조로 **foo**함수의 **environment**를 저장하였다.
이 때, global에서 **baz**함수를 호출하면, **foo**가 실행되고, 그 안의 return값인 **bar**함수가 실행된다. 
**bar**함수는 자신의 스코프에서 **color**값을 찾지만, 자신의 스코프에서 **color**가 지정되어 있지 않기때문에, outer environment참조를 찾아가 **foo**함수의 스코프를 탐색하여 **color**값을 찾는다. 
이로써 color는 **blue**가 출력되는 것이다.
  
이 형태가 우리가 말하는 클로저이다.

## 클로저의 반복문 예시
```
function count() {
	var i;
	for (i = 1; i < 10; i += 1) {
		setTimeout (function timer() {
			console.log(i);
		}, i * 100);
	}
}
count();
```
우리가 일반적으로 생각하는 위 코드의 목표는 1, 2, 3, ..., 9를 0.1초마다 출력하는 것일거다.
하지만, 실제 실행 결과로는 10이 9번 출력되었다.

**그 이유는,** timer는 클로저로 언제 어디서 호출되던지 항상 상위 스코프인 count에게 i를 알려달라고 할 것이다. 그러나, 첫 0.1초가 지날 동안 이미 i는 10이 되었기 때문에 timer가 0.1초 주기로 i를 불러올 때마다 이미 10이 되어버린 i만을 출력할 수 밖에 없는 것이다.
  
  
그렇다면, 1~9를 의도대로 0.1의 간격을 두고 출력하려면 어떻게 해야 할까?
1. 새로운 스코프를 추가하여 반복 시마다 그곳에 각각 따로 값을 저장하는 방식
2. ES6에서 추가된 블록 스코프를 이용하는 방식

이렇게 두 가지 방법을 써야 한다. 하나씩 살펴 보자.

### 1. 새로운 스코프를 추가하여 반복 시마다 그곳에 각각 따로 값을 저장하는 방식
```
function count() {
	var i;
	for (i = 1; i < 10; i += 1) { 
		(function(countingNumber) {
			setTimeout(function timer() {
				console.log(countingNumber);
			}, i * 100);
		})(i);
	}
}
count();
```
  
### 2. ES6에서 추가된 블록 스코프를 이용하는 방식
```
function count() {
	'use strict';
	for (let i = 1; i < 10; i+= 1) {
		setTimeout(function timer() {
			console.log(i);
		}, i * 100);
	}
}
count();
```


[참고 : 카일 심슨, 『You don't know JS - 스코프와 클로저』, 최병현, 한빛미디어(2015)]
[참고문서](http://meetup.toast.com/posts/86)
