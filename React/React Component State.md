# React Component State 리액트 컴포넌트의 상태
React의 **상태**란, 컴포넌트의 변경 가능한 데이터 저장소다. 
뷰(`render()`)에서 상태를 이용하고, 이 값을 변경하면 뷰의 표현에 영향을 줄 수 있다.

Vue.js를 해본 사람들이라면, Vue.js에서의 상태와 비슷한 의미로 생각하면 된다.
  
다시 한번 빗대어 설명하자면 이렇다. 컴포넌트를 속성과 상태가 있는 함수라고 생각하면 이 함수의 결과가 UI표현(뷰)인 것이다.
속성과 상태는 둘 다 뷰를 갱신하기 위해 사용할 수 있지만, 서로 다른 목적으로 사용한다.
  
상태 객체에 접근할 때는 이름을 이용한다. 이름은 this.state객체의 속성이다. 
  
상태 데이터는 흔히 뷰의 렌더링이 갱신될 때, 동적 정보를 출력하기 위해 사용된다.
React 개발자는 상태 객체를 이용해서 새로운 UI를 생성한다.
컴포넌트 속성(this.props)이나, 일반적인 변수(inputValue), 클래스 속성(this.inputValue)으로는 처리할 수 없는데, 
이것들을 현재 컴포넌트 내부에서 변경하더라도 뷰를 자동으로 변경할 수 없기 때문이다.
  
예를 들어, 다음 예제는 상태 외의 다른 값을 변경해도 뷰를 갱신할 수 없는 안티패턴이다.
```
let inputValue = 'Texas';
class Autocomplete extends React.Component {
  updateValues() {
    this.props.inputValue = 'California',
    inputValue = 'California',
    this.inputValue = 'California',
  }
  
  render() {
    return (
      <div>
        {this.props.inputValue}
        {inputValue}
        {this.inputValue}
    )
  }
}
```


## 상태 객체 다루기
그럼 어떻게 상태 객체를 다뤄야할까?
시계 컴포넌트를 만든다고 예를 들어보자.

```
class Clock extends React.Component {
  render() {
    return <div>{this.state.currentTime}</div>
  }
}

ReactDOM.render(
  <Clock/>,
  document.getElementById('content');
)
```
이렇게 하면 `Uncaught TypeError: Cannot read property 'currentTime' of null`이라는 오류 메시지가 발생한다.
이는, currentTime값이 없다는 것을 의미한다. 
속성과 달리 상태 객체는 부모 컴포넌트에서 설정하는 것이 아니다. 그렇다고, 상태를 설정하기 위해 `render()`안에서 setState를 실행할 순 없다.
setState -> render -> setState... 로 끊임없이 반복되기 때문에 React가 오류를 발생시킬 것이기 때문이다.

### 초기 상태 설정하기
`render()`에서 상태 데이터를 사용하려면 먼저 상태를 초기화해야 한다.
초기 상태를 설정하려면 React.Component를 사용하는 ES6 클래스의 생성자(constructor)에서 this.state를 선언한다.
반드시 `super()`에 속성을 전달해서 실행해야 한다. 

```
class MyFancyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {...}
  }
  render() {
    ...
  }
}
```
> 부모 클래스가 있는 클래스에서 `constructor()`메서드를 생성하면 그 안에서 거의 항상 `super()`를 호출한다. 그렇지 않으면 부모 클래스의 생성자가 실행되지 않는다. 만약 이렇게 상속으로 클래스를 구현하는 경우, `constructor()`메서드를 따로 작성하지 않으면 `super()`를 호출한 것으로 가정한다.

### 상태 갱신하기
`this.setState(data, callback)`를 사용하면 상태를 변경할 수 있다. 이 메서드를 실행하면 React는 전달하는 data를 현재 상태에 병합하고 `render()`를 호출한다. 이 후, React가 callback함수를 실행한다.
`setState()`가 콜백함수를 사용할 수 있다는 것은, `setState()`가 **비동기**로 작동한다는 뜻이다. 새로운 상태에 의존하는 경우, 콜백함수를 사용해야 새로운 상태가 적용된 후 필요한 작업을 수행할 수 있다. 
이 때 `setState()`가 완료되길 기다리지 않고 새로운 상태에 의존하는 작업을 수행하는 것은 asynchronous작업을 synchronous처럼 다루는 것이다. 이 경우 버그가 발생할 수 있다.

- `setState()`로 전달하는 상태는 상태 객체의 일부분만 갱신한다. 만약 모든 상태를 갱신하고 싶다면, `setState()`에 이 상태에 대한 새로운 값을 명시적으로 전달해야 한다. 


[출처 : 리액트 교과서 / 아자트 마르단 지음, 곽현철 옮김]
