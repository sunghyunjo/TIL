# Angular LifeCycle
1. constructor
2. ngOnChanges
3. ngOnInit
4. ngDoCheck
5. ngAfterContentInit
6. ngAfterContentChecked
7. ngAfterViewInit
8. ngAfterViewChecked
9. ngOnDestroy

### Constructor
- Component 혹은 Directive가 생성될 때 호출.
- TS의 초기화 관련된 것은 constructor에서 진행하는 것이 맞지만, Angular에서 사용되는 속성의 초기화는 ngOnInit에서 하는 것이 좋다.

### ngOnChanges
- 부모 Component가 자식 Component에게 데이터가 바인딩되거나 변경되었을 때 호출된다. (@Input을 사용하지 않으면 호출되지 않음.)
- 정확히 말하면, 전달하는 primitive 값이 변경되거나 혹은 참조하는 객체의 reference가 변경되어야 호출된다. 즉, 참조하는 객체의 property가 변경되는 경우에는 호출되지 않는다!
- @Input값의 바인딩은 생성자의 호출 이후 일어난다. 그러므로 생성자에서 값을 출력하면 undefined가 출력된다. 
- 간략히 요약하자면, @Input으로 전달되는 값이 변경될 때마다 호출되는 라이프사이클이다.

### ngOnInit
- ngOnChanges가 호출된 이후 모든 속성에 대한 초기화가 완료된 시점에 한번만 호출된다.
- 즉, class가 가지고 있는 속성과 @Input을 통해 값을 내려받은 속성이 모두 초기화된 후 호출된다.

### ngDoCheck
- 컴포넌트에서 발생하는 모든 상태변화에 반응하여 호출된다. Angular의 Change Detection이 상태변화를 감지하면 자동으로 호출된다.
- ngOnChanges와는 다르게 primitve, reference객체, reference객체 속성의 변경 등 모든 변경에 대해 호출된다. 이전이랑 같은 값이 assign되어도 호출된다.
- 즉, ngDoCheck를 너무 많이 사용하면 성능이 저하될 수 있다.

### ngAfterContentInit, ngAfterContentChecked
- ngDoCheck 이 호출된 후 한번만 호출된다. 
- ngAfterContentInit: 외부 콘텐츠가 컴포넌트 뷰로 들어갔을 때 호출

### ngAfterViewInit, ngAfterViewChecked
- 컴포넌트에 속한 모든 view와 viewChild가 시작된 후 호출된다. 
- 쉽게 말하자면, HTML이 모두 화면에 출력된 후 호출된다.
- ngAfterViewChecked는 컴포넌트의 view에 대한 Change Detection이 실행된 후 호출된다.

### ngOnDestroy
- component가 소멸하기 직전에 호출된다. 일반적으로 사용된 자원에 대한 해제 코드가 들어옴.
