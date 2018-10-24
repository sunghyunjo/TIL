# HTTP

## HTTP Header
### 예시 
```
Get /test/test.htm HTTP/1.1
Accept: */*
Accept-Language: ko
Accept-Encoding: gzip, deflate
If-Modified-Since: Fri, 21 Jul 2006 05:31:13 GMT
If-None-Match: "734237e186acc61:a1b"
User-Agent: Mozilla/4.0(compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; InfoPath.1)
Host: localhost
Connection: Keep-Alive

HTTP/1.1 200 OK
Server: Microsoft-IIS/5.1
X-Powered-By: ASP.NET
Date: Fri, 21 Jul 2006 05:32:01 GMT
Content-Type: text/html
Accept-Ranges: bytes
Last-Modified: Fri, 21 Jul 2006 05:31:52 GMT
ETag: "689cb7f885acc61:a1b"
Content-Length: 101
```

### 요청 헤더
#### 1. GET/test/test.htm HTTP/1.1
- 요청 메소드 / 요청 파일 정보 / http버전

- **GET**: 지정된 리소스(URI)를 요청
- **POST**: 서버가 클라이언트의 폼 입력 필드 데이터의 수락을 요청. 클라이언트는 서버로 HTTP body에 데이터를 전송.
- **HEAD**: 문서의 헤더 정보만 요청하여, 응답 데이터(body)를 받지 않는다.
- **PUT**: 클라이언트가 전송한 데이터를 지정한 URI로 대체한다. ftp의 PUT과 동일하며, 클라이언트는 서버로 HTTP body에 데이터를 전송한다.
- **DELETE**: 클라이언트가 지정한 URI를 서버에서 삭제한다.
- **TRACE**: 클라이언트가 요청한 자원에 도달하기까지의 경로를 기록하는 루프백(loop back)검사용. 클라이언트가 요청 자원에 도달하기까지 거쳐가는 프록시나 게이트웨이의 중간 경로부터 최종 수신 서버까지의 경로를 알아낼 때 사용된다.

#### 2. Accept
- 클라이언트가 허용할 수 있는 파일 형식(MIME TYPE)으로 `*/*`은 특정 유형이 아닌 모든 파일형식을 지원한다는 의미이다.

#### 3. User-Agent
- 클라이언트 소프트웨어(브라우저, os 등)의 이름과 버전을 나타낸다.
- 위의 예시 메세지에서는 MS IE 6.0, 윈도우 xp, .NET Framework 1.1버전이 클라이언트에 설치되어 있음을 나타내고 있다.

#### 4. Host
- 요청을 한 서버의 Host

#### 5. If-Modified-Since
- 페이지가 수정되었을 때, 최신 버전 페이지 요청을 위한 필드.
- 만일, 요청한 파일이 이 필드에 지정된 시간 이후로 변경되지 않았다면, 서버로부터 데이터를 전송받지 않는다. 
- 이 경우, 서버로부터 notmodified(304) 상태코드를 전송받게 된다.

```
HTTP/1.1 304 Not Modified
Server: Microsoft-IIS/5.1
Date: Fri, 21 Jul 2006 06:23:04 GMT
X-Powered-By: ASP.NET
ETag: "689cb7f886acc61:a1b"
Content-Length: 0
```

위의 헤더 정보는 동일한 파일을 재 요청했을 때의 응답 헤더이다.

파일 변경사항이 없음으로 `304`(수정하지 않음)와 `Content-Length: 0`(데이터 받지 않음) 응답을 받았다. 이렇게 함으로써 http는 요청의 부하를 줄이고 있다.

#### 6. Refer
- 위의 예시에는 나와있지 않지만, 자주 등장하는 필드.
- 특정 페이지에서 링크를 클릭하여 요청 했을 경우 나타나는 필드로, 링크를 제공한 페이지를 나타낸다.

#### 7. Cookie
- 위의 예시에는 나와있지 않지만, 자주 등장하는 필드.
- 웹 서버가 클라이언트에 쿠키를 저장해 놓았다면, 해당 쿠키의 정보를 이름-값 쌍으로 웹서버에 전송한다.

#### 8. Accept-Language
- 클라이언트가 인식할 수 있는 언어로 우선 순위 지정이 가능하다.

#### 9. Accept-Encoding
- 클라이언트가 인식할 수 있는 인코딩
- 압축 방법으로 위의 예시에서는 서버에서 gzip, deflate로 압축한 리소스를 클라이언트가 해석할 수 있다는 말이 된다.
- 만일, 서버에서 압축 했다면 응답헤더에 Content-Encoding 헤더에 해당 압축 방법이 명시된다.

### 응답 헤더
```
HTTP/1.1 200 OK
Server: Microsoft-IIS/5.1
X-Powered-By: ASP.NET
Date: Fri, 21 Jul 2006 05:32:01 GMT
Content-Type: text/html
Accept-Ranges: bytes
Last-Modified: Fri, 21 Jul 2006 05:31:52 GMT
ETag: "689cb7f886acc61:a1b"
Content-Length: 101
```
#### 1. HTTP/1.1 200 OK
- HTTP 버전과 응답 코드(200 성공)

#### 2. Server
- 웹 서버 정보
- 예시에서는 MicrosoftIIS 5.1이다.

#### 3. Date
- 현재 날짜

#### 4. Content-Type
- 요청한 파일의 MIME타입을 나타낸다.
- Text/html은 Text 중 html파일임을 나타낸다.

#### 5. Last-Modified
- 요청한 파일의 최종 수정일을 나타낸다.

#### 6. Content-Length
- 헤더 이후 이어지는 요청한 파일의 데이터 길이. (바이트 단위)

#### 7. ETag
- 캐쉬 업데이트 정보를 위한 임의의 식별 숫자

[참고](https://12bme.tistory.com/325)
