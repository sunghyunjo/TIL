# 언제 unsubscribe를 해야 할까?
## unsubscribe를 해야하는 이유

- 기본적으로 Observable은 subscribe했을때 실행한다. 그 이후 들어오는 stream(data)은 subscribe메서드 안에서 처리된다.
    - 때문에 unsubscribe를 안하는 경우 계속 관찰하여 메모리낭비 or 로직이 불확실해질 수 있다.

## unsubscribe를 해야하는가?

일반적으로 Angular에서 unsubscribe 하는 위치는 ngOnDestroy LifeCycle 시점이다.

해당 시기는 component나 directive모두 동일하다. 

### 가장 베스트 방법

subscribe를 쓰지 않고, Angular내부적으로 subscribe와 unsubscribe처리를 모두 해주는 Async Pipe를 사용하는 것!

## unsubscribe방법

### 1. take method

```ts
    this.productions$
    	.take(1)
    	.subscribe(products => {
    	// ..code	
    });
```

위처럼 take(1) 코드는 한번 실행 후 자동으로 구독이 취소되는 방식이다. 

ajax호출같은 스트림을 처리할 때 유용하다.

### 2. takeWhile

```ts
    alive = true;
    
    ngOnInit() {
    	this.filtereChange$
    		.takeWhile(() => this.alive)
    		.subscribe(filter => {
    			// filter가 변경되었을 때의 처리
    		});
    }
    
    ngOnDestroy() {
    	this.alive = false;
    }
```

subscribe 전에 takeWhile 연산자를 통해 this.alive가 true일때만 구독하도록 하고,

ngOnDestory LifeCycle에서 this.alive값을 false로 변경함으로써 구독을 취소하는 방식이다.

위의 방법을 사용하면 구독 시 매번 Subscription객체를 선언할 필요가 없다.

### 3. takeUntil

```ts
    unsubscribe: Subject = new Subject(void);
    
    public ngOnInit() {
    	this.tagService.getTags({ category: 'keyword' })
    		.takeUntil(this.unsubscribe)
    		.subscribe(tags => {
    			this.tags = tags;
    		});
    }
    
    public ngOnDestroy() {
    	this.unsubscribe.next();
    	this.unsubscribe.complete();
    }
```

takeUntil은 연산자에 Observable을 넘겨 미러링을 한 후, 넘겨준 observable이 데이터를 받거나 완료 처리가 되면 미러링을 중단하고 처음 Observable은 구독취소된다.
