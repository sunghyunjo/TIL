# Proxy Server
클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터나 응용 프로그램.
- Proxy : 서버와 클라이언트 사이에서 중계기로서 대리로 통신을 수행하는 기능
- Proxy Server : 중계 기능을 하는 것
![proxy](https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Open_proxy_h2g2bob.svg/350px-Open_proxy_h2g2bob.svg.png)
> 더 자세한 내용은 [위키백과 proxy참고](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9D%EC%8B%9C_%EC%84%9C%EB%B2%84)

보안 상의 관점으로 본다면, 보안상의 이유로 직접 통신할 수 없는 두 점 사이에서 **중계기 역할을 해주며 대리로 통신을 수행하는 기능**을 Proxy라고 한다.

### 특징
프록시 서버는 클라이언트와 서버의 입장에서 볼 때, 서로 반대의 역할을 하는 것처럼 보여진다.
- 클라이언트가 Proxy를 바라봤을 때 : Proxy가 Server와 같이 동작을 하게 된다.
- 서버가 Proxy를 바라봤을 때 : Proxy가 Client와 같이 동작을 하게 된다.

### 장점 
- Proxy Server는 요청 된 내용들을 **캐시를 이용해 저장**해둔다. 
때문에, 캐시 안에 있는 정보를 요구하는 요청에 대해서 원격 서버에 접속할 필요가 없게 됨으로 **전송 시간을 절약**할 수 있고, 
**불필요한 외부와의 연결을 하지 않아도 된다**는 장점을 갖는다.
- 또한, 외부와의 트래픽을 줄이게 됨으로써 **네트워크 병목 현상을 방지하는 효과**도 얻을 수 있다.

### 그렇다면, 우리는 왜 proxy를 설정해줘야 할까?
API를 가져다 쓰다보면, [CORS](https://github.com/sunghyunjo/TIL/blob/master/web/CORS.md)문제를 겪을 때가 있다.
이는, 처음 서브되는 리소스의 도메인과 다른 도메인의 리소스가 요청될 때, Cross origin http요청에 의해 수행되기 때문이다.
  
### 해결방법
- 간단히 cors모듈을 사용할 수 있다.
```
const cors = require('cors');

app.use(cors());
```

- 그 후, proxy설정을 담은 `proxy.conf.json`파일을 만든다.
```
{
   "/auth/abc": {
      "target": "http://localhost:3000",
      "secure": false,
      "logLevel": "debug"
   }
}
```
위와 같이 key에 route 값을 주고, target은 서버의 주소(ex. 3000port), http요청이므로 secure는 false, log는 debug수준으로 설정한다.
- 이제 package.json에서 `"start": "ng serve --proxy-config proxy.conf.json".` 로 설정한다면,
기존의 `http://localhost:3000/auth/abc`는 Front-end의 port + route로 연결된다.

#### 출처
[위키백과](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9D%EC%8B%9C_%EC%84%9C%EB%B2%84)
