# Vue.js의 라이프사이클
vue.js의 라이프 사이클은 creation, mounting, updating, destruction으로 나눌 수 있다.
![Vue.js의 라이프사이클](https://kr.vuejs.org/images/lifecycle.png)

## 1. Creation : 컴포넌트 초기화 단계
- Creation단계에서 실행되는 hook들이 라이프사이클 중에서도 가장 처음 실행.
- 컴포넌트가 DOM에 추가되기 전.
- 서버렌더링에서도 지원되는 훅.
  * 때문에, Client와 Server단 모두에서 처리할 일은 Creation 단계에서 실행하면 된다. 
  * 그러나, 아직 component가 DOM에 추가되기 전이기 때문에 DOM에 접근하거나 this.$el을 사용할 수 없음.

### 1) Before Create
모든 훅 중 가장 먼저 실행되는 훅.
아직 data와 events(on, once, off, emit)가 셋팅되기 전의 시점이라서 접근 불가하다.

즉,  data()와 이벤트에 접근할 수 없다.

### 2) Created
data와 events가 활성화되어 접근할 수 있다. 여전히 템플릿과 가상DOM은 마운트 및 렌더링되지 않은 상태.
```
<script>
  export default {
    data () {
      return {
        title: ''
      }
    },
    computed: {
      titleComputed() {
        console.log('I change when this.property changes.')
        return this.property
      }
    },
    created () {
      //can use Data(this.title, this.titleComputed ...), events(vm.$on, vm.$once, vm.$off, vm.$emit)
      //don't use $el
    }
  }
</script>
```

## 2. Mounting : DOM삽입 단계
Mounting단계에서는 초기 렌더링 직전에 컴포넌트에 직접 접근 가능하다. 
그러나, 서버렌더링에서는 지원하지 않는다.
- 초기 렌더링 직전에 DOM을 변경하고자 하면, 해당 단계를 활용할 수 있다. 그러나, 컴포넌트 초기에 셋팅되어야 하는 data는 created단계를 사용하는 편이 낫다.

### 1) before Mount

템플릿과 render함수들이 컴파일된 후, 첫 렌더링이 일어나기 직전에 실행됨.
가급적 사용하지 않는 편이 낫고, 서버사이드 렌더링 시에는 호출되지 않음.

### 2) mounted
컴포넌트, 템플릿, 렌더링된 DOM에 접근할 수 있음.
> 그러나 모든 하위 컴포넌트가 mount된 상태를 보장하진 않음.
> vm.$nextTick을 사용하면 전체가 렌더링된 상태를 보장할 수 있으며, 서버렌더링에서는 호출되지 않음.

```
<script>
export default {
  mounted() {
    console.log(this.$el.textContent) // can use $el
    this.$nextTick(function () {
      // 모든 화면이 렌더링된 후 실행합니다.
    })
  }
}
</script>
```
#### 여기서 유의할 점! 부모와 자식 관계의 컴포넌트에서 우리가 생각한 순서대로 mounted가 발생하지 않는다.  즉, 부모의 mounted훅이 자식의 mounted훅보다 먼저 실행되지 않는다. 오히려 반대..

![Parent/child initialisation workflow](https://cdn-images-1.medium.com/max/1600/1*7nxq9WPZrQA_0tj0ultWqw.png)
그림을 참고하자면, Created훅은 부모 자식 순서대로 실행되지만, mounted는 아니라는 걸 알 수 있다. 부모는 mounted훅을 실행하기 전에 자식의 mounted훅이 끝나기를 기다린다.

## 3. Updating 
컴포넌트의 속성들이 변경되거나 재 렌더링이 발생할 때 실행되는 라이프사이클.
컴포넌트의 재 렌더링 시점을 알고 싶을 때 사용하면 된다. 서버렌더링에서는 호출되지 않음.

## 4. Destruction


참고: [Vue.js 공식문서](https://kr.vuejs.org/v2/guide/instance.html) 
[ Vue.js 2.0 라이프사이클 이해하기](https://medium.com/witinweb/vue-js-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-7780cdd97dd4)
