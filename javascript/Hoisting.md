# 호이스팅(Hoisting)
호이스팅이란 단어를 직역하자면 끌어올리기, 들어올리기 라는 뜻을 가지고 있다.
자바스크립트에는 호이스팅이란 개념이 중요하게 여겨지고 있는데, 이는 간단하게 말하면 **변수 선언문을 끌어올린다는 뜻**으로 이해하면 된다.
  
아래 코드를 통해 이해해보자.
```js
function foo() {
	console.log(val);
	var val = 10;
	console.log(val);
}
foo();
/*
실행결과 
undefined
10
*/
```

위의 코드를 보면 foo함수 안에서 변수 val의 호출이 2번 일어난다.
한 번은 변수 선언문 전에, 또 한번은 변수가 선언된 이후에 호출이 되는데, 다른 프로그래밍 언어는 선언문 전에 호출이 됐다면 Error가 발생하게 된다. 
하지만, javascript는 val이 호이스팅되며 아래의 코드와 같은 맥락으로 구동된다.

```js
function foo() {
	var val;
	console.log(val);
	val = 10;
	console.log(val);
}
foo();
```

즉, 변수 선언은 가장 위쪽으로 끌어올려지게 되고, 선언문이 있던 자리에서 변수가 초기화되는 결과가 발생하는 것이다. 
그 실행결과로써 첫 호출에서는 초기화 전의 val값인 undefined가, 두번째 호출에서는 10으로 초기화 된 값이 출력되게 되는 것이다.

#### 두번째 예제를 살펴보자.

```js
var val = 30;
function foo() {
	console.log(val);
	var val = 10;
	console.log(val);
}
foo();

/*
실행결과 
undefined
10
*/
```

다른 프로그래밍 언어에 익숙한 개발자들은 변수 value의 첫 호출에서 전역변수가 참조된다고 생각할 수 있다. 하지만, javascript의 호이스팅으로 인해 선언부가 foo함수의 가장 최상단으로 끌어올려지고, 이로써 전역변수val이 아닌, 지역변수 val을 참조하게 된다.

### 기억해야 할 것! 함수 선언문(function declaration)방식만 호이스팅이 가능하다.
```js
// 함수 선언문
foo();

function foo() {
	var val = 10;
	console.log(val);
}

/*
실행결과
10
*/
```

```js
// 함수 표현식
foo();

var foo = function() {
	var val = 10;
	console.log(val);
}

/*
실행결과
foo of object is not a function
*/
```

```js
// function 생성자
foo();

var foo = new Function("", "return console.log('foo');")

/*
실행결과
foo of object is not a function
*/
```

위의 세 가지 예제 코드와 실행결과를 보자면 함수 선언문 방식만 호이스팅이 이뤄진 것을 볼 수 있다.
그 이유는 함수 표현식과 function생성자를 통한 함수 정의 방식은 변수에 함수를 초기화시키기 때문에, 선언문이 호이스팅 되어 상단으로 올려진다 하여도 함수가 아닌 변수로 인식되기 때문이다.

> 위 코드에서 함수 실행 구문이 아닌, 변수를 호출하게 되면 변수가 호이스팅된 것처럼 undefined란 결과가 나오게 된다.

[참고](http://www.nextree.co.kr/p7363/)
