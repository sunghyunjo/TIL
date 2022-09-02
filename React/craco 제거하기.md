## 첫 시도 (subtree + craco 로 구성된 원래의 Merlin 상태에서 시도)

https://nextjs.org/docs/migrating/from-create-react-app#updating-packagejson-and-dependencies 

위 문서에 따라 react-router-dom을 제거하고, pages 폴더를 통해 라우트가 생성되는 Next 정석의 방식을 따라감.

But,  src/pages 안의 내용들을 모두 pages로 옮겨야 하는 등 작업량이 방대해질 것이라고 판단하여 가장 간단하게 라우트는 기존의 react-router-dom을 사용하는 형태로 남겨두어 SPA형태를 유지하고, 진입점을 Catch-All 할 수 있게하는 방법을 택하기로 함.



## pages/[[...app]].tsx 로 전략 수정  

https://nextjs.org/docs/migrating/from-create-react-app#single-page-app-spa

기존 package들은 유지하되, Next를 설치하여 yarn start script에서 craco만 제거 진행. 

그러자 오류가 빵빵..터지는데,,,,,,  

.

.

.

# 오류 해결 (발생한 순서대로 작성)

 ## React hooks를 잘못 쓰고 있다는 오류 발생 Invalid Hook Call warning

https://ko.reactjs.org/warnings/invalid-hook-call-warning.html 

여러 원인이 있었는데, hook 규칙을 위반했을 것이라곤 생각하지 않음 → why? 이미 잘되고 있던 코드라서

원인 찾아보니 React가 여러개 존재하는 경우 해당 오류가 발생하는 경우가 있다고 하여, npm ls react 명령어를 통해 여러개의 react가 존재하는 것을 확인함. 

subtree 구조때문일 것이라고 추측하였고, yarn berry 형태로 subtree가 제거된 브랜치에서 다시 Next 적용을 시도함.

→ subtree 때문이 맞았음. yarn berry로 변경한 코드 베이스에선 위 오류가 더이상 발생하지 않음.



## QueryClient 존재하지 않는 오류 

Next는 Sever-side 렌더링이라, DOM이 생성된 후 실행될 것이라 가정하에 짜여있는 기존 src/index 파일의 로직들이 DOM이 생성되기 전에 실행되며 생긴 이슈. 

기존 src/index에서 진행하던 rootElement에 각종 기본 셋팅 작업을 진행하는 부분을 함수로 빼고, 그 함수를 [[...app]].tsx에서 초기에 렌더하도록 수정함.

```ts
// 기존 

const rootElement = document.getElementById('root');
if (!rootElement) {
  throw new Error('root element not found');
}

ReactDOM.unstable_createRoot(rootElement).render(
  <React.StrictMode>
    ... // 변경된 부분 없음
  </React.StrictMode>,
);



// 변경 후

export default function Main() {
  return (
    <React.StrictMode>
      ... // 변경된 부분 없음
    </React.StrictMode>
  );
}
```




## index.css 와 같은 global.css 파일들을 실행하는 시점에 대한 오류 

Next구조 상, global.css는 pages/_app.tsx에 넣어야한다고 하여 _app.tsx를 생성하여 global.css import를 해당 파일에서 처리함.





## window is not defined 오류 

initializeApp2WebInterface 와 같은 pigeon 초기화 타이밍 이슈때문. 

dom이 생성되지 않은 상태에서 window객체를 찾는 로직들이 존재하였고, 이를 dom 생성 후 호출되도록 useEffect hook 안에 넣음. 



## Analytics not initialized  오류

pigeon 에서 내부적으로 실행하는 초기화가 안됐다고 나옴. 

 이 코멘트 주신대로 hawk 참고하여, Init 로직을 _document.tsx 파일에 넣음. 


// _document.tsx

import Document, { Head, Html, Main, NextScript } from 'next/document';

class MyDocument extends Document {
  render() {
    return (
      <Html lang="ko">
        <Head />
        <script
          // eslint-disable-next-line react/no-danger
          dangerouslySetInnerHTML={{
            __html:
              '!function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on","addSourceMiddleware","addIntegrationMiddleware","setAnonymousId","addDestinationMiddleware"];analytics.factory=function(e){return function(){var t=Array.prototype.slice.call(arguments);t.unshift(e);analytics.push(t);return analytics}};for(var e=0;e<analytics.methods.length;e++){var key=analytics.methods[e];analytics[key]=analytics.factory(key)}analytics.load=function(key,e){var t=document.createElement("script");t.type="text/javascript";t.async=!0;t.src="https://cdn.segment.com/analytics.js/v1/" + key + "/analytics.min.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(t,n);analytics._loadOptions=e};analytics.SNIPPET_VERSION="4.13.2";}}();',
          }}
        />
        <body>
          <Main />
          <div id="bottom-sheet" />
          <div id="toast" />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```


## 몇몇 특정 페이지의 특정 컴포넌트에서 Element type is invalid오류가 발생

file path alias ~문제인줄 알고 삽질 했으나, svgr 문제였음. 

@svgr/webpack설치와 next.config 추가를 통해 해결 

```js
// next.config.js

module.exports = {
  webpack(config) {
    config.module.rules.push({
      test: /\.svg$/,
      use: [{
        loader: "@svgr/webpack",
        options: {
          svgProps: {
            fill: 'currentColor'
          },
        },
      }],
    });

    return config;
  },
}
```

이에 따라 svg icon을 import하는 방식을 변경해야 했음.

```ts
// 기존
import { ReactComponent as Info } from '@koreacreditdata/feather/dist/icons/ic__system__info__filled.svg';

// 변경된 버전
import Info from '@koreacreditdata/feather/dist/icons/ic__system__info__filled.svg';
```




##  craco.config.js 제거 → craco test 사용하는 부분 jest로 변경

### 오류 1.

```
 SyntaxError: /Users/lucy/Dev/merlin/services/core/src/pages/결제/이용권/빠른정산_패키지_결제.test.tsx: Support for the experimental syntax 'jsx' isn't currently enabled (34:18):
```

@babel/preset-react 를 babel.config.js에 preset으로 추가 



### 오류2.

Jest encountered an unexpected token
