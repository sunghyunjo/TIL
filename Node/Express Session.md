# express-session 이란 ?
- express 프레임워크에서 세션을 관리하기 위한 미들웨어
- 인증기능을 구현하기 위해 많이 쓰임 ex . login, logout

## express-session option 값들
```js
app.use(session({			// app.use 란 페이지에 접속했을 때마다 use안에 있는 함수들이 실행되도록 한다. 즉, 해당 코드는 session이 시작되게 한다.
	secret: 'keyboard cat',
	resave: false,
	saveUninitialized: true,
}))
```
- secret : required option. 다른 사람에게 공개되선 안됨. 버전관리에 올릴 땐 변수처리 해서 공개되지 않도록!
- resave : session데이터 값이 변하기 전엔 값을 저장하지 않는다. true면 session이 변경되지 않아도 저장한다.
- saveUninitialized : session이 핖요하기 전까지 구동하지 않는다. false로 하면 필요여부에 상관없이 세션이 구동되어 서버에 큰 부담을 줄 수 있다.


- req 객체에 session을 자동으로 추가해준다. 
- session data를 메모리와 같은 휘발성이 있는 곳에 저장하면, 사용자의 정보가 날아가버리기 때문에 휘발성이 없는 session-store 에 저장해야 한다.


## session store에 저장하는 방법
```js
app.use(session({
	secret: 'keyboard cat',
	resave: false,
	saveUninitialized: true,
	store: new FileStore().
}))
```
- option값 중 store값에 원하는 session-store 를 입력하면 리로드 시 저장이 된다.
- request header의 cookie값으로 session id가 전달됨.
- session middleware가 저  cookie값을 가지고 session store에 id값에 대응1되는 정보를 읽는다.


## session 저장하기
```js
request.session.is_logined = true;
request.session.nickname = authData.nickname;

response.redirect('/');
```
session에 저장하는 단계가 redirect 함수보다 느리게 실행되는 경우, 사용자는 홈으로 이동했지만 로그인은 되어있지 않은 현상이 지속되다가 나~중에 session이 저장된 후 페이지에 접속하면 로그인이 되어있는 현상을 마주하게 된다.

#### 이런 현상을 방지하기 위해, 아래와 같이 session을 저장한다. 
```js
request.session.is_logined = true;
request.session.nickname = authData.nickname;
request.session.save(function () {
	response.redirect('/');
});
```
위와 같이 save함수를 작성하면 session을 저장한 후에 사용자를 redirection할 수 있게 된다.
