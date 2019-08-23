# Observable
1. **observable은 lazy하다.**

    각 observable은 subscribe를 하는 요소에만 보내고, 다른 요소엔 보내지 않는다.

2. **observables는 시간이 지남에 따라 여러 값을 가질 수 있다.**

    subscribe를 계속 열어두면, 매번 새로운 observable을 받게 된다. 

## Promise vs observable

### promise

- 오직 하나의 값만을 반환한다.
- subscribe를 취소할 수 없다. promise가 건네지면, 그 promise의 resolve가 이미 진행되고 있으며, 일반적으로 promise의 resolve를 막을 수 있는 권한이 없다.

### observable

- subscribe를 취소할 수 있다.
- 여러개의 값을 가질 수 있다.

## 방식 1

### Async Pipe

- Angular는 컴포넌트의 생명주기 동안 구독을 처리한다.
- 자동으로 구독하고 구독취소한다.
- 비동기 파이프가 노출되어야 하므로 모듈에 "CommonModule"을 import해야 한다.

**observable 변수이름에 $를 사용하면 모범 사례! → 변수가 관찰 가능 여부를 쉽게 식별할 수 있다.**

```ts
    import { Component } from "@angular/core"
    import { Observable } from "rxjs/Rx"
    
    // client
    import { HttpClient } from "../services/client"
    
    // interface
    import { IUser } from "../services/interfaces"
    
    @Component({
        selector: "user-list",
        templateUrl:  "./template.html",
    })
    export class UserList {
    
        public users$: Observable<IUser[]>
    
        constructor(
            public client: HttpClient,
        ) {}
    
        // do a call to fetch the users on init of component
        // the fetchUsers method returns an observable
        // which we assign to the users$ property of our class
        public ngOnInit() {
            this.users$ = this.client.fetchUsers()
        }
    }

    <!-- We use the async pipe to automatically subscribe/unsubscribe to our observable -->
    <ul class="user__list" *ngIf="(users$ | async).length">
        <li class="user" *ngFor="let user of users$ | async">
            {{ user.name }} - {{ user.birth_date }}
        </li>
    </ul>
```

## 방식 2

### subscribe()

- 데이터를 표시하기 전에 먼저 데이터에 뭔가 작업하기를 원한다면 편리할 수 있다.
- 단점은, 구독을 직접 관리해야 한다.
    - 구독을 사용하지 않는 동안 열어두면 메모리 누수가 발생한다.
    - 사용하지 않을때는, unsubscribe()를 호출하여 구독을 취소한다.

