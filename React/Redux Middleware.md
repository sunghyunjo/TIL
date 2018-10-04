# Redux Middleware
![미들웨어 구조](https://redux-advanced.vlpt.us/images/redux-middleware.png)

- 액션이 디스패치(dispatch)되어서 리듀서에서 이를 처리하기 전에 사전에 지정된 작업들을 설정하는 역할을 한다.
- 미들웨어를 액션과 리듀서 사이의 중간자라고 이해하면 편하다.

## Middleware Example
미들웨어를 만들며, 구조를 파악해보자.

```js
const loggerMiddleware = store => next => action => {
	/ * 미들웨어 내용 */
}
```

위에서 `next`는 `store.dispatch`와 비슷한 역할을 한다.

#### next 와 store.dispatch 차이점

![](https://redux-advanced.vlpt.us/images/next-vs-dispatch.png)

#### `next(action)`
- 바로 리듀서로 넘기거나, 혹은 미들웨어가 더 있다면 다음 미들웨어가 처리 되도록 진행된다. 

#### `store.dispatch`
- 처음부터 다시 액션이 디스패치되는 것이기 때문에 현재 미들웨어를 다시한번 처리하게 된다. 


## 비동기 작업을 위한 미들웨어
### redux-thunk
- 객체 대신 함수를 생성하는 액션 생성함수를 작성 할 수 있게 해준다.

리덕스에서는 기본적으로 애션 객체를 디스패치한다. 일반 액션 생성자는, 다음과 같이 파라미터를 가지고 액션 객체를 생성하는 작업만 한다.

```js
const actionCreator = (payload) => ({ action: 'ACTION', payload });
```

만약에 특정 액션이 몇초뒤에 실행되게 하거나, 현재 상태에 따라 아예 액션이 무시되게 하려면, 일반 액션 생성자로는 할 수 없다. 그러나, redux-thunk를 이용하면 가능하다.

우선, 1초 뒤 액션이 디스패치되게 하는 예제코드를 살펴보자.

```js
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
	return {
		type: INCREMENT_COUNTER
	};
}

function incrementAsync() {
	return dispatch => { // dispatch를 파라미터로 가지는 함수를 리턴한다.
		setTimeout(() => {
			// 1초 뒤 dispatch한다.
			dispatch(increment());
		}, 1000);
	};
}	
```

이렇게 코드를 작성하면, 나중에 `store.dispatch(incrementAsync());`를 하면 `INCREMENT_COUNTER`액션이 1초뒤에 디스패치된다.

이번엔 조건에 따라 액션을 디스패치하거나 무시하는 코드를 살펴보자.

```js
function incrementIfOdd() {
	return (dispatch, getState) => {
		const { counter } = getState();
		
		if (counter % 2 === 0) {
			return;
		}
		
		dispatch(increment());
	};
}	
```

만약, 리턴함수에서 `dispatch, getState`를 파라미터로 받게 한다면 스토어의 상태에도 접근할 수 있다.
따라서, 현재의 스토어 상태 값에 따라 액션이 dispatch될 지 무시될지 정해줄 수 있는 것이다.

### 간단하게 정리하자면,
redux-thunk는 일반 액션 생성자에 날개를 달아준다. 보통의 액션 생성자는 그냥 하나의 액션 객체를 생성할 뿐이지만, redux-thunk를 통해 만든 액션생성자는 그 내부에서 여러가지 작업을 할 수 있다. 네트워크 요청을 해도 무방하고, 액션을 여러번 디스패치 할 수도 있다.

[출처](https://redux-advanced.vlpt.us/2/01.html)
