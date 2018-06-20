# javascript Scope
자바스크립트는 **함수레벨 스코프**, **블록레벨 스코프**, **렉시컬 스코프**규칙을 따른다.

> 자바스크립트는 ES6부터 블록레벨 스코프를 지원하기 시작했다.

## 함수 레벨 스코프 (function level scope)
**var**키워드로 선언된 변수나, **함수 선언식(function declaration)**으로 만들어진 함수가 갖는 스코프. 
함수 내부에서 유효한 식별자가 된다.
    
아래 코드는 아무 문제 없이 *blue*를 출력한다.
```
function foo() {
	if (true) {
		var color = 'blue';
	}
	console.log(color); // blue
}
foo();
```

## 블록 레벨 스코프 (block level scope)
ES6의 **let, const** 키워드로 선언된 변수는 블록 레벨 스코프 변수를 만든다.
```
function foo() {
	if (true) {
		let color = 'blue';
		console.log(color); // blue
	}
	console.log(color); // ReferenceError: color is not defined
}
```
let color 변수는 if블록 내부에서만 선언되었고, 이 때문에. if블록 내부에서만 참조할 수 있다. 그래서 if블록 외부에서 호출한 color변수는 not defined로 에러가 출력되는 것이다.
const로 선언된 변수도 마찬가지다.

## 렉시컬 스코프 (Lexical scope)
렉시컬 스코프(Lexical Scope = 정적 스코프)는 보통 동적 스코프(Dynamic Scope)와 많이 비교한다.
   
동적 스코프는 프로그램의 런타임 도중 실행 컨텍스트나 호출 컨텍스트에 의해 결정되고, 렉시컬 스코프에서는 소스코드가 작성된 그 문맥에서 결정된다. 대부분의 프로그래밍 언어들은 렉시컬 스코프 규칙을 따른다.

아래의 예시는 동적 스코프를 갖는 Perl언어와 렉시컬 스코프를 갖는 javascript를 비교한 것이다.

![렉시컬 스코프 vs 동적 스코프 비교](http://image.toast.com/aaaadh/alpha/2016/techblog/lexicalscopejs.png)

렉시컬 스코프를 따르는 자바스크립트는 **호출 스택과 관게없이 각각의(this를 제외한) 대응표를 소스코드 기준으로 정의하고, 런타임에 그 대응표를 변경시키지 않는다.**

[참고: 자바스크립트의 스코프와 클로저](http://meetup.toast.com/posts/86)
