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
