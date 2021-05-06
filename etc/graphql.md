# GraphQL
- API를 만들때 사용하는 쿼리 언어
- 쿼리에 대한 데이터를 받을 수 있는 런타임

## 기존 REST api 단점
1. overfetch : 앱에서 필요한 데이터보다 불필요한 데이터를 얻게 되는 경우 
2. underfetch : 필요한 데이터가 더 깊은 depth 등에 존재하여 추가 데이터를 또 요청해야 하는 상황.
3. 유연성 부족 : 클라이언트에 변경사항이 생기면 엔드포인트를 새로 만들어야 하는 경우가 생긴다. 

## Query
- 데이터를 Get할때 사용

### selection set
- 쿼리를 작성할 때 필요한 필드를 중괄호로 감싸는데, 이 중괄호로 묶인 블록을 selection set라고 부름.
- 서로 중첩시킬 수 있음. 

### query arguments
- 쿼리 결과에 대한 필터링 작업을 하고싶을 때 사용
- 쿼리 필드와 관련 있는 key-value 쌍을 하나 이상 인자로 넘길 수 있음.

### fragment
- 쿼리에서 중복되는 부분을 줄일 수 있는 방법
- 특정 타입에 대한 selection set이므로 어떤 타입에 대한 fragment인지 정의에 꼭 써줘야 함. 
```graphql
  fragment liftInfo on Lift { // liftInfo 라고 명명하였으며, Lift 타입에 대한 셀렉션 세트
    name
    status
    capacity
    night
  }
  
  
  // 위에서 만든 liftInfo fragment 사용
  query {
    Lift(id: "jazz-cat") {
      ...liftInfo
      trailAccess {
        name
        difficulty
      }
    }
    
    Trail(id: "river-run") {
      // liftInfo를 Trail 안에 넣는 것은 불가능 : Lift타입에 종속된 fragment이기 때문
      name
      difficulty
      accessedByLifts {
        ...liftInfo
      }
    }
  }
```

### inline fragment
- union type에서 여러 타입의 객체를 반환할 때, 각각의 객체가 어떤 필드를 반환할 것인지 정하는 용도로 사용

### interface
- 필드 하나로 객체 타입을 여러개 반환할 때 사용. 
- 추상적인 타입이며, 유사한 객체 타입을 만들 때 구현해야 하는 필드 리스트를 모아둔 것.

## Mutation
- 데이터를 수정할때 사용
- 애플리케이션의 전반적인 상태를 수정하고자 하는 의도로 사용


## 장점
- 쿼리 한 번에 여러 종류의 데이터를 모두 받을 수 있음. 
