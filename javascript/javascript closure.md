# Closure (클로저)
자바스크립트의 [Scope Chain](https://github.com/sunghyunjo/TIL/blob/master/javascript/ScopeChain.md)을 이용하여 이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 방법이다. 외부함수가 종료되더라도 내부함수가 실행되는 상태라면, 내부함수에서 참조하는 외부함수는 닫히지 못하고, 내부함수에 의해 닫히게 되어 Closure(클로저)라고 불린다.
> 따라서, 클로저란 외부에서 내부 변수에 접근할 수 있도록 하는 함수이다.