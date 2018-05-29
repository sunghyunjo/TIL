# JavaScript의 Function


-----

### JavaScript에서 function의 정의
* 독립적으로 분리된 로직으로서 프로그램 수준에서 미리 정의되어 있거나, 사용자 정의에 의해서 만들어진 실행단위.
* 자바스크립트의 Function은 일급객체로써 변수나 데이터 구조 안에 담을 수 있으며, 인자로 전달할 수도, 반환 값으로도 사용할 수 있으며, 런타임에 생성할 수도 있다.

자바스크립트에서 function은 일급객체 이면서 모든 함수객체의 프로토타입인 Function Prototype Object의 생성자 함수가 가리키는 class개념이기도 하다. function또한 일반 객체처럼  취급될 수 있다. 그래서 익명함수를 비동기 함수의 callback과 같은 형식으로 넘길 수 있는 것이다.
<br>
</br>
## 함수선언(Function Declarations)

* 함수선언은 Function Statement라고도 하며 말 그대로 함수 문장이라는 뜻. 이는 곧 실행가능한 코드블럭이 아니며, 함수의 정의를 나태는 문장으로 해석되며 따라서 코드 해석에 따른 수행결과가 존재하지 않는 다는 것을 의미한다.

Statement라고 하는 말은 위에서 말한 것처럼 코드블럭 자체는 실행가능한 코드가 아니라는 것이다. 즉, 해당 코드블럭을 콘솔에서 실행하여도 어떠한 결과도 return되지 않는다. 이러한 이유로 함수선언문을 Class와 동일한 개념으로 이해해도 무방하다.
```
function foo(){
		console.log("Hello");
	}
```
이러한 statement는 console에서 실행하여도 아무런 결과를 반환하지 않는다.
![](http://insanehong.kr/post/javascript-function/@img/state.jpeg)

<br>
## 함수표현(Function Expression)
* 함수 표현은 Function Literal이다. 이는 실행 가능한 코드로 해석 되어지거나 변수나 데이터구조에 할당되어지고 있음을 의미한다. 즉, 해당 코드블럭이 실행코드로서 해석되어지며 동시에 코드실행에 따른 결과값을 가지거나 특정 변수에 할당된 값으로 존재한다.

이처럼 함수표현은 함수리터럴로 특정변수에 할당되거나 즉시 실행가능한 코드 블럭으로서 존재하는 함수를 의미하는 것이다. 함수표현이 실행코드로서 해석되기 위해서는 ()를 이용하여 함수를 감싸 주어야 한다. 이를 자기호출함수(self invoking function)라고도 한다. 자기호출함수는 재귀함수와는 다른 개념이다. 재귀함수는 함수 안에서 자신을 호출하는 형식이지만, 자기호출함수는 해석과 동시에 실행되는 코드블럭을 말한다.
```
//anonymous function expression
var foo = function() {
		console.log("hello");
};

//named function expression
var foo = function foo() {
		console.log("hello");
};

//self invoking function expression
(function foo() {
		console.log("hello");
})();
```

위의 코드에서 named function expression은 매우 특이하다. foo라는 변수에 이름있는 함수를 할당하고 있다. 흔히 알고 있는 함수리터럴과는 좀 거리가 있다. 하지만 이 named function expression에는 한 가지 특징이 있다. 바로 해당 함수의 이름은 함수 밖에서는 사용할 수 없다는 것이다.
```
var foo = function A() {
		A(); // 실행가능
}

A(); // Syntax Error
```

즉, 재귀호출외에는 그다지 쓸 데가 없다. 함수표현은 자기호출함수를 이용하여 콘솔에서 실행결과를 받을 수 있다.
![](http://insanehong.kr/post/javascript-function/@img/func.jpeg)
이들 함수선언과 함수표현은 함수호출 방법에서는 큰 차이가 없다. 하지만, "호이스팅(hoisting)관점에서는 큰 차이"를 보이게 된다.

<br>
</br>
## 호이스팅(hoisting)
* 인터프리터가 자바스크립트 코드를 해석함에 있어서 Global영역에 선언된 코드블럭을 최상의 Scope로 끌어올리는 것을 호이스팅이라고 한다.

즉, 글로벌 영역에 선언된 변수 또는 함수는 자바스크립트 엔진이 가장 먼저 해석하게 된다. 단, 할당된 구문은 런타임과정이 이루어지기 때문에 hoisting되지 않는다. 

쉽게 말하자면, 선언문은 항시 자바스크립트 엔진 구동 시 가장 최우선으로 해석된다라고 말할 수 있다. 즉, 인터프리터가 자바스크립트의 코드를 해석하면서 코드작성 순서에 상관없이 global영역에 선언되어 있는 변수나 함수의 선언문들을 먼저 수집하여 전역객체의 속성으로 등록시켜 둔다는 것이다. 그렇기 때문에 전역변수와 전역함수는 자바스크립트 코드의 어떠한 위치에서도 실행이 가능한 것이다.

하지만, 같은 이유로 너무 많은 선언문이 남발되어 있는 자바스크립트 코드는 실행코드의 해석시점이 뒤로 밀리게 됨으로써 자바스크립트 실행코드의 구동시점이 길어지는 좋지않은 결과를 가져오기도 한다. 

이 호이스팅에서 중요한 부분은 선언문은 호이스팅되지만, 할당은 호이스팅되지 않는 다는 것이다. 즉, {}안의 내용은 포함하지만 = 연산자를 사용한 값은 호이스팅되지 않는다.

```
// 예제 1 : 함수선언에서의 호이스팅
foo();
function foo () {
	console.log("hello");
};
> hello

//예제 2 : 함수표현에서의 호이스팅
foo();
var foo = function () {
	console.log("hello");
};
> Syntax Error
```

위 코드에서 알 수 있듯이 함수선언은 foo();라는 실행코드를 해석하기 이전에 foo함수에 대한 선언을 호이스팅하여 global객체를 등록시키기 때문에 오류 없이 수행된다. 

이러한 호이스팅은 자바스크립트 코드를 사람이 해석하는데 많은 혼란을 주게 된다. 그런 이유로 자바스크립트 코드를 작성할 때 선언문은 항상 최상위에서 작성하는 것을 권고하고 있다. 

그럼 지금까지ㅏ 이해한 내용을 가지고 아래의 퀴즈들을 풀어보도록 하자. 아래의 퀴즈들을 보고 바로 답이 나온다면, 더 이상 이 글을 읽을 필요는 없을 것이다.

<br>
</br>
## Quiz
```
//Question 1:
function foo() {
	function bar() {
		console.log("hello");
	}
	
	return bar();
	
	function bar() {
		console.log("world");
	}
}
foo();
```
```
// Question 2:
function foo() {
	var bar = function() {
		console.log("hello");
	};
	
	return bar();
	
	var bar = function () {
		console.log("world");
	};
}
foo();
```
<br>
</br>


-----


## Question 풀이
### Question 1.
```
정답 : world
```
```
function foo() {
	function bar() {
		console.log("hello");
	}
	
	return bar();
	
	function bar() {
		console.log("world");
	}
}
foo();
```
자바스크립트 엔진이 구동되는 시점에 global영역에 선언된 foo함수를 호이스팅한다. 이 때 foo의 선언문인 {}블럭 안의 내용 또한 같이 따라 올라간다. 런타임에서 해석되는 실행코드보다 선언문이 먼저 해석되기 때문에 return bar();라는 실행문이 해석되기 전에 function bar()로 선언된 함수선언문이 먼저 해석 되어 지고 두번째로 선언된 bar함수 역시 return구문보다 먼저 해석되어 진다. 그렇기 때문에 퀴즈 1번에서 실행코드를 만나기 전까지 자바스크립트가 해석한 foo함수는 아래와 같다. 
```
function foo(){
    function bar() {
        console.log('hello');
    }

   function bar() {
        console.log('world');
    }
    return bar();
}
```
foo() 호출되고 런타임 과정에서 return bar() 라는 실행문은 가장 나중에 선언된 함수를 실행하게 되고 'world'를 출력한다. 여기까지 이해가 완벽하게 되었다면 너무 당연하겠지만 같은 이유로 아래의 코드 역시 정상적으로 실행되며 'world'를 출력한다.
```
foo();
function foo () {
	function bar () {
		console.log("hello");
	}
	return bar();
	function bar() {
		console.log("world");
	}
}
```
<br>
### Question 2.
```
정답 : hello
```
```
function foo(){
    var bar= function() {
        console.log('hello');
    };
    return bar();
    var bar = function() {
         console.log('world');
    };
}
foo();
```
2번 퀴즈에서 역시 foo 함수는 hoisting 되어 런타임이전에 미리 해석되어 진다. 하지만 {}안에 작성된 var bar 라는 변수는 함수리터럴을 할당받고 있다. 할당은 런타임 시점에서 이루어지고 자바스크립트 엔진은 런타임 이전까지는 강제적으로 undefinde 를 할당하게 된다. 즉 hoisting 되어진 foo 함수안에 구조는 실행코드를 만나기 전까지는 아래와 같이 해석되어 진다.
```
function foo(){
    var bar= undefined;
    var bar= undefined;
    bar= function() {
        console.log('hello');
    };
   return bar();
}
```
2번째로 작성된 함수표현식은 실행조차 되지 않는다. 런타임에서는 자바스크립트 엔진이 해석한 순서로 실행되기 때문에 할당구문보다 먼저 return 이 실행되어 버린다. 그렇기 때문에 2번 퀴즈는 'hello' 를 출력한다. 같은 이유로 아래의 코드는 Syntax Error 를 뱉어내게 된다.
```
function foo(){
   return bar(); 
   var bar= function() {
        console.log('hello');
    };
    var bar = function() {
         console.log('world');
    };
}
foo();
```

# 마무리하며
사실 호이스팅을 사용한 코드작성은 절대 추천하고 싶지 않다. 코드 가독성이 너무나도 떨어지게 되어 코드를 해석하는 데 있어 혼란을 겪게 될 수 있기 때문이다. 


[원문](http://insanehong.kr/post/javascript-function/)
