# Redux
  
## Redux는 왜 사용할까
- 리덕스란, 앱의 규모가 커졌을 때, 컴포넌트 간의 소통이 많아져 코드가 꼬이는 현상을 방지하기 위해 컴포넌트 간의 통신을 담당하는 중간 역할

## 구성요소
*  Store : 모든 데이터를 저장하고, 이 데이터를 조작할 수 있는 메서드를 제공. 스토어를 생성할 때는 createStore() 메서드를 사용한다.
*  Provider : 모든 컴포넌트가 스토어에서 데이터를 가져올 수 있도록 만들어준다.
*  connect() 메소드 : 컴포넌트를 감싸서 스토어에 있는 애플리케이션 상태의 일부를 컴포넌트의 속성으로 연결한다.

## 동작 방식
### Store의 모든 변경은 Action에 의해 이뤄진다.
- 각 액션은 애플리케이션에 발생한 일과 이에 따라 스토어에서 변경되어야 할 부분을 알려준다.
- 액션은 데이터를 제공하기도 한다.

### Store에서 데이터의 변경 방법은 pure function인 reducer에서 의해 명시된다.
- reducer는 `(state, action) => state`형태를 가지며, 현재 상태에 액션을 적용하여 새로운 상태를 얻는 방식으로 동작한다.
- reducer를 통해 애플리케이션 상태를 예측할 수 있고, 취소 또는 디버깅을 통해 이전 상태로 되돌릴 수 있는 기능도 갖게 된다.
- reducer는 하느 또는 그 이상의 갯수를 둘 수 있다.
- action을 호출할 때마다 모든 리듀서가 호출된다.
