# Hooks
React v16.8에 정식 탑재되었다.
> class없이 state를 사용할 수 있는 새로운 기능.

## 기존 방식의 문제점
- 로직이 UI 및 리액트 life cycle에 너무 밀접하게 결합되어 있었다.

#### 그래서 도입된 HOC(Highter Order Component)
HOC : 화면에서 재사용 가능한 로직만을 분리해서 component로 만들고, 재사용 불가능한 UI와 같은 다른 부분들은 parameter로 받아서 처리하는 방법

#### 그러나, HOC로 인해 생긴 wrapper의 지옥
여러 로직이 `componentWillUnmount`, `componentDidMount`등의 리액트 life cycle에 흩어지게 된다. 이로 인해 함수는 단일 책임 원칙을 벗어나고, 코드는 점점 복잡해진다.

#### 이런 문제를 해결하기 위해 생겨난 React Hook의 등장
로직을 분리하면서도 wrapper hells를 피할 수 있고, React Component life cycle에도 종속적이지 않게 코딩하는 방법의 일환으로 도입되었다.

## State Hook: useState
State Hook은 함수형 컴포넌트에서 변화 할 수 있는 상태를 사용 할 수 있게 해준다.

- `useState`함수의 파라미터로는 사용하고 싶은 상태의 기본값을 넣어준다. 
- `useState`를 호출하면 배열을 반환하는데 이 배열의 첫번째 원소는 현재 상태 값과, 두번째 원소는 이 값을 설정해주는 setter함수이다.

## Effect Hook: useEffect
`useEffect`함수는 컴포넌트가 마운트되거나 리렌더링을 마치고 나서 실행된다. `componentDidMount`와 `componentDidUpdate`와 비슷하다고 생각하면 된다.

- `useEffect`에 넣은 함수는 컴포넌트가 render를 마친 다음에 실행된다.
-  props나 state가 바뀌지 않고 부모 컴포넌트가 리렌더링 될 때에도 호출이 된다. 만약, 특정 상황에만 이 함수가 실행되게끔 하고 싶다면 `useEffect`의 두번째 파라미터로 주시하고 싶은 값들을 배열 형태로 전달해주면 된다.

```js
useEffect(() => {
	// 이 함수는 render가 마치고 난 다음에 실행된다.
	console.log('rendered:', value);
}, [value]);
```

위의 코드처럼 하면 value값이 바뀔 때만 useEffect가 호출된다.

## Hooks 사용 규칙
#### 1. Hooks를 컴포넌트의 Top-level에서만 사용 할 것
- Hooks는 반복문이나 조건문, 감싸진 함수에서 사용하면 안된다.

#### 2. 리액트 함수에서만 사용할 것
- Hooks는 리액트 함수형 컴포넌트 내부에서만 사용해야 한다. (Custom Hook 제외) 일반 JavaScript 함수에서는 사용하면 안된다. 

## 내장 Hooks
#### useContext
```js
const context = useContext(Context);
```

`useContext`는 Context API를 Hook을 통해 사용 할 수 있게 해준다.

#### useReducer
`useReducer`는 리덕스에서 리듀서를 사용하는 것과 유사한 방식으로 컴포넌트 상태 관리를 할 수 있게 해준다. 해당 Hook을 사용하기 위해 사전에 리덕스를 설치해야 하는 것은 아니다. 단지 컴포넌트 내부에서 사용 할 수 있는 리듀서라고 생각하면 된다.

#### useRef
`useRef`는 함수형 컴포넌트에서도 ref를 사용 할 수 있게 해주는 Hook이다.

[참고문서1](https://velog.io/@velopert/react-hooks)
[참고문서2](https://velog.io/@vies00/React-Hooks)
