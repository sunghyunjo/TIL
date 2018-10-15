# HOC (Higher Order Component)
- 코드 재사용을 위한 리액트의 표준 방식

### 컴포넌트 = (순수)함수
- 리액트의 컴포넌트는 `props`를 입력 받고, `ReactElement`트리를 반환하는 순수 함수이다.

## Higher Order Function
HOC와 가장 유사한 개념인 HOF를 먼저 알아보자.

- HOF는 함수를 인자로 받아서 새로운 함수를 반환하는 함수이다. 

```js
const fy = HOF(fx);
```

즉, fx를 인자로 받아서 fy를 반환하는 함수라고 볼 수 있다. 우리가 자주 사용하는 Lodash도 모두 HOF이다.
Lodash의 `_.partial`함수는 다음과 같이 기존 함수의 인자를 고정시킨 새로운 함수를 반환해준다.

```js
const add = (v1, v2) => v1 + v2;

const add3 = _.partial(add, 3);
const add5 = _.partial(add, 5);

console.log(add3(10)); // 13
console.log(add5(10)); // 15
```

#### HOF의 장점 
- 함수에 기능을 추가하는 코드를 재사용할 수 있다. 

만일, `add`함수를 이용해서 `partial`함수 없이 `add3`와 `add5`함수를 만드려면, 다음과 가이 직접 두 개의 함수를 만들어야 할 것이다.

```js
const add3 = v => add(v + 3);
const add5 = v => add(v + 5);
```

그러나, HOF를 이용하면 기능 단위로 새로운 함수를 만들어내는 코드를 재사용할 수 있게 된다.
좀 더 복잡한 예제를 살펴보자.

`_.partial`함수를 직접 구현해 보도록 하자. 

```js
const partial = (f, v1) => v2 => f(v1, v2);

const add3 = partial(add, 3);
const add5 = partial(add, 5);

console.log(add3(10)); // 13
console.log(add5(10)); // 15
```

좀 더 유용한 HOF를 만들어보자. 함수의 클로저를 이용하면 내부 상태를 갖는 함수를 만들 수 있다. 간단한 예로 함수의 반환값을 0부터 계속 누적시켜서 적용된 값을 반환하도록 하는 HOF를 만들어보자. 

```js
function add(f) {
	let v = 0; 
	return function() {
		v = f(v);
		return v;
	}
}

const acc3 = acc(add3);

console.log(acc3()); // 3
console.log(acc3()); // 6
console.log(acc3()); // 9
```

HOF를 이용하면 부수효과를 만들어낼 수도 있다. 간단한 예제로, 결과값을 반환하기 전에 콘솔에 로그를 출력하도록 하는 HOF를 만들어 보자.

```js
function logger(f) {
	return function(v) {
		const result = f(v);
		console.log(result);
		return result;
	}
}

const add3Log = logger(add3);

console.log(add3Log(10)); // 13, 13
console.log(add3Log(15)); // 18, 18
```

지금까지 3가지의 HOF를 만들어보았다. 이제 이들을 조합해서 `partial`, `acc`, `logger`를 모두 사용하면 다음과 같이 0부터 시작해서 3씩 더한 결과값을 누적시키면서 로그를 남기는 함수를 만들 수 있을 것이다.

```js
const acc3Log = logger(acc(partial(add, 3)));

acc3Log(); // 3
acc3Log(); // 6
acc3Log(); // 9
```

함수형 프로그래밍은 기본적으로 작은 단위의 범용적인 함수를 만들고 이들을 조합해 가면서 프로그램을 만들어나가는 방식을 취한다. HOF는 이러한 기능 단위의 함수들을 조합해서 재사용할 수 있는 패턴을 제공함으로써 아주 중요한 역할을 하고 있다.

## Higher Order Component
- 컴포넌트를 인자로 받아서 컴포넌트를 반환하는 함수를 뜻한다.

```js
const compX = HOC(compX);
```

그렇다면, 이 HOC를 `partial`함수처럼, Props를 고정한 형태의 컴포넌트를 반환하게 적용해보자.

