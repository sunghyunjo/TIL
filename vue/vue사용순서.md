# 새 페이지를 추가하려면
1. 새 페이지를 위한 컴포넌트를 만듭니다.
2. routes.js에 라우트를 정의해 줍니다.
3. 새로 만든 페이지로 이동할 수 있도록 router-link를 만듭니다.

# 새 모듈을 추가하려면 (Vuex)
1. mutation-types.js 에 어떤 변이가 필요한지 추가하세요.
2. modules 디렉터리에 사용할 모듈 파일을 만들고, state와 mutations을 추가하세요.
3. actions.js에 mutation을 호출할 메소드를 만드세요.
4. getter.js에 컴포넌트에서 사용할 상태를 추가하세요.
5. 컴포넌트에서 computed에 상태를, methods에 액션을 호출할 dispatch를 추가하세요.
