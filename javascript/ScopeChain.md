# Scope Chain (유효범위 체인)
scope chain을 간단하게 설명하면, 함수가 중첩함수일 때, 상위함수의 유효범위까지 흡수하는 것을 말한다. 즉, 하위 함수가 실행되는 동안 참조하는 상위 함수의 변수 또는 함수의 메모리를 참조하는 것이다. 
  
가장 먼저, 쉬운 예를 통해 살펴보자.
```
function a() {
	var a = 1; 
	function b() {
		console.log(a);
	}
	b();
}

a();

// 실행결과 : 1
```

함수 a를 호출함으로써 함수a 안의 함수b가 호출되며 변수a를 출력하고 있다. 
어떻게 함수 b()에 선언도 안되어 있는 외부 변수 a를 출력할 수 있었을까?
여기엔 scope chain개념이 적용되었기 때문이다.

자바스크립트는 실행시점에 [Excecution Context(EC)](https://poiemaweb.com/js-execution-context)가 생성되고, 그 시점에 여러 행위들이 이루어진다.
     
다시 위의 코드를 보자면, 함수 a()와 b()가 실행되면서 생성된 EC에 의해 함수 b()내의 변수 a를 탐색하기 시작한다.
만약, 함수b() 안에 변수 a가 선언되어있지 않다면, 함수b()를 감싸고 있는 외부함수 a()를 탐색하게 된다. 이 때 외부함수 a() 안에 변수a가 존재한다면 a를 참조하게 되고, 만일 함수a에도 변수a가 없다면 계속적으로 함수를 감싸고 있는 상위 함수로까지 탐색과정이 올라가게 된다.

**만일,**결국 찾지 못하고 root인 Global Object EC까지 오게 됐는데도 변수a가 존재하지 않다면 **VM500:1 Uncaught Refrerence Error: a is not defined**라는 에러를 낳게되는 것이다.
  
이런 중첩 스코프의 탐색은 해당하는 변수를 찾거나, 외부 렉시컬 환경을 모두 참조했는데도 null이 된다면 탐색을 멈추게 된다.

아래의 이미지를 참고해보자.
![scope chain : Lexical Environment](http://image.toast.com/aaaadh/alpha/2016/techblog/scopchain.png)

## 정리해보자면,
가장 처음에 보였던 예제인 아래의 예제로 다시 설명하자면,
```
function a() {
	var a = 1; 
	function b() {
		console.log(a);
	}
	b();
}

a();

// 실행결과 : 1
```

위의 코드는 아래 그림과 같은 내부구조로 실행되게 된다.
![내부 구조](https://s3.ap-northeast-2.amazonaws.com/tyle.io.seoul/tyle.post/gU/gUL6NX3jTI1480309443_cont.png)
함수a()가 실행되면 Execution Context(EC)가 생성되고, 렉시컬 환경이 함수 a()의 정보를 담으며 생성된다. EC b(), EC a(), Global이 생성되면서 외부 렉시컬 환경에 의해 연결이 이루어진다.

**즉,** 함수가 생성되면 Execution Context가 생성되고, 각 외부 렉시컬 환경과 연결되어 지면서 내부 함수 정보를 탐색할 수 있게 되는 것이다.

[참고문서1](http://knphouse.co.kr/91) [참고문서2](http://meetup.toast.com/posts/86)