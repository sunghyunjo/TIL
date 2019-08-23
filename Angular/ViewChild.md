# ViewChild
부모 컴포넌트에서 자식 컴포넌트의 객체 뿐만 아니라 자식으로 포함된 directive에 직접 접근이 가능하며, 컴포넌트가 렌더링하는 view 자체에 직접 접근할 수 있다. 

- ngAfterViewInit 이 부르기 전에 셋팅 된다.
- @ViewChild를 이용하면 조건에 부합되는 객체를 모두 찾게 되고, QueryList 형태로 객체들의 집합을 얻을 수 있다. QueryList는 실제 배열이 아니기 때문에 toArray() 메소드를 이용해 배열을 얻어내 이용할 수 있다.

### Metadata

- **selector**
- **read :** true이면 다른 토큰을 읽을 수 있다.
- **static** : true이면, 변화를 감지하기 전에 쿼리 결과를 해결할 수 있다.
