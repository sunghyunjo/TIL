# React의 장점
react의 대표적인 장점은 크게 세가지로 말할 수 있다.
1. 단수한 앱 개발
2. 빠른 UI
3. 코드량 감소
위의 세가지 장점을 좀 더 자세히 살펴보자.

## 간결하다.
리액트는 세가지 이유로 인하여 간결한 형태라고 말할 수 있다.
- 선언형 스타일 채택
- 순수한 자바스크립트를 이용한 컴포넌트 기반 아키텍처
- 강력한 추상화

과연 이게 무슨 말일까?
  
### 선언형 스타일 채택
React는 명령형 스타일이 아닌, 선언형 스타일을 채택했다. 선언형 스타일이 무엇일까? 명령형 스타일과 비교하여 선언형 스타일을 설명해보자.
- 명령형 스타일 : 개발자가 순서대로 무엇을 해야 할 지 작성하는 방법
- 선언형 스타일 : 실행 결과가 어떻게 되어야 할지를 코드로 작성하는 방법
그런데 왜 선언형 스타일이 나을까?
선언형 스타일은 복잡도를 줄여줄 뿐만 아니라 코드에 대한 이해도와 가독성을 높여주기 때문이다. 
두 가지 방식의 차이를 알 수 있는 간단한 코드를 살펴보자.

```js
// 명령형
var arr = [1, 2, 3, 4, 5];
var arr2 = [];

for (var i = 0; i < arr.length; i++) {
  arr2[i] = arr[i]*2;
}

console.log('a', arr2);
```
```js
// 선언형
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.map(function(v, i) {
  return v*2;
});

console.log('b', arr2);
```

위의 두 예제 모두 arr2에 arr값의 2배를 넣는 것이다.
간단한 예제이기 때문에, 명령형 스타일임에도 잘 작동하지만 코드의 복잡도가 높아진다면 제대로 작동하지 않을 수 있다.

대부분의 개발자들은 명령형에 익숙하기에 아직 선언형 스타일이 크게 와닿지 않을 수 있다.
다른 예제를 한번 더 보자.
  
아래의 두 예제는 각각 명령형과 선언형으로 쓰여진 중첩된 객체 내부의 갑을 가져오는 함수를 사용하는 예이다.
  
```js
var profile = {account: '475'};
var profileDeep = {account: {number: 475}}};
console.log(getNestedValueImperatively(profile, 'account') === '475');
console.log(getNestedValueImperatively(profileDeep, 'account.number' === 475);
```

기본적인 선언은 위와 같이 된 형태에서 해당 함수들을 명령형과 선언형이 어떻게 구현했는지 살펴보자.

```js
// 명령형 스타일
var getNestedValueImperatively = function (object, propertyName) {
  var currentObject = object;
  var propertyNameList = propertyName.split('.');
  var maxNestedevel = propertyNameList.length;
  var currentNestedLevel;
  
  for(currentNestedLevel = 0; curentNestedLevel < maxNestedLevel; currentNestedLevel++) {
    if (!currentObject || typeof currentObject === 'undefined')
      return undefined;
    
    currentObject = currentObject[propertyNameList[currentNestedLevel]];
  }
  
  return currentObject
}
```

```js
// 선언형 스타일
var getValue = function getValue(object, propertyName) {
  return typeof object === 'undefined' ? undefined : object[propertyName]
}

var getNestedValueDeclaratively = function getNestedValueDeclaratively(object, propertyName) {
  return propertyName.split('.').reduce(getValue, object)
}

console.log(getNestedValueDeclaratively({bar: 'baz'}, 'bar') === 'baz');
console.log(getNestedValueDeclarativelyl({bar: {bar: 1}}, 'bar.baz') === 1);
```
선언형 스타일은 위와 같이 명령형 스타일과 달리 결과값에 좀 더 집중한 것을 볼 수 있다.
지역 변수가 줄어들고 논리도 단순해졌다.
  
지금까지 자바스크립트 코드를 살펴본 것처럼, React도 마찬가지로 UI를 구성할 때 선언형 스타일로 작성한다. 
개발자가 UI요소를 선언형 스타일로 작성한 후, 뷰에 변경이 발생하는 경우 React가 알아서 갱신한다.

[출처: 리액트교과서 (아자트마르단 지음/ 곽현철 옮김)]
