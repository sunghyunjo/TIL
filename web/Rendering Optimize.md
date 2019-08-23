# 렌더링 엔진과 성능 최적화
## 렌더링 엔진
- 렌더링 엔진 : 브라우저 화면에 요청된 페이지를 표시하는 것. HTML 및 XML 문서와 이미지들을 표시할 수 있다. 추가적인 플러그인을 사용하면 엔진은 PDF와 같은 다른 문서도 표시할 수 있다.

### 렌더링 엔진 종류
- **Gecko** : Firefox
- **WebKit** : Safari
- **Blink** : Chrome, Opera (15 버전부터)

## 렌더링 과정
렌더링 엔진은 네트워킹 레이어로부터 요청된 문서의 내용을 전달받는다.

1. HTML을 파싱해 DOM트리를 구성
2. 렌더 트리 구성
3. 렌더 트리 레이아웃
4. 렌더 트리 페인팅

### 1-1. DOM트리 구성
- HTML도큐먼트를 파싱하고 파싱된 요소들을 DOM트리의 실제 DOM노드로 변환하는 것.

```html
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" type="text/css" href="theme.css">
  </head>
  <body>
    <p> Hello, <span> friend! </span> </p>
    <div> 
      <img src="smiley.gif" alt="Smiley face" height="42" width="42">
    </div>
  </body>
</html>
```

위의 HTML에 대한 DOM트리는 다음과 같다.

