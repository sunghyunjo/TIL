# call by reference vs call by value
함수에 인자로 무엇을 던지느냐에 다라서 함수가 실행되는 방법을 결정하는 것을 `call by ...` 형태로 표현한다. 여러가지 호출 방법들이 있지만 대표적인 `call by value`와 `call by reference`에 대해 살펴보자.

## parameter vs arguments
`call by value`에 들어가기 전에 `parameter(매개변수)`와 `arguments(인자)`에 대해 먼저 알아보자. 

- parameter(매개변수)는 formal parameter(형식 매개변수)로 인식하면 되고, arguments는 actual parameter(실인자)로 받아들이면 된다.

Ex.

```js
var a = 1;
var func = function(b) { // parameter, formal parameter
	// code...
};

func(a); // arguments, actual parameter
```

parameter, formal parameter는 b가 되는 것이고, 
실제로 넘어가는 값인 arguments, actual parameter는 1이 되는 것이다.

**parameter는 함수 선언부에 정의되고, arguments는 함수 호출부에서 사용된다.**

## call by value
- arguments로 값이 넘어온다.
- 값이 넘어올 때 복사된 값이 넘어온다.
- caller(호출하는 녀석)가 인자를 복사해서 넘겨줬으므로 callee(호출당한 녀석)에서 해당 인자를 아무리 변경해도 caller는 영향받지 않는다.

Ex.

```js
var a = 1;
var func = function(b) { // callee
	b = b + 1;
}

func(a); // caller
console.log(a); // 1

```

기본적으로 자바스크립트는 원시값을 arguments로 넘겨주면 `call by value`형태로 작동한다.
따라서 caller가 1을 arguments로 넘겨줘도 그 값은 복사되어 넘어오기 때문에, callee내부에서 변경해도 전혀 영향받지 않는다. 그로 인해, a값은 원래대로 1이 출력된다.

## call by reference

- arguments로 reference(값에 대한 참조 주소, 메모리 주소를 담고있는 변수)를 넘겨준다.
- reference를 넘기다 보니 해당 reference가 가리키는 값을 복사하지는 않는다.
- caller(호출하는 녀석)가 인자를 복사해서 전달하는 것이 아니므로 callee(호출당한 녀석)에서 변경하면 caller도 영향을 받는다.

Ex.

```js
var a = {};
var func = function(b) { // callee
	b.a = 1;
}

func(a); // caller
console.log(a.a); // 1
```

## 자바스크립트에서의 동작 방식
```js
var a = {};
var func = function(b) { // callee
	b = 1;
}

func(a); // caller
console.log(a); // {}	
```
참조 타입을 넘겼는데 값이 변하지 않았다. 
왜냐하면, 자바스크립트에서는 무조건 `call by value`로 작동하기 때문이다.
사람들이 참조 타입을 넘기면 `call by reference`로 작동한다고 알고 있지만 위 코드를 보면 절대 그렇지 않다. 

자바스크립트에서는 참조 타입을 인자로 넘기면 참조값에 대한 복사본을 만들어서 넘긴다.

### 정리하자면,
**자바스크립트는 항상 Call by value이며, 객체나 배열 같은 참조형(Reference) 타입인 경우에도 실제로는 그 복사본을 만들어 value로 parameter를 전달한다.**

- [참고문서 1](https://blog.perfectacle.com/2017/10/30/js-014-call-by-value-vs-call-by-reference/)
- [참고문서 2](http://emflant.tistory.com/64)
