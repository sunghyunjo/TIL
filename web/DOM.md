# DOM이란?
Document Object Model의 약자.
* **Document란?** HTML이나 XML문서와 같이 부분적 요소나 내용이 관련된 것들끼리 묶어서 존재하는 구조화 된 문서. (ex. 같은 내용은 같은 태그 안에 존재)

즉, 이렇게 구조화 된 문서에 스크립트를 이용하여 접근할 때에도, 구조적으로 표현하는 방식(Object Model)을 제공하는 것이 바로 DOM이라고 볼 수 있다.

![DOM Tree](https://www.w3schools.com/js/pic_htmltree.gif)

위의 트리 구조를 보면, Element(태그)외에도 Attribute, Text까지 모두 노드로 표현되어 있다. 즉, DOM을 통해서 스크립트가 문서 내의 모든 요소에 동적으로 접근할 수 있다는 것을 말한다.
그렇기 때문에 DOM을 통해서 문서 상의 요소에 접근하여 모양이나 내용, 속성 등을 조작할 수 있으며 새로운 요소나 내용을 만들어서 사용자가 임의적으로 문서 상에 적용할 수 있다.  그래서 DOM접근은 웹의 풍부한 상호작용을 위해 꼭 필요한 작업이다.

그러나, 순수한 JavaScript로 하면, 다수의 반복문과 긴 스크립트로 작성해야 하는 경우가 많아, 이를 해결하는 것이 jQuery의 장점 중 하나이다.


# jQuery의 DOM접근 vs 순수 JavaScript의 DOM접근

jQuery에서는 CSS의 선택자(Selector)방식을 사용하여 DOM에 접근한다. 
```
$("p") //p가 선택자(Selector)
```
선택자에는 여러 종류가 있는데, 자주 사용하는 선택자로는 id Selector, class Selector, tag Selector 등이 있다. 

### JavaScript와 jQuery의 DOM접근 비교

위에서 설명하였듯이, jQuery에서는 jQuery DOM객체 생성함수인 $();을 사용하여 Selector로 간단하게 DOM에 접근한다. 하지만, JavaScript에서는 브라우저의 여러 가지 내장 메소드를 통해 DOM에 접근한다.

|  | jQuery | 순수 JavaScript(1) |순수 JavaScript(2) |
| -------- | -------- | -------- | -------- |
| id Selector   | $("#foo")    | document.getElementById("foo"); |document.querySelector("#foo"); |
| tag Selector   | $("foo")    | document.getElementsByTagName("foo"); |document.querySelectAll("#foo"); |
| Class Selector   | $(".foo")    | document.getElementsByClassName("foo"); |document.querySelectAll("#foo"); |
| attribute Selector   | $("[name = foo]")    | document.getElementsByName("foo"); |document.querySelectorAll("[name = foo]"); |


표에 나오는 것처럼 jQuery의 간단한 문법에 비해, 순수 JavaScript의 DOM접근은 다양한 종류의 긴 이름을 가진 메소드를 사용한다. 

태그를 통한 DOM요소의 접근의 예를 한 가지 보자.
```
$(function)(){
		var a = document.getElementsByTagName("h2");
		console.log(a);
		
		var b = document.querySelectorAll("h2");
		console.log(b);
		
		var c = $("h2");
		console.log(c);
});
```

이 코드를 통해 반환하는 객체들을 살펴보자면, 
jQuery Selector를 통해 반환된 객체는 jQuery객체이고, document.getElementsByTagName을 통해 반환된 객체는 HTMLCollection, document.querySelectorAll을 통해 반환된 객체는 NodeList이다.

즉, jQuery를 통해 선택하게 되면 jQuery객체, 순수 JavaScript를 통해 선택하면 native JavaScript객체를 반환한다는 것을 알 수 있다.


# jQuery Selector의 내부적 처리
그러나 속도의 문제를 고려해야 할 필요가 있다. jQuery는 선택자를 사용하여 간단하게 DOM에 접근할 수 있지만, 각 방법에 따라 내부적인 처리가 다르기 때문에 속도의 차이가 생길 수 있다는 단점이 있다.

사실 jQuery는 내부적으로 가능하다면, 속도 향상을 위해 순수 JavaScript의 브라우저 내장 메소드를 사용하게 된다. 그러나 브라우저의 내장 메소드가 지원되지 않는 경우, 모든 DOM노드를 순회해야 하기 때문에 속도가 느려진다.

위의 브라우저 내장 메소드 중에서도 getElementsByClassName()은 IE8이하에서는 지원하지 않고, querySelector() / querySelectorAll() 메소드는 특정 버전 미만의 브라우저에서는 지원하지 않기 때문에 브라우저에 따라서도 속도가 달라질 수 있다.
[브라우저 호환성 참고 링크](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector
)

그렇다면, getElementsBy-()의 메소드를 사용할 수 없는 Selector들은 (가능한 브라우저일 경우에) querySelector()나 querySelecotrAll()메소드를 사용할 것이다. 하지만, 이 조차도 불가능해서 jQuery가 직접 DOM tree를 순회해야만 하는 선택자들이 있다.

그것은 바로 jQuery만의 확장된 선택자이다. 예를 들자면,  :last, :first, :even, :eq() 등이 있다. 이는 css선택자 표준에 포함되지 않은 것으로  jQuery에서만 사용가능한 선택자이다. 



