# Flux 데이터 아키텍처

Flux는 Facebook에서 클라이언트-사이드 웹 어플리케이션을 만들기 위해 사용하는 어플리케이션 아키텍처이다.
Flux의 요지는, **단방향 데이터 흐름을 적용하고 MVC계열 패턴의 복잡도를 제거하는 것**이다.

## 일반적인 MVC계열의 패턴
1. 사용자 조작으로 컨트롤러에 이벤트가 발생하면 컨트롤러가 모델을 처리한다.
2. 모델에 따라 앱이 뷰를 렌더링하고 광기가 시작된다.
3. 개별 뷰는 모델을 갱신한다. 이 때, 자체적인 모델뿐만 아니라 다른 모델도 갱신한다.
4. 모델은 다시 뷰를 갱신한다. (양방향 데이터 흐름이다.)

![복잡한 MVC구조](http://gnujoow.github.io/images/dev/5/1.png)

하지만, 위와 같은 아키텍처 안에서는 방향을 잃기 쉽다. 이해하기 어렵고 디버깅도 힘든 아키텍처이다.
MVC계열의 아키텍처에서 여러 개의 뷰가 여러 개의 모델에 변경을 발생시키거나 또는 반대 방향으로 변경을 발생시켜 복잡도가 높아지게 되는 예이다.


하지만 이와 다르게 Flux는 단방향 데이터 흐름의 사용을 제안한다.

## Flux 구조
Flux 어플리케이션은 다음 핵심적인 세가지 부분으로 구성되어 있다.
##### Dispatcher, Stores, Views(React 컴포넌트)

Flux는 MVC와 다르게 **단방향**으로 데이터가 흐른다.

1. React view에서 사용자가 상호작용을 한다.
2. 이 때, view는 중앙의 dispatcher를 통해 action을 전파하게 된다.
3. 어플리케이션의 데이터와 비즈니스 로직을 갖고 있는 store는 action이 전파되면, 이 action에 영향이 있는 모든 view를 갱신한다.

![flux의 단방향 구조](https://haruair.github.io/flux/img/flux-simple-f8-diagram-1300w.png)

Store는 데이터와 view의 표현을 책임진다. view는 데이터를 변경하지 않고, dispatcher를 이용해서 action을 전달한다.

![좀 더 복잡한 flux아키텍처](https://66.media.tumblr.com/eeab519ae5a83261728699f2c684b730/tumblr_inline_nrb9kb1Rxc1skvdct_540.png)

- 참고: 리액트 교과서 / 아자르 마르단 지음, 곽현철 옮김
- [참고](https://haruair.github.io/flux/docs/overview.html)
