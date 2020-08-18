> 2020 오픈소스 컨트리뷰톤에 참여하며 ESLint에 기여하였고, 해당 경험에 대한 후기를 간략하게 작성한 글입니다.

# [eslint/eslint#12579](https://github.com/eslint/eslint/issues/12579) 해결 후 정리
## 간단한 이슈 요약 및 해결 방식
### 이슈 요약
- 기존 규칙 중, [no-underscore-dangle](https://eslint.org/docs/rules/no-underscore-dangle#disallow-dangling-underscores-in-identifiers-no-underscore-dangle)에서 함수 파라미터의 이름에 대해 체크 하지 않는 것을 체크 하는 형태로 업데이트하는 것.

### 해결 방식
- `allowFunctionParams` 옵션 생성 및 `checkForDanglingUnderscoreInFunctionParameters` 생성
#### `allowFunctionParams`을 위해 작성한 주요 함수 로직
```js
function checkForDanglingUnderscoreInFunctionParameters(node) {
   if (!allowFunctionParams) {
        node.params.forEach(param => {
            const { type } = param;
            let nodeToCheck;

            if (type === "RestElement") {
                nodeToCheck = param.argument;
            } else if (type === "AssignmentPattern") {
                nodeToCheck = param.left;
            } else {
                nodeToCheck = param;
            }

            if (nodeToCheck.type === "Identifier") {
                const identifier = nodeToCheck.name;

                if (hasDanglingUnderscore(identifier) && !isAllowed(identifier)) {
                    context.report({
                        node: param,
                        messageId: "unexpectedUnderscore",
                        data: {
                            identifier
                        }
                    });
                }
            }
        });
    }
}
```
- `Function Delaration`, `Function Expression`, `Arrow Function Expression`형태일 때, 모두 위의 로직을 pass할 수 있도록 변경 및 추가

## 새로 알게 된 내용
- 일단, 가장 먼저 기존에 알고 있던 지식을 심화시킬 수 있었다는 점은 가장 큰 수확이지 않을까(?)라고 생각한다.
>`javascript를 AST형태로 분석하고 그 Tree를 순회하여 구문 분석이 일어난다.` 라는 일차원적인 내용만 알고 있었지만, 그 내부를 자세히 들여다 볼 생각을 하진 않았었다.

### 1. 생소했던 AST의 자세한 명칭 및 타입
- 기존에 구문 분석에 대한 키워드나 동작 방식을 이론으로나마 알고 있었기에, Explorer로 풀어진 형태가 마냥 어렵진 않았었다. 
그렇지만, 한글로만 대충 명칭을 알고 있었거나 정확한 영문 명칭을 기억하지 못한 채  PR리뷰에서 언급되는 내용들은 직접 Explorer에 코드를 짠 후 json을 하나하나 펼쳐보며 알게 되었다. ex) `RestElement`나,`AssignmentPattern`..
> 만약 다른 분들께서도 `이런 형태에 대한 케이스도 판단해야 할 것 같은데, 이런 것도 AST가 판단하려나...?` 싶은 생각이 든다면, 바로 Explorer로 AST를 만들어보길 권유한다. 왜냐면..이미 있을 거거든요...ㅎㅎ

### 2. Test Case 작성
- 내가 주로 회사에서 하는 업무는 일반적인 프론트개발자의 업무로 잘 알려진 UI를 개발하는 형태이다. 더 나아간다면, data model이나 interface를 짜는 정도이다. 때문에, TC를 한 두번 공부할 때 말고는 짜본 경험이 없었기에 `어떡하지?`라는 생각으로 먼저 기존 TC 코드들을 싹 훑어봤었다.
그런데 생각보다 eslint의 TC는 어려운 형태가 아니었다. 나같은 경우엔 새로운 코드를 보았을 때 가장 core단까지 `Implementations`을 찾아가보는 방식으로 코드를 분석하는 편인데, 역시나 eslint같은 엄청나신 분들이 만든 오픈소스에는 각 함수에 대한 doc이나 comment가 너무 친절했다. 👍 
- 기존 코드를 보는데 많이 어려운 것 같다는 생각이 드시는 분들이 있으시다면, 차근차근 Implementations을 찾아가보며 공부해보는 방식을 추천한다.

## 소감
- `eslint`메인테이너 분들은 정말 자세히 리뷰해주신다는 걸 느꼈다. 하나하나 모든 케이스에 대해 생각하시며 리뷰해주셨고, 이렇기 때문에 많은 사람들이 쓰는 eslint가 매 릴리즈 때마다 큰 영향없이 동작하는 것이구나 라는 걸 몸소 느낄 수 있었다.
- 리뷰가 계속 길어질 때, typescript plugin을 작업하신 분들의 리뷰가 성공적으로 merge되는 것을 보며,, 다음 이슈는 다른 걸 찾아볼까 했었다. 😢 아주 단편적인 생각이었다 ㅎㅎㅎ 그렇지만, 꼼꼼한 리뷰를 받은 덕택에 eslint에 기여한 나의 코드 줄 수도 증가하였고, 회사에서는 배우지 못했던 아주아주 꼼꼼한 Edge case에 대해 고려하는 방법 등을 익혔다. 더불어 내가 좀 더 꼼꼼한 개발자가 되어야겠다는 생각도 함께 하였음은 물론이다 😄 
- 그래서 일단 지금의 결심은 이번에도 `eslint에서 이슈를 찾아보자!` 이다. 한 번 익숙해진 만큼, 다음 이슈에 대해 진입하는 것도 지난번보다는 쉬워졌으리라 믿는다.
- 주안님이 초반에 알려주신 내용이 굉장히 디테일했기에, 빠르게 이슈를 해결했다는 느낌이 아주 많이 든다...(증맬루..) 거의 사수님이 숟가락 떠먹여주신 정도라고 봐도 무방하다 🙏 다음 이슈는 이렇게 빨리 파악해서 할 수 있을진 모르지만,, 좀 더 이슈를 많이 해결하다보면 척척 `아~ 이거?` 라는 느낌이 언젠가..오지 않을까..라고 다짐해본ㄷ..ㅏ.. 다시 한번 감사드립니다 👍 
