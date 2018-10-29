# URL & URI & URN
URL과 URI는 아직 많이 혼동되고 있는 개념이다.

## URI (Uniform Resource Identifier)
- URI는 인터넷의 우편물 주소 같은 것으로, 정보 리소스를 고유하게 식별하고 위치를 지정할 수 있다.
- 이 URI에는 두 가지 형태가 있는데 이것이 URL, URN이라는 것이다.

### URL (Uniform Resource Locator)
- URL은 특정 서버의 한 리소스에 대한 구체적인 위치를 서술한다.
- URL은 리소스가 정확히 어디에 있고 어떻게 접근할 수 있는지 분명히 알려준다.

##### 예시
	http://naver.com - Naver 사이트의 URL
	http://img.naver.net/static/www/dl_qr_naver.png - Naver 앱 QR 코드의 이미지에 대한 URL
	http://news.naver.com/main/main.nhn?mode=LSD&mid=shm&sid1=104 - 네이버 뉴스에서 분류 중 "세계" 주제의 기사에 대한 URL

### URN (Uniform Resource Name) 
- URN은 콘텐츠를 이루는 한 리소스에 대해, 그 리소스의 위치에 영향 받지 않는 유일무이한 이름 역할을 한다.
- 이 위치 독립적인 URN은 리소스를 여기저기로 옮기더라도 문제없이 동작한다.
- 리소스가 그 이름을 변하지 않게 유지하는 한, 여러 종류의 네트워크 접속 프로토콜로 접근해도 문제가 없다.

##### 예시
다음의 URN은 인터넷 표준 문서 'RFC 2141'이 어디에 있어도 그것을 지칭하기 위해 사용할 수 있다.
	
	urn:ietf:rfc:2141 - 'RFC 2141'문서

-  URN은 아직 채택되지 않아, 접할 기회가 없었을 것이지만 URN은 **URL의 한계**로 인해 착수되었다.

#### URL의 한계
- URL은 주소이지 실제 이름이 아니다. 

	**즉, 특정 시점에 어떤 것이 위치한 곳을 알려준다는 것이다.**
	
예를 들어, 구글 검색에 노출된 http://www.ticketmonster.co.kr/deal/10 링크가 있다. 만일, 이 주소를 변경하고 싶어 http://www.ticketmonster.co.kr/deal/20 으로 URL을 변경하였면, 첫번째 URL로 접근 하면 페이지를 찾을 수 없게 된다.
이러한 단점으로 인해, 리소스가 옮겨지면 해당 URL을 더는 사용할 수 없다는 것이다.
그리고, 그 시점 기존 URL이 가지고 있던 객체를 찾을 방법이 없어진다.

이런 문제를 예방하기 위한 이상적인 방법은, 객체의 위치와 상관없이, 그 객체를 가리키는 실제 객체의 이름을 사용하는 것이다. 그렇게 된다면, 위치가 바뀌더라도 리소스의 위치를 찾을 수 있게 된다.

## 정리하자면,
- URL과 URN은 URI의 한 종류이다.
- 때문에, 모든 URL은 URI이고, 모든 URN은 URI이다.
- 위에서 언급했듯이, URL과 URN은 다르다.
- 그렇다는 건, 모든 URI는 URL이라고 말할 수 없다.

위의 내용을 다이어그램으로 만들어본다면 아래와 같다.

![URI, URN, URL 관계](https://t1.daumcdn.net/cfile/tistory/2416C94158D62B9E11)

쉽게 말하자면, **URI는 규약이고, URL은 규약에 대한 형태**라고 생각하면 혼동되지 않을 것이다.
URL과 URN은 URI의 부분집합이라고 생각할 수 있다.

[참고자료](http://mygumi.tistory.com/139)
