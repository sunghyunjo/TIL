# call() vs apply() vs bind() 
javascript의 this바인딩이 어지럽게(?) 되면서, 자신이 원하는 형태로 this를 바인딩하기 위한 방법으로 call, apply, bind 메소드가 사용되어 진다.
  
이 문서는 기본적으로 자바스크립트에서 this가 어떤 형태로 바인딩 되는지 알고 있다는 전제하에 쓰여졌다.
  
## bind()
call(), apply()와 비슷한 기능을 갖고 있지만, 함수 자체를 넘길 수 있는 방법이다.
예제 코드를 통해 살펴보자.
```
window.color = 'red';
var o = {color : 'blue'};

function sayColor() {
	return this.color;
}

var sayHelloColor = sayColor.bind();
sayHelloColor();
// 실행결과 : 'red';

var oColor = sayColor.bind(o);
oColor();
// 실행결과 : 'blue';
```

코드를 해석해보자면, sayHelloColor에는 ayColor함수를 전달인자 없이 bind하였다.
이로써 sayHelloColor에는 sayColor함수가 선언되어, sayHelloColor()를 실행하면 sayColor함수가 실행되게 되는 것이다.
이 때, 반환되는 값인 this.color의 this는 아직 전역객체인 window를 바라보고 있기 때문에 window.color인 'red'가 출력된다.
    

그러나 두번째 변수인 oColor에는 o객체를 바라보도록 bind되었다. 
이로써 oColor의 this는 o 객체를 바라보게 되고, 이로써 this.color는 'blue'가 할당되게 된다.

## call()
bind()와 비슷한 기능을 하는 메소드이다.
call과 apply는 bind처럼 함수 전체를 넘기는 것이 아닌, this를 원하는 객체로부터 호출할 수 있도록 하는 기능을 갖고 있다.

예제 코드를 통해 살펴보자.
```
window.color = 'red';
var o = {color : 'blue'};

function sayColor() {
	return this.color;
}

sayColor.call(this); 
// 실행결과 : red

sayColor.call(o);
// 실행결과 : blue
```
this를 통해 call을 한 경우, 현재.this는 전역객체인 window이기 때문에 sayColor의 this또한 window를 바라보게 되어 red가 출력된다.

하지만, o를 통해 call을 한 경우, sayColor메소드의 this는 o가 되기 때문에, o의 color인 blue가 출력되는 것이다.

## apply()
call()과 아주 동일하다.
한 가지 차이점은 call은 parameter를 변수를 따로 떼서 전달해야 하지만, apply는 argument를 통째로 전달할 수 있다.

```
function sum(num1, num2) {
	return num1 + num2;
}

// call() 
document.write(sum.call(2, 3));

// apply()
document.write(sum.apply([1, 2]));
```

