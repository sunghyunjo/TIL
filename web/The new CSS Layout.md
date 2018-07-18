# 새로운 CSS 레이아웃
> 해당 문서는 "The New CSS Layout[Rachel Andrew]" 책의 내용을 정리하였습니다.
  
  
## { position: sticky; }
이 속성은 우리가 흔히 알고 있는 `position: fixed` 와 `position: static`이 섞어 놓은 듯이 동작한다.
- 문서가 스크롤되기 전에는 **static요소**처럼 동작하다가, 문서 스크롤이 일정한 위치에 도달하면 **fixed요소**처럼 동작한다.
  
다음 예제는 일반적인 텍스트 문단이 있는 문서이다.
```
.box {
  width: 200px;
  top: 20px;
  position: sticky;
}
```
해당 box요소는 스크롤 하기 전에는 마치 static된 것처럼 같이 스크롤되다가,
스크롤이 되다가 viewport 상단 20px에 도달하면 상자는 fixed된 것처럼 고정된다.
아무리 스크롤을 해도 상단 20px자리를 고정하고 있는 것이다.

아래의 사진은 `sticky`속성을 활용한 예제이다.
![sticky예제](https://gedd.ski/img/sticky/moves.gif)

  
## multi-column layout
이 속성은 콘텐츠를 신문처럼 여러 단으로 나누는 방식을 말한다. 
```
.example {
  column-count: 3;
}
```
위의 코드는 콘텐츠를 세 칼럼으로 나누고 싶을 때 사용하는 방법이다.
`column-count`속성에 원하는 만큼의 칼럼 숫자를 값으로 설정하면 된다.
  
    
칼럼 갯수를 설정하는 대신, `column-width`속성을 사용해서 칼럼의 너비를 설정할 수도 있다.
웹 브라우저는 설정된 너비를 칼럼 한 개의 적정 너비로 인식하기 때문에, 실제 칼럼ㅁ의 너비는 컨테이너에서 사용할 수 있는 공간에 다라 적정 너비보다 더 넓어지거나 조아질 수 있다.
**그러나, `column-width`와 `column-count`를 함께 사용하면 `column-count`는 최대 칼럼 갯수로 사용된다.**
즉, 설정한 너비를 충분히 수용할 수 잇을 만큼 화면이 넓어지더라도 칼럼 갯수는 설정한 것 이상으로 많아지지 않는다.

  
```
.example {
  column-width: 300px;
  column-count: 3;
}
```

![multi-column layout 예제](http://xahlee.info/js/i/css_multi-columns_layout_2014-08-04.png)

> `column-width`를 사용할 때는 픽셀 너비를 정확하게 설정할 수 없다.


