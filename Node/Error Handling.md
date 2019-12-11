## Basic Usage

- Express 제공 기본 error handling
- 4개의 파라미터 사용 (err, req, res, next)

```js
app.use(function (err, req, res, next) {
  console.error(err);
  res.status(500).send('Something broke!');
});
```

- error hadler는 다른 미들웨어와 라우터 호출을 정의한 후, 마지막으로 정의할 것.

```js
const express = require('express');
let app = express();

//router
app.get('/', function (req, res, next) {
  next('error occur!!');
});

//error handler
app.use(function (err, req, res, next) {
  console.log(err);
  res.status(500).send('Something broke!');
});

app.listen(3000, function() {
  console.log('http://localhost:3000/');
});
```

- request가 들어왔을 때, next('error')를 호출하면, 중간에 등록된 미들웨어는 모두 건너 뛰고 바로 error callback이 실행된다.

```js
const express = require('express');
let app = express();

// middle ware 1
app.use(function (req, res, next) {
  console.log('middle ware 01');
  next('middle ware 01 error occur!'); // 에러 발생
});

// middle ware 2, skip 됨
app.use(function (req, res, next) {
  console.log('middle ware 02');
});

// router, skip 됨
app.get('/', function (req, res, next) {
  console.log('router');
  next('error occur!');
});

// error handler
app.use(function (err, req, res, next) {
  console.error(err);
  res.status(500).send('Something broke');
});

app.listen(3000, function() {
  console.log('http://localhost:3000');
});

// result
// middle ware 01
// middle ware 01 error occur!
```

### multiple error handler

- 아래와 같이 next를 이용하여 다음 error handler로 넘겨줄 수 있으며, error가 발생했을 때 처리를 함수를 나눠서 구조화할 수도 있다.

```js
const express = require('express');
let app = express();

app.use(function (req, res, next) {
  next('request error'); // 다음 error handler로 넘기기
});

app.use(logErrorHandler);
app.use(requestErrorHandler);

// 어떤 에러가 발생했는지를 기록하는 역할
function logErrorHandler(err, req, res, next) {
  console.log('logErrorHandler', 'record :', err);
  next(err);
}

// client에게 error일 때 응답을 내려주는 역할
function requestErrorHandler(err, req, res, next) {
  console.error('requestErrorHandler', err);
  res.status(500).send('send error response');
}

app.listen(3000, function () {
  console.log('http://localhost:3000/');
});

// result
// logErrorHandler record : request error
// requestErrorHandler request error
```

**잠깐, 알아두고 갈 사항**

일단 먼저 아래의 내용을 이해해야 한다. 

- 하나의 경로에 다수의 라우트를 설정한 경우, 아래 예제와 같이 첫번째 router에서 response를 보내기 때문에 두번째 route는 절대 실행되지 않는다.

```js
router.get('/user/:id', function (req, res, next) {
  console.log('req.params.id', req.params.id);
  next();
}, function (req, res, next) {
  res.send('normal');
});

router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id);
  res.send('special');
});
```

아래의 예제를 보자.

- user/0으로 접근할 경우 next('route')가 호출되어 error handler로 이동하는 것이 아니라, 다음 route로 이동하게 된다.

    → next안에 인자가 'route'로 들어오면 다음 route로 이동함.

- user/1로 접근하면 'normal'이란 글자를 볼 수 있다.

```js
router.get('/user/:id', function (req, res, next) {
  if (req.params.id === 0 ) {
    next('route'); // error핸들러로 이동하지 않고, 다음 route로 넘어가게 된다.
  } else {
    next();
  }
}, function (req, res, next) {
  res.send('normal');
});

router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id);
  res.send('special');
});

app.use('/', router);

app.listen(3000, function () {
  console.log('http://localhost:3000/');
});
```

## Error를 중간에서 처리하게 된다면?

```js
const express = require('express');
let app = express();

// error handler
app.use(function (err, req, res, next) {
  console.error(err);
  res.status(500).send('Something broke');
});

// router
app.get('/', function (req, res, next) {
  next('error occur!');
});

app.listen(3000, function () {
  console.log('http://localhost:3000');
});

// output :
// 'error occur!!'
```

- 위의 예제는 'error occur'가 출력된다. 이를 통해, error handler가 앞에 있다면 에러가 제대로 처리되지 않는다는 걸 알 수 있다.
- error handler를 next(error); 호출 전에 등록하면 기본 오류 핸들러가 실행된다.
    
    → 기본 오류 핸들러란 ? 미들웨어 함수 스택 끝에 추가되어 있음.
  ```js
    const express = require('express');
    let app = express();
    
    // router
    app.get('/', function (req, res, next) {
    	error;
    	next('error occur');
    }); 
    
    /*
    	error handler 
    
    app.use(function (err, req, res, next) {
    	console.log(err);
    	res.status(500).send('Something broke')
    });
    */
    
    app.listen(3000, function() {
    	console.log('https://localhost:3000/');
    });
  ```

- **next(error)로 오류를 전달 했는데 해당 오류를 처리하지 못한 경우** 기본 오류 핸들러가 처리하는데, 기본적으로는 next로 전달된 parameter를 출력한다.

    이외의 에러가 발생할 경우 기본 오류 핸들러는 Stack Trace도 함께 출력한다. 이는 production mode에서는 실행되지 않으므로 실제 배포 시, NODE_ENV를 production으로 설정해줘야 한다.

![stack trace 출력](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F2701353458FC964B159D90)


- [참고자료1](https://dev-momo.tistory.com/entry/nodejs-error-handling)
- [참고자료2](http://expressjs.com/en/guide/error-handling.html)
