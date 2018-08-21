# ES6문법의 Spread 연산자
ES6문법의 코드들을 보던 중, `...`으로 표현한 문법에 대해 알아보았다.
`...`은 [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_operator) 에서 **spread operator**라고 표현한다.
MDN정의에 따르면 전개 연산자(spread operator)는 표현식(expression)은 2개 이상의 인수(arguments: 함수 호출용)나 2개 이상의 요소(elements: 배열 리터럴용) 또는 2개 이상의 변수(비구조화 할당용)가 예상되는 곳에 확장될 수 있도록 한다고 쓰여있다.

#### 정리하자면,
- Iterable Object(열거 가능한 오브젝트)를 하나씩 전개한다.
- 표현 방식 `...iterable`
- 변수 앞에 `...`을 찍어서 선언한다.
  
    
## Example
### Array Spread
```js
let one = [1, 2];
let two = [3, 4];
let spread = [0, ...one, 5, ...two];

console.log(spread);

/*
출력결과
[0, 1, 2, 5, 3, 4]
*/
```

### String Srpead
```js
let str = 'hello';
let spread = [...str];

console.log(spread);

/*
출력결과 
['h', 'e', 'l', 'l', 'o']
*/
```
  
### Object Spread
```js
let one = [{name : 1}, {name : 2}];
let two = [{name : 3, ...one}];

console.log(one);
console.log(two);

/*
출력결과
[{name : 1}, {name : 2}]
[{name : 3}, {name : 1}, {name : 2}]
*/
```
  
### function Spread
```js
const value = [10, 20, 30];
function abc (a, b, c) {
  console.log(a, b, c);
};
abc(...value);

/*
출력결과 
10 20 30
*/
```
  
### Rest parameter
- 하나 이상의 파라미터를 묶는다.
- 넘겨 받은 배열 [1, 2, 3, 4, 5] 중 [3, 4, 5]는 배열이 된다.

```js
function test (a, b, ...rest) {
  console.log(a, b, rest);
}
test(...[1, 2, 3, 4, 5]);

/*
출력결과
1 2 [3, 4, 5]
*/
```

[참고](http://mollangk.tistory.com/29)
[MDN공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_operator)
