> 2020 오픈소스 컨트리뷰톤에 참여하며 eslint-plugin-react에 기여하였고, 해당 경험에 대한 후기를 간략하게 작성한 글입니다.

### 목차
- [이슈 선정 과정](https://github.com/sunghyunjo/TIL/new/master/etc#%EC%9D%B4%EC%8A%88-%EC%84%A0%EC%A0%95-%EA%B3%BC%EC%A0%95)
- [선정한 이슈 내용](https://github.com/sunghyunjo/TIL/new/master/etc#%EC%84%A0%EC%A0%95%ED%95%9C-%EC%9D%B4%EC%8A%88-%EB%82%B4%EC%9A%A9)
- [해결 방법 도출 과정](https://github.com/sunghyunjo/TIL/new/master/etc#%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%EB%8F%84%EC%B6%9C-%EA%B3%BC%EC%A0%95)
- [어려웠던 점](https://github.com/sunghyunjo/TIL/new/master/etc#%EC%96%B4%EB%A0%A4%EC%9B%A0%EB%8D%98-%EC%A0%90)
- [소감](https://github.com/sunghyunjo/TIL/new/master/etc#%EC%86%8C%EA%B0%90)

---

# 이슈 선정 과정
가장 첫 컨트리뷰션을 `ESLint`로 진행했기에 `ESLint`이슈를 찾아보았지만, 

보다 엄격한 기준을 갖고 있기 때문인지 `accepted`된 이슈들 중 내가 해결할 만한 이슈가 딱히 보이지 않았다. :(

자의 반, 타의 반 ..ㅎ 그렇게 `eslint-plugin`들을 훑어보던 중, 전에 ESLint에서 해결했던 parameter 관련 이슈가 `eslint-plugin-react`에 올라와 있는 것을 보았다.

기존에 AST Explorer를 통해 param 관련 tree는 어떻게 형성되어 지는지 조금이나마 익숙했기에, 어느정도 머릿 속에 해결방법이 그려지는 듯하여 해결할 이슈로 선정하였다.

# 선정한 이슈 내용
[eslint-plugin-react/issues/2762](https://github.com/yannickcr/eslint-plugin-react/issues/2762)
### AS-IS
```ts
function Foo({ bar = "" }): JSX.Element {
  return <div>{bar}</div>;
}
```

```ts
function Foo({ bar = "" as string }): JSX.Element {
  return <div>{bar}</div>;
}
```

위의 두 예제 코드 모두 타입이 정의 되었음에도,  `react / prop-types` 오류가 발생한다.
>  'bar'is missing in props validation

현재는 destructured된 prop은 아래와 같은 방식의 코드만 오류를 뱉어내지 않는다.
```ts
function Foo({ bar = ""}: { bar: string }): JSX.Element {
  return <div>{bar}</div>;
}
```

### TO-BE
아래 세가지 코드 모두에 대해 `react/prop-types` 오류가 발생하지 않아야 한다.
```ts
function Foo({ bar = "" }): JSX.Element {
  return <div>{bar}</div>;
}
```

```ts
function Foo({ bar = "" as string }): JSX.Element {
  return <div>{bar}</div>;
}
```

```ts
function Foo({ bar = ""}: { bar: string }): JSX.Element {
  return <div>{bar}</div>;
}
```
## 이슈와 관련된 규칙
[prop-types](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prop-types.md)

### 규칙에 대한 설명
간단하게 해당 규칙에 대해 요약하자면, React + Typescript 를 사용할 때 아래의 코드와 같이 `name` 처럼 prop으로 전달 받은 데이터에 대한 타입을 명시해줘야 한다는 규칙이다.
```ts
interface Props = {
  age: number
}
function Hello({ name }: Props) {
  return <div>Hello {name}</div>;
  // 'name' type is missing in props validation
}
```



# 해결 방법 도출 과정
## 1. Test Case 작성
가장 먼저 이슈 내용 중 불필요한 오류를 뱉어내는 해당 코드들을 Test case로 추가하였고 이슈와 같은 상황을 재현하였다.

역시나 이슈 내용과 동일한 에러들이 발생하고 있는 것을 확인하였다.

## 2. [AST Explorer](https://astexplorer.net/)로 AST탐색해보기
- 이 과정보다 3번의 디버깅 과정을 먼저 해보는 것도 좋을 수 있다. 다만, 나에겐 디버깅이 아직 익숙한 상황은 아니었어서 좀 더 내가 원하는 부분에 대한 tree를 쉽게 볼 수 있는 AST Explorer로 전체 구조를 파악하였다.
- 아래의 AST Explorer를 보고 적은 1,2,3의 내용들은 이슈를 해결하기 전 (OSS 컨트리뷰션에서 멘토님과 논의 하기 전 내용) 적은 내용들이기 때문에, 좀 더 보완할 부분이 보이는 분석이었다.

### 2-1. 현재 `eslint-plugin-react`에서 오류가 나지 않는 첫번째 케이스
```ts
function Foo({ bar = ""}: { bar: string }): JSX.Element {
  return <div>{bar}</div>;
}
```

`typeAnnotation`이 `properties`와 같은 레벨에 아래의 이미지와 같이 tree가 구성되어 있었다.
![image](https://user-images.githubusercontent.com/20012817/90951191-84f20380-e493-11ea-83db-b62ebd74387e.png)

### 2-2. 첫번째 오류 케이스 
```ts
function Foo({ bar = "" }): JSX.Element {
  return <div>{bar}</div>;
}
```
`properties`와 같은 레벨에 `typeAnnotation`이 나타나지 않았다.
> 결과적으론, typeAnnotation보다 properties의 타입이 AssignmentPattern일 때 오류가 발생하지 않도록 판단하였다.
이 케이스를 커버하는 로직을 작성하려면 `Literal`type `right`의 value에`"..."` 처럼 문자열이 들어가면 type으로 판단하도록 해야하지 않을까..라고 생각했다.
![image](https://user-images.githubusercontent.com/20012817/90951212-bec30a00-e493-11ea-889a-0aeae470524f.png)


### 2-3. 두번째 오류 케이스
```ts
function Foo({ bar = "" as string }): JSX.Element {
  return <div>{bar}</div>;
}
```
위의 2-2번과 다르게 `right`에 `typeAnnotation`은 정의되어 있지만,
2-1번의 오류를 뱉지 않는 케이스와는 다르게 (하지만, 2번의 케이스와는 같게) `properties`와 같은 레벨에 `typeAnnotation`은 나타나지 않았다.
> 이 또한, typeAnnotation보다 properties의 타입이 AssignmentPattern일 때 오류가 발생하지 않도록 판단하여 해결 가능했다.

![image](https://user-images.githubusercontent.com/20012817/90951277-5d4f6b00-e494-11ea-99bc-ef5cb1943bcb.png)

> ## 위의 AST 분석을 정리하자면...
위의 내용처럼 각각의 케이스에 대해 분석해보았고, 멘토님의 조언을 얻어 좀 더 간단한 해결 방안을 도출할 수 있었다.
- `destructuring` 형태의 param이 보이면, properties의 type이 AssignmentPattern일 때 오류를 발생시키지 않는다.
- AssignmentPattern임에도 `{foo = bar}`와 같은 `Literal`형태가 아닌 값이 정의될 수 있으므로, right가 `Literal`형태가 아닐때 오류를 발생시키지 않는다.
크게 이렇게 두 가지의 방식을 토대로 진행하였다.


## 3. 디버깅 
해결 방법을 어느정도 생각해본 뒤, 실제로 코드를 작성하기 시작하였고 중간 중간 node가 어떤 흐름으로 어떤 값이 들어가 있는 지를 판단하기 위해 디버깅을 했다.

지난 번에 해결했던 ESLint 이슈에서는 디버깅을 거의 사용하지 않았어서 조금 익숙하지 않았지만, 
> 사실 지난 번엔 어디 부분을 어떻게 해결해야 하는지 멘토님이 거의 알려주신 상태라 디버깅이 없어도 되지 않았을까..ㅎ

점점 사용하다보니 console 지옥을 경험하지 않아도 됨에 감탄했다. ^0^

## 4. 해결!
기존 로직에서 prop이 declare되지 않았다는 판단이 된 node들을 아래의 `isDeclaredInDestructuredParam`함수를 한번 더 통과하도록 만들었다.
```js
/**
 * Checks if the prop is declared in destructured params
 * @param {Object[]} params List of destructured param among props without declaredPropTypes
 * @returns {Boolean} True if the prop is declared, false if not.
 */
 function isDeclaredInDestructuredParam(params) {
    let result = true;
    params.forEach((param) => {
      if (!param.properties) {
        result = false;
        return;
      }
      param.properties.forEach((property) => {
        const type = property.value.type;
        const right = property.value.right;
        if (type !== 'AssignmentPattern') {
          result = false;
          return;
        }
        if (type === 'AssignmentPattern' && right && right.expression && right.expression.type && right.expression.type !== 'Literal') {
          result = false;
        }
      });
    });

    return result;
 }
```

# 어려웠던 점
### 기존 Test Case들과의 충돌
기존에 있던 test case가 약 350개가 넘었던 걸로 기억한다. ㅠㅠ 이전에 해결했던 ESLint 룰은 test case가 이렇게 많지는 않았던 터라 적잖이 당황스러웠다.

모든 테스트를 함께 돌리니 새로 추가한 케이스에 대해 찾아내는 것이 다소 번거로워졌고, 지난 번에 멘토님이 알려주신 `다른 테스트 케이스 모두 주석처리하기`방법을 사용해보았다.
내가 추가한 주석만 확인해보니 다행히 어느정도 수월해졌다. 

하지만, 내 나름대로 완성했다고 생각한 뒤 기존 테스트 케이스의 주석을 해제하자 기존 코드에서 에러가 발생했다. 
기존 테스트 케이스에서 오류가 발생한다는 것은 내가 코드를 무언가 잘못 작성했다는 힌트이기 때문에, 이때는 원초적이지만 하나하나 해당 코드 외의 코드는 모두 주석처리한 뒤 디버깅하여 답을 찾아냈다.

### 엣지 케이스에 대한 커버
PR을 올리고 난 후 받은 첫 리뷰는 `{foo, bar = ''}`와 같이 param이 destructuring 된 형태 내에서 여러개의 property가 나타날때도 추가해보아라. 라는 것이었다.

지난 ESLint에서 받았던 리뷰 내용 중, `param은 여러개로 구성되기 때문에 forEach를 사용해야 한다`는 것을 떠올리고 `foo, {bar = ''}`와 같이 여러 개의 param이 올 때에 대해서는 잘 대응했지만, 
destructuring 변수 안에서에 대해서는 고민하지 못했던 것이다.

이 밖에도 PR을 올리기 전까지 내가 커버하지 못했던 엣지 케이스들을 멘토님이 추가로 알려주셔서 커버하기도 했었다. 
이렇듯 모든 케이스에 대해 커버할 수 있는 로직을 짜기 위해선, 내가 그 케이스들을 그려낼 수 있는 능력이 먼저 필요하다고 생각했다. 

이런 부분은 오픈소스와 같이 더 큰 생태계에 대한 경험부족과, 회사에서 UI개발 업무 위주에 내 사고가 수동적으로 변한게 또 하나의 원인이지 않을까라는 반성도 했던 것 같다 ㅜㅜ
> 보통 회사에서 UI 개발을 진행할때는 기획 단계에서 개발에서 처리해야 할 모든 엣지케이스를 명시해준다. 또한 기획과 개발에서 잡지 못했던 케이스는 QA단계에서 한번 더 걸러준다.

# 소감

이번 이슈는 merge되었지만 살짝 찜찜하다..ㅎㅎ 

리뷰에 대해 고민하고 있던 찰나에 리뷰어분께서 조금 답답하셨는지 force-push 후 merge하셨기 때문이다,, 내가 빠릿빠릿하게 답변을 달고 해결하지 못한 탓인 것 같다.

지난 번 첫 컨트리뷰션보다는 회사 업무 일정이 갑자기 바빠져서 투자할 수 있던 여유시간이 좀 더 적었고, 코로나 이슈로 인해 온라인으로만 진행되다 보니 나도 마음이 조금 해이해졌던 것 같다.

아 그리고,, 확실한건 `eslint-plugin-react`의 리뷰는 엄청 빡빡하진 않은 거 같다는 점이다. 

merge가 생각보다 빨리 되어서 장점은 있지만, 나 같은 경우엔 빡센 리뷰를 받았을 때 내 코드에 대한 확신과 단단함이 증가한다고 느끼는 편이라 얼른 다시 `ESLint`에서 해결할 이슈가 생겼으면 좋겠다.
