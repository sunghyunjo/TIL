# REST API 

## REST 구성
- 자원 RESOURCE : URL
- 행위 Verb : HTTP METHOD
- 표현 Representations

## REST의 특징
1) Uniform (유니폼 인터페이스)
	- URL로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍쳐 스타일

2) Stateless (무상태성)
	- 작업을 위한 상태정보를 따로 저장하고 관리하지 않음. 
	- 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API서버는 들어오는 요청만을 단순히 처리하면 됨.
		-> 서비스의 자유도가 높아지고, 서버에서 불필요한 정보를 관리하지 않음으로 구현이 단순해짐.

3) Cacheable (캐시 가능)
	- HTTP라는 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 	그대로 활용이 가능함.
		-> 따라서, HTTP가 가진 캐싱 기능이 적용 가능함.
		-> HTTP프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능함.

4) Self-Descriptiveness (자체 표현 구조)
	- REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있음.

5) Client-Server 구조
	- REST 서버는 API제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 됨.

6) 계층형 구조
	- REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 함.


## REST API 중심 규칙
1) URL은 정보의 자원을 표현해야 함. (리소스명은 동사보다 명사를 사용한다)
2) 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현
	=> 1, 2를 종합한 예제

> GET /members/delete/1 ===> DELETE /members/1


## URL설계 시 주의할 점
	1) 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용된다.
	2) URL마지막 문자로 슬래시를 포함하지 않는다.
	3) 하이픈(-)은 URL의 가독성을 높이는 데 사용된다.
	4) 밑줄(_)은 URL에 사용하지 않는다.
	5) URl경로에는 소문자가 적합하다.
	6) 파일확장자는 URL에 포함시키지 않는다.



