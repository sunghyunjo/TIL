# created vs mounted 비교

vue.js로 프로젝트를 진행하다 보니, created와 mounted가 애매하게 헷갈리는 부분이 생겨 정리하고자 한다.

## created
- 인스턴스가 작성된 후, 동기적으로 호출된다.
- 이 단계에서 데이터 처리, 계산된 속성, 메소드, 감시/이벤트 콜백 등과 같은 옵션 처리를 완료한다. 
- 그렇지만, mount가 시작되지 않았기에 $el 속성을 아직 사용할 수는 없다.

## mounted
- el이 새로 생성된 vm.$el로 대체된 인스턴스가 마운트 된 직후 호출된다.
- 루트 인스턴스가 문서 내의 엘리먼트에 마운트 되어 있으면, mounted가 호출될 때 vm.$el도 문서 안에 있게 된다.
- 컴포넌트, 템플릿, 렌더링된 돔에 접근할 수 있음.

## created와 mounted의 호출 순서
1. Parent created
2. Child created
3. Child mounted
4. Parent mounted

## 정리하자면,
#### created는 데이터 초기화에 대한 목적, mounted는 DOM조작에 대한 목적으로 사용한다.