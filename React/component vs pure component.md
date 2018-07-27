# React의 Component vs Pure Component
[React Grid Layout](https://github.com/STRML/react-grid-layout)라이브러리를 사용하려고 코드를 분석하던 중 의문점이 생겼다.
class선언 시 Component를 extends하는 것이 아닌, Pure Component를 extends하고 있었다. 
과연 두 Component는 무엇이 다를까?

> #### React.Component와 React.pureComponent는 **shouldComponentUpdate**라이브 사이클 메소드를 다루는 방식을 제외하고는 동일하다.
즉, PureComponent는 shouldComponentUpdate라이프 사이클 메소드가 이미 적용된 버전의 React.Component클래스라고 보면 된다.
  
React.Component를 확장(extends)해서 컴포넌트를 만들 때, shouldComponentUpdate메소드를 별도로 선언하지 않았다면
컴포넌트는 **props, state값이 변경되면 항상 re-render**하도록 되어 있다.
  
그러나, React.PureComponent를 확장해서 컴포넌트를 만들면, shouldComponentUpdate메소드를 선언하지 않아도
PureComponent내부에서 props와 state를 shallow level안에서 비교하여 변경된 값이 있을 시에만 리렌더링하도록 되어 있다.

## 요약하자면,
**shouldComponentUpdate메소드를 선언하지 않았을때,**
- React.Component : props, state값이 변경되면 항상 re-render한다.
- React.PureComponent : props, state가 변경되면, 기존의 값과 비교하여 변경되었을 때만 re-render한다.

[참고1](https://www.vobour.com/%EB%A6%AC%EC%95%A1%ED%8A%B8-react-%EC%9D%B4%ED%95%B4-%EA%B8%B0%EC%B4%88-component-vs-purecomp)
[참고2](https://medium.com/@async3619/when-to-use-component-or-purecomponent-b810897a19a2)
