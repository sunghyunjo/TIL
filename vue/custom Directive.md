# custom Directive(사용자 지정 디렉티브)
v-modle과 v-show이외에도 사용자가 원하는 디렉티브를 등록하고 사용할 수 있다.

## 예제
```
// 사용자 정의 디렉티브 등록
Vue.directive('focus', {
	// 바인딩 된 엘리먼트가 DOM에 삽입되었을 때
	inserted: function (el) {
		// your code
	}
})
```

해당 디렉티브를 로컬로 등록하기 위해서는 directives옵션으로 지정한다.
```
dirctives: {
	focus: {
		// 디렉티브 정의
		inserted: function(el) {
			el.focus()
		}
	}
}
```

이 후, 모든 요소에서 새로운 v-focus 속성을 사용할 수 있다.

## 훅 함수
디렉티브를 정의할 때, 여러가지 훅 함수를 사용할 수 있다.(모두 반드시 필요한 것은 아님.)
#### bind 
- 디렉티브가 처음 엘리먼트에 바인딩 될 때, 한번만 호출
- 일회성 설정을 이곳에서 할 수 있다.

#### inserted
- 바인딩 된 엘리먼트가 부모 노드에 삽입 되었을 때, 호출된다.
- 이 훅 함수를 쓴다는 것은 부모 노드가 존재한다는 것을 보장한다. 
- 하지만, 부모 노드가 반드시 document내에 있는 것은 아니다.

#### update
- 포함하는 컴포넌트가 업데이트 된 후, 호출된다.
- 그렇지만, 자식 컴포넌트가 업데이트 되기 전일 가능성이 있다.
- 디렉티브 값은 업데이트로 변경 여부를 확인할 수 없지만, vinding의 현재 값과 이전 값을 비교하는 과정을 통해 불필요한 업데이트를 하지 않을 수 있다.

#### componentUpdated
- 포함하고 있는 컴포넌트와 그 자식 컴포넌트들이 업데이트 된 후 호출된다.

#### unbind
- 디렉티브가 엘리먼트로부터 언바인딩된 경우에만 한번 호출된다.

## Directive Hook Parameter
Directive hook은 아래의 값들을 파라미터로 사용할 수 있다.

#### el
- 디렉티브가 바인딩 된 엘리먼트

#### binding
- 아래의 속성을 가진 객체
	- name : 디렉티브 이름, v- 프리픽스가 없다.
	- value : 디렉티브에서 전달받은 값. 
		만일, v-my-directive="1 + 1"인 경우, value는 2이다.

#### 그 외, [참고문서](https://vuejs.org/v2/guide/custom-directive.html)에서 확인할 것. 

