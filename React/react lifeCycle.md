# React의 life cycle
  
  - 마운팅(Mounting)이벤트 : React 엘리먼트(컴포넌트 클래스의 인스턴스)를 DOM노드에 추가할 때, 발생한다.
    - 이벤트를 한 번만 실행한다.
  - 갱신(updating)이벤트 : 속성이나 상태가 변경되어 React엘리먼트를 갱신할 때 발생한다.
    - 이벤트를 여러 번 실행한다.
  - 언마운팅(unmounting)이벤트 : React 엘리먼트를 DOM에서 제거할 때 발생한다.
    - 이벤트를 한 번만 실행한다.
  
  
## 컴포넌트 초기 생성 단계
컴포넌트가 브라우저에 나타나기 전, 후에 호출되는 API

### constructor
```js
constructor(props) {
  super(props);
}
```
- 컴포넌트 생성자 함수
- 컴포넌트가 새로 만들어질 때마다, 이 함수가 호출된다.

### componentWillMount
```js
componentWillMount() {
}
```
- 컴포넌트가 화면에 나가기 직전에 호출되는 API
- React v16.3에서 deprecated되어 이젠 필요하지 않음.
- v16.3이후 부터는 `UNSAFE_componentWillMount()`로 사용된다.
- 이 API에서 하던 일들은 거의 constructor와 componentDidMount에서 충분히 처리할 수 있다.

### componentDidMount
```js
componentDidMount() {
  // 외부 라이브러리 연동 : D3, masonry, etc
  // 컴포넌트에서 필요한 데이터 요청 : Ajax, GraphQL, etc.
  // DOM에 관련된 작업 : 스크롤 설정, 크기 읽어오기 등
}
```
- 컴포넌트가 화면에 나타나게 됐을 때 호출.
- d3, masonry처럼 DOM을 사용해야하는 외부 라이브러리를 연동하거나,
- 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch등을 통해 ajax요청을 하거나,
- DOM의 속성을 읽거나 직접 변경하는 작업을 진행.

  
  
  
## 컴포넌트 업데이트 단계
컴포넌트 업데이트는 props의 변화, state의 변화에 따라 결정된다.
업데이트가 되기 전과 그리고 된 후에 어떠한 API가 호출 되는지 살펴보자.

### componentWillReceiveProps
```js
componentWillReceiveProps(nextProps) {
  // this.props는 아직 바뀌지 않은 상태
}
```
- 새로운 props를 받게됐을때 호출된다.
- 주로 state가 props에 따라 변해야 하는 로직을 이 안에 작성한다.
- 새로 받게될 props는 nextProps로 조화 가능하다.
- this.props를 조회하면 업데이트 되기 전의 API이다.
- 이 또한 v16.3에서 deprecate되고, UNSAFE처리 되었다.
- v16.3이후에서는 getDerivedStateFromProps로 대체될 수 있다.

### static getDerivedStateFromProps() 
v16.3이후에 만들어진 라이프사이클 API
- props로 받아온 값을 state로 동기화하는 작업을 해줘야하는 경우 사용된다.
```js
static getDerivedStateFromProps(nextProps, prevState) {
  // 여기서는 setState를 하는 것ㄱ이 아니라,
  // 특정 props가 바뀔 때 설정하고 설정하고 싶은 state값을 리턴하는 형태로 사용된다.
  /*
  if(nextProps.value !== prevState.value) {
    return {
      value: nextProps.value
    };
  return null; // null을 리턴하면 따로 업데이트할 것은 없다는 의미
  */
  
}
```
  
[출처](https://velopert.com/3631)
