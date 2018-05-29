# Vue.js 디렉티브 정리 (https://kr.vuejs.org/v2/api/index.html)

## 1. v-text 
	: {{ message }}와 동일한 기능
	ex) <div v-text="name"></div>
## 2. v-html
	: v-text처럼 렌더링 형태가 html이라는 것을 알려주기 위한 디렉티브
		!! 서버측에서 불필요한 부분은 필터링하게 할 것.
## 3. v-show
	: 해당 element가 보여질 것인지 아닌지를 값(T/F)으로 지정하는 것.

## 4. v-if
	: 조건문 

	```
	ex) <div v-if="value > 5"></div>

	(js 지정)
	var app = new Vue({
		el: '#app',
		data: {
			value: 0
		}
	});
	```

## 5. v-else
	: 위의 조건문이 만족하지 않을 때.
	: v-else의 디렉티브 값은 따로 설정하지 않아도 됨.
## 6. v-else-if
	: 다중 조건문을 사용할 때. 평소아는 else-if문과 같음.
		!! 단, v-if 와 v-else 사이에 있어야함.
## 7. v-pre
	: 특정 엘리먼트를 무시하는 데에 사용됨.
	ex) <div v-pre> {{ 이건 그대로 렌더링해줘요. }} </div>
## 8. v-cloak
	: Vue인스턴스가 제대로 준비되기 전까지 템플렛을 위한 HTML코드를 숨기고 싶을 때 사용.
	: 값 설정은 불필요하고, 그냥 추가하기만 하면 됨.
	: css에서 v-cloak에 대한 display를 설정해줘야 함.
	ex) <div id="app" v-cloak> </div>
## 9. v-once
	: 컴포넌트를 딱 한번만 렌더링함. 정적인 데이터를 보여주는 데에 사용 적합.

