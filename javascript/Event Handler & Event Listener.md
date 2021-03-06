# Event Handler vs Event Listener
## Event
- 사용자 또는 브라우저가 취하는 특정 동작
- ex. click, load, mouseover...etc

## Event Handler
- 이벤트가 발생했을 때 그 처리를 담당하는 실행 함수
- 특정 요소에서 발생한 이벤트를 처리하기 위해서는 이벤트 핸들러(event handler)라는 함수를 작성하여 연결해야만 한다.
- 이벤트 핸들러가 연결된 특정 요소에서 지정된 타입의 이벤트가 발생하면, 웹 브라우저는 이벤트 리스너에 연결된 이벤트 핸들러를 실행한다.
- 이벤트 핸들러 함수는 이벤트 객체(event object)를 인수로 전달받을 수 있다. 이렇게 전달받은 이벤트 객체를 이용하여 이벤트 성질을 결정하거나, 이벤트의 기본 동작을 막을 수도 있다.

## Event Listener
- 특정한 이벤트에 대해서 일어날 동작을 정의할 수 있다.
- 지정된 타입의 이벤트가 특정 요소에서 발생하면, 웹 브라우저는 그 요소에 등록된 이벤트 리스너를 실행시킨다.
- 이벤트 리스너와 이벤트 핸들러를 합쳐 이벤트 리스너라고 하기도 한다.

## Example
* 이벤트 : click
* 이벤트 속성 (이벤트 리스너) : onclick
* 이벤트 핸들러 : function(){}

## Event Listener 등록 방법
- 작성된 이벤트 리스너는 먼저 해당 객체나 요소에 등록되어야만 호출될 수 있다.
- 객체나 요소에 이벤트 리스너를 등록하는 방법은 다음과 같다.
	
	1. 객체나 요소에 **프로퍼티**로 등록하는 방법
	2. 객체나 요소에 **메소드**로 이벤트 리스너를 전달하는 방법

## 이벤트 호출 순서
- `addEventListener()`메소드를 사용하면 하나의 이벤트 타입에 여러 개의 이벤트 리스너를 등록할 수 있다. 
- 이때, 특정 타입의 이벤트가 발생하면 브라우저는 다음과 같은 순서로 이벤트를 호출하게 된다.

	1. 이벤트의 대상이 되는 객체나 요소에 **프로퍼티**로 등록한 이벤트 리스너가 가장 먼저 호출된다.
	2. **`addEventListener()`**메소드를 사용하여 등록한 이벤트 리스너를 등록한 순서대로 호출한다.

	
	
[참고](http://codedragon.tistory.com/5743)
	
	