![DOM트리 구성](https://camo.githubusercontent.com/49a42ee85440c6cbedaa7761ae4116e5974051a4/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a657a466f587167663931756d6c733946714f304873512e706e67)
기본적으로 각 요소는 모든 요소에 대한 부모 노드로 표시되며, 이 관계들은 DOM트리에 포함된다. 그리고 이것은 노드마다 재귀적으로 적용된다.

### 1-2. CSSOM 트리 구성
CSSOM은 **CSS 객체 모델 (CSS Object Model)**을 말한다. 브라우저가 페이지 내 DOM을 구성하는 동안, 아래와 같은 css파일을 만났다고 가정해보자.

```css
body { 
  font-size: 16px;
}

p { 
  font-weight: bold; 
}

span { 
  color: red; 
}

p span { 
  display: none; 
}

img { 
  float: right; 
}
```
HTML과 마찬가지로, 엔진은 CSS를 브라우저에서 동작할 수 있는 CSSOM으로 변환한다. 
다음은 CSSOM트리의 모습이다.

![CSSOM트리 예제](https://camo.githubusercontent.com/8d3471d1f38252fe785c3e558a229ab891f41fdb/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a355955317375326d647a48455135694469734b5579772e706e67)

### 2. 렌더 트리 구성
CSSOM트리에서 스타일 데이터와 함께 결합한 HTML의 시각적인 정보는 렌더 트리를 생성하기 위해 사용된다.

- 렌더트리 : 시각적 요소들이 화면에 표시되는 순서대로 구성된 트리. CSS + HTML을 시각적으로 표현한 것.
- 정확한 순서로 콘텐츠를 그릴 수 있도록 하는 것이 목적이다.
- 렌더 트리의 각 노드는 Webkit의 렌더러 또는 렌더 객체로 알려져 있다.

위의 DOM 렌더러 트리와 CSSOM 트리들은 다음과 같이 보여진다.

![렌더 트리 구성](https://camo.githubusercontent.com/26bd9038788e24141d1cf3f57bed43f93705b25f/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a5748525f30384144384150444954512d3443464467672e706e67)

렌더 트리를 구성하기 위해, 브라우저는 아래와 같은 일을 한다.

- DOM트리의 루트부터 시작해서 각각 시각적 노드들을 순회한다. (스크립트 태그나 메타 태그처럼 시각적인 태그가 아니면 출력되지 않고 생략된다.)
- 각각의 시각적인 노드들로부터 브라우저는 적절한 CSSOM 규칙을 찾아 적용한다.
- 브라우저는 콘텐츠를 스타일과 함께 시각적인 노드들을 보여준다.

### 3. 렌더트리 레이아웃
렌더러가 생성되고 트리에 추가될 때, 이 렌더러는 위치와 크기를 가지지 않는다. 이 위치값들을 계산하는 것을 레이아웃이라 부른다.

- 좌표계는 루트 렌더러(HTML도큐먼트 엘리먼트에서 `<html>`에 해당)를 기준으로 한다. top, left 좌표가 사용된다.
- 레이아웃은 재귀적인 과정이며, 루트 렌더러부터 시작된다. 
- 레이아웃은 부분 또는 전체 렌더러 계층을 통과하면서 재귀적으로 반복되고, 이를 필요로 하는 기하학 정보를 계산한다.
- 루트 렌더러의 위치는 `0,0`이고, 치수는 브라우저 윈도우에서 보이는 부분의 크기(뷰포트)이다.

### 4. 렌더트리 페인팅
이번 단계에서는 렌더 트리를 순회하고 화면 상에 콘텐츠를 보여주기 위해 렌더러의 `paint()`메서드를 호출해본다.
페인팅 과정은 Global이거나 Incremental일 수 있다.

- Global : 전체 트리가 다시 그려진다.
- Incremental : 전체 트리에 영향을 주지 않는 방법으로 렌더러 일부가 변경된다.

`paint()`는 렌더 트리를 만들고 레이아웃하기 위해 모든 HTML이 파싱될 때까지 기다리지 않는다. 콘텐츠 일부는 파싱되고 표시되며, 나머지 콘텐츠 항목들은 네트워크상에서 계속 처리된다.

## 렌더링 성능 최적화
만일 애플리케이션을 최적화하고 싶다면, 5가지 주요 영역에 집중해야 한다.

1. 자바스크립트
2. 스타일 계산
3. 레이아웃 
4. 페인트
5. 합성

### 1. 자바스크립트 최적화
- 시각적 업데이트를 위해 `setTimeout` 또는 `setInterval` 사용을 피하라. 이 함수들은 프레임의 어떤 지점에서 `callback`을 실행할 수 있다. 
- 장기 실행되는 자바스크립트 계산을 Web Worker로 바꾼다.
- 여러 프레임에 DOM 변화를 주입하기 위해 마이크로 태스크(micro-task)를 사용한다. 

	> 이것은 Web Worker가 접근할 수 없는 DOM에 접근이 필요할 때 사용된다. 이것은 기본적으로 큰 작업을 작은 작업으로 나누고, 작업의 특성에 따라 requestAnimationFrame, setTimeout, setInterval 함수 안에서 실행할 수 있음을 의미한다.
	
### 2. CSS 최적화
- selector의 복잡도를 줄인다. selector의 복잡도는 자체 스타일을 구성하는 나머지 작업들과 비교했을 때, 엘리먼트의 스타일을 계산할 때 필요한 시간의 50% 이상을 더 소비할 수 있다.
- 스타일 계산을 수행해야 하는 엘리먼트의 갯수를 줄인다. 

### 3. 레이아웃 최적화
- width, height, left, top 및 기하학과 관련된 속성들을 변경하려면 레이아웃 계산이 필요하다. 따라서 가능한 해당 속성들을 많이 변경하지 말아야 한다.
- 구형 레이아웃 모델에 대해 `flexbox`를 사용한다. 이는 빠르게 동작하고 애플리케이션에 큰 성능 이점을 만들 수 있다.
- 강제 레이아웃 동기화를 피한다. 엘리먼트에 일부 css를 동적으로 추가하는 행위처럼 스타일을 변경하는 경우, 브라우저는 먼저 스타일을 변경하고 레이아웃을 실행해야 한다. 이런 행위들은 시간과 자원을 많이 소비할 수 있으므로 피해야 한다.

### 4. 페인트 최적화
페인트 과정은 전체 작업에서 가장 많이 실행되므로 가능한 피하는 것이 중요하다.

- transforms 또는 opacity 이외의 속성을 변경하면 페인트가 실행되므로 덜 사용한다.
- 레이아웃이 발생하면 페이트가 발생하므로 레이아웃 조정을 피한다.

### 5. 합성 최적화
페이지의 부분들은 여러 레이어로 그려지기 때문에 페이지가 정확히 렌더링 되도록 순서대로 화면에 그려져야 한다.
겹치는 엘리먼트가 있을 경우, 최대한 순서대로 렌더링이 되로록 한다.

[참고 문서](https://github.com/codepink/codepink.github.com/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80:-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%97%94%EC%A7%84%EA%B3%BC-%EC%84%B1%EB%8A%A5%EC%9D%84-%EC%B5%9C%EC%A0%81%ED%99%94%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95) 
	