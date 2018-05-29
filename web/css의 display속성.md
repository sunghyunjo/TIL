# CSS : display속성 이해하기



**display**는 우리가 CSS에서 레이아웃을 짜기 위한 중요한 속성이다. 레이아웃을 짜는데 있어 가장 중요한 태그는 div와 span이다. 두 태그는 아무런 '의미가 없는' 태그이다. 단지, 레이아웃을 만드는 태그일 뿐이다. 아무런 의미가 없는 태그가 왜 두 가지나 존재하는 것일까? 그 이유는  두 태그는 각기 다른 display속성을 가졌기 때문이다. **span**은 **inline**,** div**는 **block**으로 지정되어 있다. 

이 두 속성의 차이에 대해 알아보기 위해 가장 자주 사용하는 display속성에 대해서 알아보자. 사실 display속성은 총 16가지 이다. 하지만, 대부분 거의 잘 쓰지 않으며, 가장 많이 사용하는 것은 

### **block, inline, none, table, table-cell, inline-block**

이라고 할 수 있다.




-----


## none

none은 말 그대로 없애버린다는 것이다. 주로 탭 메뉴나 드롭다운 메뉴처럼 없어졌다 나타나는 기능을 구현할 때 숨겨놓는 용도로 자주 사용하는 속성이다. 어떠 식으로 디자인을 했든 해당 태그에 none을 주면 모니터 상에서 사라져 버리게 된다.

일단,  div와 span의 차이를 알아보기 위해 하나씩 만들어 보자. 모양을 식별하기 위해서 백그라운드 색상도 추가하였다.
![](http://postfiles8.naver.net/20120720_215/iyakiggun_1342710223090ce2Ma_JPEG/display1.jpg?type=w2)

이것이 기본 모습이다. 가장 큰 차이는 가로의 크기가 눈에 띄게 다르다는 것이다.

이들에게 width와 height값을 줘보자
![](http://postfiles11.naver.net/20120720_90/iyakiggun_1342710223227y5TQL_JPEG/display2.jpg?type=w2)


 div는 정확하게 사이즈대로 만들어진 반면, span은 아무런 변화가 없다. 이는, span의 기본 속성이 inline으로 지정되어져 있기 때문이다. 
 
 또 다른 비교를 해보자. 똑같은 태그를 연속으로 세 개씩 만들어보자.
 ![](http://postfiles1.naver.net/20120720_256/iyakiggun_1342710223366ArpHe_JPEG/display3.jpg?type=w2)
 
div는 한 줄에 하나씩 생겼고, span은 한 줄에 세 개가 되었다. 

## block, inline
이로써 div와 span의 가장 큰 차이를 한 눈에 알 수 있을 것이다. div는 block이고, span은 inline으로 지정되어 있다고 말했듯이.** block은 width를 100%로 차지하며 한 줄에 하나씩 생기는 반면, span은 inline이라는 말의 뜻처럼 직렬, 횡렬로 한 줄에 자신의 크기만을 가진 채로 여러개가 붙어있을 수 있다는 것**이다.

그렇다면, 왜 div는 가로세로 사이즈가 적용이 되는데 span은 안되는 것일까? 
**span은 inline속성을 가진 채로 자신의 사이즈만큼 크기가 변화**한다. 때문에 임의적으로 사이즈를 조절할 수 없는 것이다. 

그렇다면 span처럼 inline 속성을 가진 태그의 사이즈를 정해주고 싶다면 어떻게 해야할까?

## inline-block
그럴땐** diplay속성을 변경**해주어야 한다. 크기 제어가 가능한 block이나, inline-block 속성을 사용하면 된다.** inline-block이란, inline처럼 자신의 공간만을 차지하지만 block처럼 특정한 사이즈를 지정**해줄 수도 있다. 
![](http://postfiles12.naver.net/20120720_171/iyakiggun_1342710223823geYiG_JPEG/display6.jpg?type=w2)

이 inline-block이라는 속성은 익스플로러에서 발생하는 여백 버그를 잡아줄 때 유용하게 쓰이는 속성이다. 가령 li태그로 리스트를 만들고 그 안에 이미지를 넣었는데 하단에 여백이 생성되는 사건이 종종 발생하곤 한다. 그럴 땐 inline이나 inline-block으로 속성을 변경해주면 된다. 하지만, IE6/7에서는 inline-block을 지원하지 못한다. 

#### vertical-align
inline-block속성은 브라우저에 따라 하단에 미세한 여백이 생기는 경우도 있다. 이 때를 위해서 세로정렬 값을 추가해줘야 한다. 이땐 vertical-align: top; 을 해주면 해결할 수 있다. 

## table-cell, table

그렇다면, 나머지 table과 table-cell 속성은 언제 사용할까?

가령 가로 사이즈가 100%인 어떤 메뉴를 하나 만든다고 하자. 그리고 그 안에 메뉴를 네 개 만들고 메뉴 사이즈고 각각 25%씩 지정해보자. 그렇다면 사이즈가 커지거나 작아지거나 메뉴 사이즈는 100%이고, 각각의 메뉴는 25%일 것이다. 하지만, 브라우저를 늘이거나 줄일 때 몇 px씩 여백이 생기는 현상을 발견할 수 있을 것이다. 
![](http://postfiles15.naver.net/20120720_142/iyakiggun_1342710223952uuOad_JPEG/display7.jpg?type=w2)
![](http://postfiles2.naver.net/20120720_193/iyakiggun_13427102241018XJmL_JPEG/display8.jpg?type=w2)
이러한 현상을 브라우저마다 약간씩 다르게 나타난다. 모바일이나 파이어폭스 등에서 확인하면 여백이 발생하지 않지만, 익스플로러나 크롬에서는 사이즈를 늘리고 줄이다보면 여백이 생겼다가 사라졌다가 한다. 즉, 엄밀히 따지면 100%가 아닌 것이다. 거기에다 border라도 집어넣는다면 사이즈가 커져서 아래로 쏟아져버리게 된다.

이럴 때에는 메뉴 바의 속성을 table로 하고 그 안에 들어가는 메뉴의 속성을 table-cell로 변경해주면 된다. table-cell이 들어가는 곳에 float값이 같이 들어가면 float의 속성만 적용이 되므로 float값은 지워줘야 한다.
![](http://postfiles11.naver.net/20120720_42/iyakiggun_1342710224272cWQew_JPEG/display9.jpg?type=w2)
![](http://postfiles4.naver.net/20120720_83/iyakiggun_1342710224406AWzXv_JPEG/display10.jpg?type=w2)
table-cell로 바꾼 뒤 확인하면, 더 이상 여백이 발생하지 않는 것을 볼 수 있다. table과 table-cell은 표와 같은 속성으로 변경해주는 속성이다. table, td, tr등이 기본 속성처럼 말이다. 



-----

## 태그의 기본 속성


block을 기본 속성으로 가진 태그는
### **div, p, ul, li, dl. dt, dd, h1~h6, form**
등이 있으며,

inline을 기본 속성으로 가진 태그는 
### **span, a, img, input, mark, strong, em, abbr** 
등이 있다.

주로 텍스트와 이미지에 관련된 요소라고 보면 이해가 쉽게 될 것이다.
