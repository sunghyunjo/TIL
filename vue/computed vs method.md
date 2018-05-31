# Vue의 computed, method는 어떻게 다를까?
Vue.js를 사용해 프로젝트를 진행하다 보니, computed, method의 기능에 대해 헷갈리는 부분이 생겨 정리하고자 한다.

## computed vs method
[Vue.js의 공식문서](https://kr.vuejs.org/v2/guide/computed.html)에 따르면, computed는 계산된 캐싱이란 말로 설명되어지고 있다.

- 쉽게 설명하자면, computed는 내부의 변수가 변화되는 것을 계속 감지하며, 그 변수가 변화된다면 바로 computed가 실행되어 결과가 업데이트 된다는 것이다.
- 그러나, method는 내부 변수에 의존성을 갖고 있지 않기 때문에, 내부 변수가 변경되어도 결과값이 자동으로 업데이트 되지 않는다.

#### 비교해보자면, method호출은 렌더링을 할때마다 항상 메소드를 호출한다. 하지만, computed는 결과 값을 캐싱하여 변화가 생겼을 때만 함수를 호출하여 결과 값을 업데이트 하는 것이다.
