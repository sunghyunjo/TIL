# Destructing Assignment (비구조화 할당)
비구조화 할당 구문은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 자바스크립트 표현식이다.
비구조화 할당 구문을 사용하기 위한 방법 중 하나로 ES6의 [spread operator](https://github.com/sunghyunjo/TIL/blob/master/javascript/ES6%20Spread%20operator.md)를 사용한다. 

## 대표적인 구문
```js
var a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20

// stage 3 proposal
({ a, b, ...rest } = { a: 10, b: 20, c: 30, d: 40 });
console.log(a); // 10
console.log(b); // 20
console.log(rest); // { c: 30, d: 40 }
```

  
    
## 일반적인 구문과 비교
일반적인 배열을 정의하는 형태는 아래와 같다.
```js
var x = [1, 2, 3, 4, 5];
```

비구조화 할당은 이와 비슷하지만, 선언문의 좌변에 어떤 값들을 해체시킬 것인지 정의한다.
```js
var x = [1, 2, 3, 4, 5];
var [y, z] = x;
console.log(y); // 1
console.log(z); // 2
```
즉, y와 z에는 x 배열이 해체되어 index 0과 1을 갖는 (배열의 앞에서부터 2번째까지)수들이 할당되는 것이다.


## 배열 비구조화
위에서 일반적인 배열 구문과 비교를 거쳤기 때문에, 새로운 방식들에 대한 예시만 다뤄볼 것이다.

### 비구조화를 통해 변수선언과 분리하여 값을 할당할 수 있다.
```js
var a, b;
[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

### undefined인 값이 들어오면 기본값을 할당할 수 있다.
```js
var a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```
a에게는 1이 할당되지만, 우변에서 b에게 할당되는 값은 undefined이다.
때문에 b는 기본값으로 선언된 7이 자신의 값으로 할당되는 것이다.

### 하나의 비구조화 표현식으로 두 변수의 값을 교환할 수 있다.
일반적으로, 비구조화 할당 없이 두 값을 교환하기 위해서는 swap하는 알고리즘을 따로 작성해야 한다.
하지만 비구조화 할당을 통한다면 별다른 알고리즘 없이 가능하다.
```js
var a = 1;
var b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
```

### 함수에서 반환된 배열을 비구조화 할당을 통해 간결하게 변수에 할당할 수 있다.
```js
function f() {
  return [1, 2];
}

var a, b;
[a, b] = f();
console.log(a); // 1
console.log(b); // 2
```
위의 예제에서 `f()`함수는 그 출력으로 배열 `[1, 2]`를 반환하는데, 단 한 줄의 비구조화 구문으로 반환된 배열을 파싱할 수 있다.

### 일부 값을 무시할 수 있다.
```js
function f() {
  return [1, 2, 3];
}

var [a, ,b] = f();
console.log(a); // 1
console.log(b); // 3
```
아래와 같이 반환된 값을 모두 무시할 수도 있다.
```js
[,,] = f();
```

### 변수에 배열의 나머지 할당하기
배열을 비구조화할 경우, rest패턴을 이용하여 배열을 해체하고 남은 부분을 하나의 변수에 할당할 수 있다.
```js
var [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```
> 그러나, rest요소 오른쪽 뒤에 콤마가 있으면 `SyntaxError`가 발생한다. ex) `[a, ...b,]`


## 객체 비구조화
객체 비구조화 방법도 위의 배열 비구조화 방법과 거의 동일하다. 
자세한 예시는 [MDN비구조화 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)을 참고하길 바란다.

## ES5버전과 비교
### ES5 버전
```js
function drawES5Chart(options) {
  options = options === undefined ? {} : options;
  var size = options.size === undefined ? 'big' : options.size;
  var cords = options.cords === undefined ? { x: 0, y: 0 } : options.cords;
  var radius = options.radius === undefined ? 25 : options.radius;
  console.log(size, cords, radius);
  // 이제 드디어 차트 그리기 수행
}

drawES5Chart({
  cords: { x: 18, y: 30 },
  radius: 30
});
```

### ES2015 버전
```js
function drawES2015Chart({size = 'big', cords = { x: 0, y: 0 }, radius = 25} = {}) {
  console.log(size, cords, radius);
  // 차트 그리기 수행
}

drawES2015Chart({
  cords: { x: 18, y: 30 },
  radius: 30
});
```
위의 ES2015버전에서는 `{size = 'big', cords = {x: 0, y: 0}, radius = 25} = {}`과 같이 비구조화된 좌변에 빈 오브젝트 리터럴을 할당하는 것을 볼 수 있다.
빈 오브젝트를 할당하지 않고 함수를 작성할 수 있다. 

하지만, 지금의 형태에서는 단순히 `drawES2015Chart()`와 같이 어떤 매개변수 없이도 호출할 수 있지만,
우변의 빈 오브젝트 할당을 없앤다면 함수 호출시 적어도 하나의 인자가 제공되어야만 한다.

**즉, 이 함수가 어떠한 매개변수 없이도 호출할 수 있길 원하면 지금처럼 우변에 빈 오브젝트를 할당하는 것이 유용하고, 무조건 객체를 넘기길 원한다면 빈 오브젝트 할당을 하지 않는 것이 유용하다.**


## 더 많은 정보는..
이 글은 [MDN문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)를 참고하였다.
더 많은 예제와 브라우저 호환성은 위의 문서를 참고하길 바란다.


