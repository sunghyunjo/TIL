# 객체
## 구문
### 선언적(리터럴)형식 vs 생성자 형식
두 형식 모두 결과적으로 생성되는 객체는 같다.
#### 리터럴 형식
```js
var myObj = {
	key: value
	// ...
};
```

#### 생성자 형식
```js
var myObj = new Object();
myObj.key = value;
```
### 차이점
- 리터럴 형식은 한 번의 선언으로 다수의 키/값 쌍을 프로퍼티로 추가할 수 있다.
- 생성자 형식은 한 번에 한 프로퍼티만 추가할 수 있다.

> 생성자 형식으로 객체를 생성하는 일은 매우 드물다. 거의 리터럴 형식을 사용하며 대부분 내장 객체도 마찬가지다.

## 타입
자바스크립트 객체의 주요 타입

- null
- undefined
- boolean
- number
- string
- object
- symbol(ES6에서 추가)

**단순 원시 타입(string, number, boolean, null, undefined)은 객체가 아니다.**

function은 객체(정확히 말하자면 호출 가능한 객체)의 하위 타입이다. 자바스크립트 함수는 개체이므로 '일급(First class)'이며 여타의 일반 객체와 똑같이 취급한다.

### 내장 객체

- String
- Number
- Boolean
- Object
- Function
- Array
- Date
- RegExp
- Error

내장 객체는 클래스처럼 느껴지지만, 단지 자바스크립트의 내장 함수일 뿐 각각 생성자로 사용되어 주어진 하위 타입의 새 객체를 생성한다.

객체 래퍼 형식이 없는 null과 undefined는 그 자체로 유일한 값이다. 반대로 Date값은 리터럴 형식이 없어서 반드시 생성자 형식으로 생성해야 한다.

Objects, Arrays, Functions, RegExps는 형식과 무관하게 모두 객체다. 

## 내용
### Computed Property Names (계산된 프로퍼티명)
ES6부터 해당 기능이 추가됐는데, 객체 리터럴 선언 구문의 키 이름 부분에 해당 표현식을 넣고 []로 감싸면 된다.

```js
var prefix = "foo";
var myObject = {
	[prefix + "bar"]: "hello",
	[prefix + "bax"]: "world"
};

myObject["foobar"]; // hello
myObject["foobaz"]; // world
```

### 객체 복사
```js
function anotherFunction() { /// }
var anotherObject = {
	c: true
};

var anotherArray = [];

var myObject = {
	a: 2,
	b: anotherObejct, 
	c: anotherArray,
	d: anotherFunction
};

anotherArray.push( anotherObject, myObject );
```

myObject의 사본은 정확히 어떻게 표현해야 할까?
먼저 얕은 복사, 깊은 복사 중 선택해야 한다.

#### 얕은 복사 vs 깊은 복사
- 얕은 복사 후 생성된 새 객체의 a 프로퍼티는 원래 값 2가 그대로 복사된다. 그러나, b, c, d 프로퍼티는 원 객체의 래퍼런스와 같은 대상을 가리키는 또 다른 레퍼런스다.
- 깊은 복사를 하면 myObject는 물론이고 anotherObject와 anotherArray까지 모두 복사한다. 하지만, 여기서 문제는 anotherArray가 anotherObject와 myObject를 가리키는 레퍼런스를 갖고 있으므로, 원래 레퍼런스가 보존되는게 아니라 이들까지 함께 복사된다. 
- 즉, 환영 참조(circular Reference)형태가 되어 무한 복사의 구렁텅이에 빠지고 만다.

## 정리
- 자바스크립트 객체는 리터럴 형식과 생성자 형식 두가지 형태를 가진다.
- 리터럴 형식을 쓴느 편이 좋지만, 생성 시 옵션을 더 주기 위해 생성자 형식을 쓰는 경우도 더러 있다.

- 자바스크립트는 모든 것이 객체는 아니다.
- 객체는 6개의 원시 타입 중 하나고, 함수를 비롯한 하위 타입이 존재한다.
- 객체는 키/값의 쌍을 모아 놓은 저장소이고, 값은 프로퍼티를 통해 접근할 수 있다.

- 프로퍼티에 접근하면 [[Get]]연산을 호출하는데, 객체 자체에 포함된 프로퍼티뿐만 아니라 필요하면 [[Prototype]] 연쇄를 순회하며 찾아본다.
- 프포퍼티는 프로퍼티 서술자를 통해 제어 가능한 writable, configurable 등의 특정한 속성을 지닌다.
- 객체는 Object.preventExtensions(), Object.seal(), Object.freeze() 등을 이용하여 자신에게 여러 단계의 불변성을 적용할 수 있다.

[출처 : You don't know JS]
