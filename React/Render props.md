# React Render Props Pattern
공식 문서에 나온 대로 번역을 해본다면,
> 'Render props' 용어는 React component들 간에 코드를 공유하기 위해 함수형 props를 이용한 간단한 테크닉입니다.
라고 나와 있다.
즉, 다시 쉽게 말하자면 **Render Props패턴은 함수로 이루어진 props를 이용하여 컴포넌트 간 코드를 공유하는 방법**이다.
  
다음은 render prop 사용법에 대한 간단한 예제이다.
```
<DataProvider render = {data => (
  <h1>Hello {data.target}</h1>
)}/>
```
render props pattern으로 구현된 component는 자체적으로 rendering로직을 구현하는 대신, 
render element요소를 반환하고 이를 호출하는 함수를 사용합니다.

## Render Props는 언제 사용할 수 있을까? 그리고 그 사용법은?
Component는 React에서 코드의 재사용성을 위한 주요 단위이다. 하지만 한 컴포넌트의 상태나 동작을 다른 컴포넌트와 공유하는 방법이 쉽지는 않았다.
   
예를 들면, 아래 component는 웹 어플리케이션에서 마우스 위치를 추적하는 로직을 가지고 있다.
```
class MouseTracker extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>
        <h1>Move the mouse around!</h1>
        <p>The current mouse position is ({this.state.x}, {this.state.y})</p>
      </div>
    );
  }
}
```
스크린 주위로 마우스 커서를 움직이면, component가 마우스(x, y)좌표를 <p>에 나타내는 코드이다.
  
#### 그런데, 여기서 다른 component가 이 행위를 다시 사용하려면 어떻게 해야할까? 즉, 다른 component에서 커서 위치를 알아야 할 경우, 해당 행위를 공유하기 쉽게 캡슐화 할 수 있는가?
  
위에서 동작하는 마우스 커서 위치를 받아오는 기능을 <Mouse>컴포넌트로 캡슐화하여 어디서든 사용할 수 있도록 리팩토링을 해보면 아래와 같다.
  
```
// The <Mouse> component encapsulates the behavior we need...
class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/* ...but how do we render something other than a <p>? */}
        <p>The current mouse position is ({this.state.x}, {this.state.y})</p>
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse />
      </div>
    );
  }
}
```

이제 <Mouse> 컴포넌트는 mouse event를 감지하고 마우스 커서의 위치를 저장하는 동작을 캡슐화했다.
그러나 아직 완벽히 재사용할 수 있는 형태는 아니다.
  
예를 들어, 마우스 주위에 고양이 그림을 보여주는 <Cat>컴포넌트가 있다고 생각해보자.
우리는 <Cat mouse={{x, y}}> prop을 통해 Cat 컴포넌트에게 마우스 좌표를 전달해주고, 화면의 한 위치에 이미지를 보여줄지 알려주고자 한다.
  
### 첫번째 방법
- <Mouse> component의 render method안에 <Cat> component를 넣어 랜더링한다.
```
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class MouseWithCat extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/*
          We could just swap out the <p> for a <Cat> here ... but then
          we would need to create a separate <MouseWithSomethingElse>
          component every time we need to use it, so <MouseWithCat>
          isn't really reusable yet.
        */}
        <Cat mouse={this.state} />
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <MouseWithCat />
      </div>
    );
  }
}
```

이런 방법은 특정 상황에서는 적용할 수 있지만, 마우스 트랙킹만 캡슐화하지는 못했다.
쉽게 말하자면, <Dog> component에서 마우스 좌표를 받고자 한다면 또다시 <MouseWithDog>라는 컴포넌트를 만들어야 할 것이다.
  

### 두번째 방법
- **render prop**을 사용하는 방법 
  - <Mouse> component 안에 <Cat> component를 hard-coding해서 결과물을 바꾸는 것이 아닌, <Mouse>에게 동적으로 rendering을 할 수 있도록 해주는 함수형 prop을 제공 할 수 있다.
  **=> 이것이 render props의 개념이다!**
  
```
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```
  
위의 코드 중, MouseTracker코드를 자세히 보면 <Mouse> component의 행위를 복제하기 위해 하드코딩할 필요 없이, render함수에 prop으로 전달해주고 있다. 이로써 <Mouse> component는 동적으로 트래킹 기능을 가진 component들을 랜더링 할 수 있다.
  
### 즉, render prop은 무엇을 render할지 component에게 알려주는 함수이다.


[참고자료](https://medium.com/@dev_momo/react-render-props-pattern-1c53a6b9645c)