```js
function withProps(comp, props) {
	return function(ownProps) {
		return <Comp {...props} {...ownProps} />
	}
}
```

#### `partial`의 예제와 다른 점
- 컴포넌트는 열거된 인자가 아닌, props라는 객체를 입력값으로 받는다.
	
	> 즉, 특정 props를 고정하고 싶다면 해당 props를 객체로 받은 다음, 반환되는 컴포넌트의 props에 합쳐주는 식으로 구현해야 한다.

- 컴포넌트는 단순히 값을 반환하는 것이 아닌, `ReactElement`를 반환한다.

	> 그러므로, 반환되는 컴포넌트는 인자로 넘어온 컴포넌트를 `ReactElement`화 시켜서(JSX이용) 반환해 주어야 한다.
	

이제, 이 `withProps` HOC를 사용하는 예제를 살펴보자.

```js
function Hello(props) {
	return <div>Hello, {props.name}. I am {props.myName}</div>
}

const HelloJohn = withProps(Hello, {name: 'John'});
const HelloMary = withProps(Hello, {name: 'Mary'});

const App = () => (
	<div>
		<HelloJohn myname = "Kim" />
		<HelloMary myname = "Lee" />
	</div>
)	
```	
`Hello`컴포넌트는 props로 `name`과 `myName`을 입력받는다. `withProps`를 이용해 `name`을 고정한 형태의 컴포넌트인 Hellojohn과 HelloMary컴포넌트를 만들어내면, 이들 컴포넌트는 `myName`만 넘겨주어도 미리 고정된 `name`값을 이용할 수 있게 된다.

위의 `logger`도 HOC에 맞게 다시 구현해보자. `withProps`를 만들때와 마찬가지로 props와 반환값에만 유의하면 된다. logger는 굳이 props를 받을 필요가 없어 좀 더 간단하게 구현할 수 잇다.

```js
function logger(Comp) {
	return function(props) {
		console.log(props);
		return <Comp {...props} />
	}
}	
```
HOF와 마찬가지로 HOC도 여러개의 HOC를 동시에 조합해서 하나의 컴포넌트를 만들어낼 수 있다. 위의 두 HOC를 조합해서 사용해보자.

```js
const HelloJohn = withProps(logger(Hello), {name: 'John'});
const HelloMary = withProps(logger(Hello), {name: 'Mary'});
```
이제 `HelloJohn`와 `HelloMary`는 렌더링을 할때마다 현재 넘겨받은 props를 콘솔에 출력하게 된다. 위의 App 컴포넌트를 렌더링 해보면 각 컴포넌트의 `name`과 `myName` props가 모두 콘솔에 출력되는 것을 확인할 수 있을 것이다.

## HOC응용
HOC를 가장 많이 쓰는 형태는 아마 스토어와 컴포넌트를 연결시켜주는 부분일 것이다. react-redux의 `connect`함수도 이런 역할을 하는데, 일종에 HOC를 생성해주는 헬퍼 함수라고 할 수 있다. 
`connect`함수는 스토어의 상태를 props로 주입시켜주는 `mapStateToProps`와 액션 생성 함수를 스토어의 dispatch와 연결시켜 props로 주입시켜주는 `mapDispatchToProps`를 인자로 받아서 새로운 HOC를 반환한다.

```js
import {connect} from 'react-redux';
import {PersonComponent} from './person';

const mapStateToProps = state => ({
	name: state.name,
	age: state.age
});

const mapDispatchToProps = {
	setName: (name) => { type: 'SET_NAME', name },
	setAge: (age) => { type: 'SET_AGE', age }
};

// 	HOC생성
const connectHOC = connet(mapStateToProps, mapDispatchToProps);

// HOC가 적용된 컴포넌트 생성
const ConnetedComponent = connectHOC(PersonComponent);
```

##### 이 외에도 HOC로 할 수 있는 기능들
- 생명주기 메소드 주입
- State 및 이벤트 핸들러 주입
- Props 변환 및 주입
- Render 함수 확장


[참고](https://meetup.toast.com/posts/137)
