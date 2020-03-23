# 문제 상황

model 에 선언된 EN_SUB_HOME_DISPLAY_TARGET_TYPE enum이 deus에서 undefined로 나타남.

    // 기존 코드
    export declare enum EN_SUB_HOME_DISPLAY_TARGET_TYPE {
    ...
    }  .

# 해결

declare 제거 후 정상 작동

    export enum EN_SUB_HOME_DISPLAY_TARGET_TYPE {
    ...
    }  .

# 근데 왜 declare enum이 뭐가 문제일까?

일단 먼저 enum에 대해 좀 더 깊게 이해해보자면, 

### enum

    enum Foo { A = 4}
    var x = Foo.A; // 트랜스파일 후, 이건 Foo라는 객체가 생긴다.

    1. enum // Object 를 만들어줌.
    2. declare enum // 전역 어딘가에 선언되어 있는 객체를 enum처럼 다룰 수 있게 해줌.
    3. const enum 
    // 그런데 const enum 형태는 TS에서만 enum처럼 다뤄지고, 
    // 트랜스파일 후에는 객체가 아닌 각각 값으로 들어감.

### declare enum

- 전역 어딘가에 선언되어 있는 객체를 enum처럼 다룰 수 있게 해줌.
- 그런데 const enum 형태는 TS에서만 enum처럼 다뤄지고, 컴파일 후에는 객체가 아닌 각각 inline 값으로 들어간다.

예를 통해 살펴보자면,

    declare enum Foo {
    ...
    } 
    
    // const enum 으로 뽑힌 형태
    const enum Foo { A = 4 }
    var x = Foo.A; // emitted as "var x = 4;", always
    
    /**
    	트랜스파일 후, 4를 inline으로 넣어준다. Foo라는 객체가 생기는 것이 아니고...
    */

트랜스파일 된 파일에서는 declare 를 통해 enum을 선언했기 때문에, 런타임에는 존재하지 않는 값이 되어버린 것이다.
