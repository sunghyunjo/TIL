# Local Storage, Session Storage
- 로컬 스토리지와 세션 스토리지는 HTML5에 추가된 저장소이다.
- 쿠키에 대한 제약들을 해소하고, 좀 더 다양하게 클라이언트에 많은 기능들을 넣고자 추가되었다. 
- 간단한 key와 value를 저장할 수 있는 key-value형태의 스토리지이다.

## Local Storage와 Session Storage의 차이점

#### 가장 큰 차이점은 데이터의 *영구성*이다.

- 로컬스토리지의 데이터는 사용자가 지우지 않는 이상 계속 브라우저에 남아 있다. 
- 하지만, 세션 스토리지의 데이터는 윈도우나 브라우저 탭을 닫을 경우 제거된다.

**즉, 지속적으로 필요한 데이터(자동 로그인 등)는 로컬 스토리지에 저장하고, 잠깐 동안 필요한 정보(일회성 로그인 정보 등)는 세션 스토리지에 저장하면 효율적이다.**
그러나, 비밀번호와 같은 중요한 정보는 절대 저장해서는 안된다! 클라이언트 부분에 저장하기 때문에 언제든 해커의 타켓이 될 수 있다.

## 그렇다면 Cookie(쿠키)는 무엇일까?
쿠키는 만료 기한이 있는 key-value 저장소이다.
```
document.cookie; 
```
document.cookie하면 현재 쿠키 정보가 나온다.
- 쿠키는 4kb용량 제한이 있다.
- 매 서버 요청마다 서버로 쿠키가 같이 전송된다.
  
#### 왜 서버에 쿠키가 전송될까?

그 이유를 알아보기 위해서는 HTTP요청에 대해 간단히 먼저 알아야 한다. 
HTTP요청은 상태를 갖고 있지 않다. 다시 말하자면, 브라우저에서 서버로 나에 대한 정보를 가져오라는 GET /me라는 요청을 보낼 때, 서버는 요청 자체만으로는 그 요청이 누구에게서 오는 지 알 수 없다. 때문에, 응답을 보낼 수 없는 것이다.
  
**이 때,** 쿠키에 나에 대한 정보를 담아 서버로 보낸다면, 서버는 쿠키를 읽어 내가 누군지 파악할 수 있다. 쿠키는 처음부터 서버와 클라이언트 간의 지속적인 데이터 교환을 위해 만들어졌기 때문에 서버로 계속 전송되는 것이다.
    
**하지만 이것이 문제로 다가오기도 한다.**

### 쿠키의 제약점
만일 4kb가 꽉 찬 쿠키가 있다면, 요청을 할 때마다 기본적으로 4kb 데이터를 계속 사용한다. 
이 중에는 서버에 필요하지 않은 데이터도 존재할 것이기 때문에, 데이터 낭비가 발생하게 된다. 
*그런 불필요한 정보들을 Local Storage와 Session Storage에 저장하게 되는 것이다.*
이 두 저장소의 데이터는 서버로 자동 전송되지 않기 때문이다.
   
즉, 정리하자면 쿠키의 제약은 아래와 같다.
- 4kb의 데이터 저장 제한
- 쿠키는 매 HTTP요청에 포함되어 웹 속도 저하의 원인이 될 수 있다.
- 같은 쿠키는 도메인 내의 모든 페이지에 같이 전달된다.
- HTTP요청에 암호화 되지 않고 보내기 때문에, 보안에 취약하다.
- 쿠키는 사용자의 로컬에 텍스트로 저장되어 있어 쉽게 접근하고 내용 확인이 가능하다.
   
   
## Local Storage (로컬 스토리지)
- 로컬 스토리지는 window.localStorage에 위치한다.
- key, value 저장소이기 때문에, key-value를 순서대로 저장하면 된다.
- 저장할 수 있는 값으로는 String, Boolean, null, undefined등을 저장할 수 있지만, 모두 문자열로 변환된다.
- key도 문자열로 변환된다.

```
localStorage.setItem('name', 'sunghyunCho');
localStorage.setItem('birth', 1996);
localStorage.getItem('name'); // sunghyunCho
localStorage.getItem('birth'); // 1996 (문자열)
localStorage.removeItem('birth');
localStorage.getItem('birth'); // null (삭제됨)
localStorage.clear(); // 전체 삭제
```

```
localStorage.setItem('object', { a: 'b' });
localStorage.getItem('object'); // [object Object]
```
위의 코드처럼 객체는 제대로 저장되지 않고, toString메소드가 호출된 형태로 저장된다. 
즉, [object 생성자]형으로 저장되는 것이다. 
    
**객체를 저장하기 위해서는 2가지 방법이 있다.**
1. 그냥 key-value 형태로 풀어서 여러 개를 저장할 수 있다.
2. 한번에 한 객체를 통째로 저장하기 위해서는 `JSON.stringfy()`를 해야한다.
즉, 객체 형식 그대로 문자열로 변환하는 것이다. 받을 때는 `JSON.parse`를 통해 받는다.
```
localStorage.setItem('object', JSON.stringify({ a: 'b' }));
JSON.parse(localStorage.getItem('object')); // { a: 'b' }
```

이처럼 데이터를 지우기 전까지는 계속 저장되어 있기 때문에, 사용자의 설정(보안에 민감하지 않은 정보들)이나 데이터들을 넣어두면 된다.
   

    
## Session Storage (세션 스토리지)
- 세션스토리지는 window.sessionStorage에 위치한다. 
- 로컬스토리지처럼 clear, getItem, setItem, removeItem, key 등 같은 메소드를 용한다.
- 하지만, 로컬스토리지와는 다르게 데이터가 영구적으로 보관되지 않는다.


## 마무리하며..
보안이나 아직 해결되지 않은 문제점들로 인해 웹스토리지를 사용하는 것을 항상 권장하는 것은 아니다. 간단한 기능들인데 없어도 되고 있으면 좋은 기능들을 웹 스토리지를 사용해 구현하면 좋을 것이다. 대표적인 예들을 아래와 같다.

- '오늘 하루 열지 않기'
- sesstionStorage를 활용해서 사용자가 '입력폼'을 입력하다가 페이지에서 벗어난 경우 백업/복구
- 글쓰기를 하다가 사용자가 창을 벗어난 경우 관련 작성하던 내용 백업/복구용
- 웹서버에 필수적으로 접근해야 하는 캐쉬용(캐쉬로 먼저 서비스 제공, 차후에 업데이트)
- 웹페이지의 개인화 설정들에 대한 저장과 제공(캐쉬로 활용)
- 현재 읽은 글의 히스토리 저장(카운팅, 읽은글 표시 등으로 활용)
- Canvas나 이미지에 대한 임시 저장 기능(base64로 변환)
- 웹페이지간 정보 전달(웹서버를 경유하지 않고 정보 로컬에 유지)
- 중요 CSS 저장용(참조: https://speakerdeck.com/patrickhamann/css-and-the-critical-path-cssconfeu-september-2014)

[참고1](https://www.zerocho.com/category/HTML/post/5918515b1ed39f00182d3048)
[참고2](http://unikys.tistory.com/352)
