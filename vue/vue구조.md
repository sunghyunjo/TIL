<Vue.js 정리>

# 1.	Vue란?
- 데이터바인딩과 화면 단위를 컴포넌트 형태로 제공하며, 관련 API를 지원하는데에 궁극적인 목적이 있음.
- Angular의 2 way binding을 동일하게 제공
- 그러나, component간 통신은 React의 1 Way Data Flow(부모 -> 자식)와 유사
- 다른 FE FW보다 훨씬 가볍고 빠름
- 러닝커브 낮고, 쉬움


## 1) MVVM패턴 사용
- 화면 앞단의 화면 동작 관련 로직과 뒷단의 DB데이터 처리 및 서버 로직을 분리하고, 
뒷단에서 넘어온 데이터를 Model에 담아 View로 넘어주는 중간지점.

> 	View <-> ViewModel <-> Model




# 2. Vue Instance 
	
## 1) Vue Instance 생성자

Ex..
```
var vm = new Vue({
  template: ...,
  el: ...,
  methods: {

  },
  // ...
})
```

- 각  options으로 미리 정의한 vue객체를 확장하여 재사용이 가능함. 
그러나, 이 방법보단 template에서 cumstom element로 작성하는 것이 나음.

Ex.. extends 방법
```
var MyComponent = Vue.extend({
  // template, el, methods와 같은 options 정의
})
// 위에서 정의한 options를 기본으로 하는 컴포넌트 생성
var myComponentInstance = new MyComponent()
```



## 2) Vue Isntance 라이프 사이클 초기화
: Vue객체가 생성될 때, 초기화 작업을 수행한다.
- 데이터 관찰, 템플릿 컴파일, DOM에 객체 연결, 데이터 변경 시 DOM 업데이트
- 이 외에도 개발자가 의도하는 커스텀 로직을 아래와 같이 추가할 수 있음.

Ex.. 
```
var vm = new Vue({
  data: {
    a: 1
  }, 
  created: function(){
    // 여기서 this는 vm을 가리킴.
    console.log('a is: ' + this.a);
  }
})
```
- 이 외에도 라이프 사이클 단계에 따라 mounted, updated, destroyed등을 사용할 수 있다.
이 라이프사이클 초기화 메소드로 커스텀 로직을 수행하기 때문에 Vue에서는 따로 Controlledr를 갖고 있지 않다.



## 3) Vue components

Ex..
```
<div id = "app">
  <my-component></my-component>
</div>

//등록

Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})
//Vue 인스턴스 생성
new Vue({
  el: '#app'
})
```
> !!!!!!!! Vue 인스턴스를 생성하기 전에 꼭 Component부터 등록할 것.





- 컴포넌트의 data속성은 꼭 함수로 작성해야 한다.

Ex..
```
//아래의 Vue컴포넌트는 오류를 발생시킨다.
Vue.component('my-component', {
  data: {
    message: 'hello'
  }
})
```
```
var data = {text: 'hello'}
Vue.component('my-component', {
  data: function() {
    return data;
  }
  // 모든 컴포넌트가 같은 값을 공유하지 않게 아래와 같이 수정한다.

  /*
  data : function() {
    return {
      text: 'hello'
    }
  }
  */

})
```




## 4) Global or Local Component
- 컴포넌트를 뷰 인스턴스에 등록해서 사용할 때 , 다음과 같이 Global하게 등록할 수 있다.

Ex..
```
Vue.component('my-component', {
  //,,,
})
```


- local하게 등록하는 방법
Ex..
```
var cmp = {
  data: function() {
    return {
      // ...
    };
  }
  template: '<hr>',
  methods: {}
}
```
```
new Vue({
  componets: {
    'my-cmp' : cmp
  }
})
```



## 5) 부모와 자식 컴포넌트 관계
: 구조상 상-하 관계에 있는 컴포넌트의 통신은

- 부모 -> 자식 : props down (pass props)
- 자식 -> 부모 : events up (emit events)



### (1) props
: 모든 컴포넌트는 각 컴포넌트 자체의 스코프를 갖는다.
- 예를 들어, 하위 컴포넌트가 상위 컴포넌트의 값을 바로 참조할 수 없는 형식
- 상위에서 하위로 전달하려면 props속성을 사용한다.

Ex..
```
//상위 컴포넌트
<div id = "app">
  // 하위 컴포넌트에 상위 컴포넌트가 갖고 있는 message를 전달함.
  <child-component v-bind:passed-data="message"></child-component>
</div>

//하위 컴포넌트 - 아래 상위 컴포넌트의 data의 message를 passedData에 넘겨받음.
Vue.component('child-component', {
  props: ['passedData'],
  template: '<p> {{ passedData }} </p>'
});

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!',
  }
});
```

> !!!!!! js에서  props 변수 명명을 카멜 기법(aBow)으로 하면 html에서 접근은 케밥 기법(-)으로 해야함.



## 6) 같은 레벨의 컴포넌트 간 통신
동일한 상위 컴포넌트를 가진 2개의 하위 컴포넌트 간의 통신은
> child(하위) -> Parent(상위) -> 다시 2개의 Children(하위) 순으로 이루어짐.

##### 컴포넌트 간의 직접적인 통신은 불가능하도록 되어 있는게 Vue의 구조


###	(1) Event Bus
Non Parent - Child 컴포넌트 간의 통신을 위해 Event Bus를 활용할 수 있음.


- Event Bus를 위해 새로운 Vue를 생성하여 아래와 같이 Vue Root Instance가 위치한 파일에 등록

Ex..
```
// Vue Root Instance 전에 꼭 등록 순서가 중요.
export const eventBus = new Vue();
new Vue({
  //....
})
```



- 이벤트를 발생시킬 컴포넌트에 evnetBus import 후 $emit으로 이벤트 발생

Ex ..
```
import { eventBus } from '../../main';
eventBus.$emit('refresh', 10);
```




- 해당 이벤트를 받을 컴포넌트에도 동일하게 import후 콜백으로 이벤트 수신

Ex ..
```
import { eventBus } from '../../main';

//등록 위치는 해당 컴포넌트의 created 메서드에 등록
created() {
  eventBus.$on('refresh', function (data) {
    console.log(data);
  })
}
```


- 참고 : eventBus의 콜백함수 안에서 해당 소스의 메서드를 참고하려면 self사용

Ex..
```
methods: {
  callAnyMethod() {
    //..
  }
}

created() {
  var self = this;
  eventBus.$on('refresh', function(data){
    console.log(this); // this는 빈 Vue인스턴스를 접근
    self.callAnyMethod() 
    // self는 이 created의 Vue컴포넌트에 접근, 
    // 따라서 이 컴포넌트에 미리 선언된 메서드에 접근 가능
  })
}
```

## ----------- Event Bus 예제

### 1 이벤트 버스 생성
```
var EventBus = new Vue()
```

### 2 이벤트 발행
```
EventBus.$emit('message', 'hello world');
```

### 3 이벤트 구독
```
EventBus.$on('message', function(text) {
  console.log(text);
})
```


## < Vue Routers >
 : Vue를 이용한 SPA 제작에 유용한 라우팅 라이브러리


- Nested Routers
	: 라우터를 이용한 화면을 이동할 때, Nested Routers를 이용하여 여러개의 컴포넌트들 렌더링 할 수 있다.

- Named Views
	: 라우터를 이용하여 특정 URL로 이동할 때, 해당 URL에 해당하는 여러개의 View(컴포넌트)를 동시에 렌더링 한다.



























