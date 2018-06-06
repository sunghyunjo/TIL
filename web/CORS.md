# CORS (Cross-Origin Resource Sharing) - HTTP 접근 제어
시스템에서 타 도메인 간의 자원 호출 권한을 결정하는 것 (승인하거나, 차단)

## 동일 출처 정책(Same-Origin Policy)
- 보안 상의 문제로 웹 브라우저들이 외부 host로 접근하는 것을 막기 시작하였다.
> ex. /page/image.html 과 같이 현재 host의 하위 경로로만 접근이 가능했고,
> www.google.com 처럼 외부 host url로 접속이 불가능했다.
- 이런 이슈를 해결하기 위해 외부 도메인으로의 접근 권한을 결정하는 것을 CORS라고 한다.

- 같은 출처라는 의미는 '프로토콜, 호스트명, 포트'가 같다는 것을 의미

## CORS요청 종류
- Simple/Preflight
- Credential/Non-Credential

### Simple Request
- GET, HEAD, POST중의 한 가지 방식을 사용해야 한다.
- POST방식일 경우, Content-type이 셋 중 하나여야 한다.
	* application/x-www-form-unlencoded
	* multipart/form-data
	* text/plain
- 커스텀 헤더를 전송하지 말아야 한다.
- 서버와 요청, 응답이 1:1로 이루어진다.

### Preflight Request
- Simple Request 조건에 해당하지 않을 때, Preflight Request방식으로 요청한다.
	* GET, HEAD, POST외의 다른 방식으로도 요청을 보낼 수 있고,
	* application/xml처럼 다른 Content-type으로 요청을 보낼 수도 있으며,
	* 커스텀 헤더도 사용 가능하다.
- 예비요청과 본 요청으로 나뉜다.
	: 서버에 예비요청(Preflight Request)를 보내고, 서버는 예비 요청에 대해 응답하고,
	 그 다음, 본 요청(Actual Request)를 서버에 보내고, 서버도 본 요청에 응답한다.

##### 예비요청과 본 요청을 프로그래머가 구분해서 처리하지 않아도 된다.
프로그래머가 Access=Control-계열의 Response Header만 적절히 정해주면,
OPTIONS요청으로 오는 예비 요청과 GET, POST, HEAD, PUT, DELETE 등의 본 요청은 서버가 알아서 처리한다.


