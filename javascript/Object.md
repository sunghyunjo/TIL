# Object
- JavaScript에서 모든 객체는 Object의 후손이다.
- 즉, 모든 객체는 Object.prototype으로 부터 메소드 및 프로퍼티를 상속한다.

## Method
### ES5
- `create(proto[, propertiesObject])` : 지정된 프로토타입 객체와 속성을 갖는 새로운 객체를 만든다.
- `defineProperties(obj, props)` : 이미 존재하거나 새로운 프로퍼티들의 각종 속성들을 재정의할 수 있다.
- `defineProperty(obj, props, descriptor)` : 객체에 직접 새로운 속성을 정의하거나 이미 존재하는 객체를 수정한 뒤 그 객체를 반환한다.
- `freeze()` : 객체를 얼린다. 즉, 객체에 새로운 속성을 추가하거나 기존의 속성을 변경할 수 없는 등 객체를 불변(immutable)객체로 만들어준다.
- `getOwnPropertyDescriptor(obj, prop)` : 객체에 해당 prop이 존재하면 prop의 descriptor가 반환되며, 없으면 undefined가 반환된다.
- `getPrototypeOf(obj)` : 지정된 객체의 프로토타입을 반환한다.
- `isExtensible()` : 객체에 새 속성을 추가할 수 있는지 여부를 알려준다.
- `isFrozen(obj)` : 객체가 동결됐는지 여부를 Boolean값으로 반환한다.
- `isSealed(obj)` : 객체가 봉인됐는지를 결정한다. 
	
	> [isFrozen](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)과 [isSealed](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)에 대한 자세한 설명을 참고할 것. 

- `keys()` : 객체에서 enumerable한 속성들 중 key값들을 array형태로 반환한다.
- `seal(obj)` : 객체를 밀봉한다. 새로운 속성을 추가할 수 없고, 현재 존재하는 모든 속성을 설정 불가능 상태로 만들어준다. 하지만, 쓰기 가능한 속성의 값은 밀봉 후에도 변경할 수 있다. **(이 점이 `freeze()`와의 차이점이다.)**
- `preventExtensions(obj)` : 새로운 속성이 객체에 추가되는 것을 방지한다.
- `getOwnPropertyNames(obj)` : 해당 객체의 프로퍼티들을 string 배열로 반환한다.
- `prototype.hasOwnProperty(prop)` : 객체가 특정 프로퍼티를 가지고 있는지를 나타내는 Boolean값을 반환한다.
- `prototype.isPrototypeOf(obj)` : 해당 객체가 다른 객체의 프로토타입 체인에 속한 객체인지 확인하기 위해 사용한다.
- `prototype.propertyIsEnumerable(prop)` : 특정 속성이 enumerable한 속성인 지 여부를 Boolean값으로 반환한다.
- `prootype.toLocaleString()` : `toString()`을 호출 한 결과를 반환한다. 모든 객체가 사용할 수 있는 함수는 아니며, 시간이나 날짜 등을 문자열로 변환할 때 사용되어 진다.
- `prototype.toString()` : 객체를 문자열로 반환한다.
- `prototype.valueOf()` : 특정 객체의 원시값을 반환한다.

### ES6
- `assign(target, ...sources)` : sources에 해당하는 오브젝트들을 target오브젝트로 복사한다.
- `is(value1, value2)` : 두 값이 같은 값인지 여부를 Boolean값으로 반환한다.
- `setPrototypeOf(obj, prototype)` : 지정된 객체의 프로토타입을 다른 객체 또는 null로 설정한다.
- `getOwnPropertySymbols(obj)` : 해당 객체에 존재하는 프로퍼티를 symbol 타입 배열로 반환한다.

### ES8
- `entries()` : `for...in`과 같은 순서로 주어진 객체 자체의 열거된 속성 [key, value]쌍의 배열을 반환한다.
- `values(obj)` : 해당 객체의 value들로 구성된 배열을 반환한다.
- `getOwnPropertyDescriptors(obj)` : 지정된 객체의 모든 property descriptor를 반환한다. 빈 객체라면, 아무런 프로퍼티가 존재하지 않는다. 


[참고 : MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
