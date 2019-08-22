# NgModule이란?
- @NgModule 데코레이터가 지정된 클래스.
- Angular에서 제공하는 대표적인 라이프러리 : FormsModule, HttpClientModule, RouterModule..
- 컴포넌트와 디렉티브, 파이프 등 기능이 연관된 구성요소를 하나로 묶어 관리하는 단위.

```ts
// imports
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { ItemDirective } from './item.directive';


// @NgModule 데코레이터에 메타데이터를 지정합니다.
@NgModule({
  declarations: [
    AppComponent,
    ItemDirective
  ],
  imports: [ // 이부분에 NgModule을 로드. 이 부분에 이 모듈에서 사용하는 외부 모듈을 여기에 등록한다.
    BrowserModule,
    FormsModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- imports 배열에는 JavaScript 모듈이 아니라, Angular 모듈이 들어가야 한다.


# 자주 사용하는 NgModule
- BrowserModule	@angular/platform-browser:브라우저에서 애플리케이션을 실행할 때
- CommonModule	@angular/common:	NgIf, NgFor를 사용할 때
- FormsModule	@angular/forms:	템플릿 기반 폼을 사용할 때 (NgModel 포함)
- ReactiveFormsModule	@angular/forms:	반응형 폼을 사용할 때
- RouterModule:	@angular/router	RouterLink, .forRoot(), .forChild()를 사용할 때
- HttpClientModule:	@angular/common/http	서버와 HTTP 통신을 해야 할 때
- BrowserModule은 최상위 앱 모듈에서 딱 한 번만 로드해야 한다.

# Providers
의존성 주입에 사용되는 객체(일반적으로 서비스)를 가져오는 방법을 지정한 것. 

```ts
import { Injectable } from '@angular/core';


@Injectable({
	provideIn: 'root', // 이 서비스가 최상위 인젝터에 위치하도록 지정하는 코드.
})


export class UserService {
}
``` 

- @Injectable 데코레이터 안에는 providedIn 프로퍼티가 있는데, 이 프로퍼티를 지정하면 서비스 프로파이더 범위를 지정할 수 있음. 
- 애플리케이션의 최상위 인젝터에 서비스 프로파이더를 등록하면, 이 서비스는 앱 전역에서 자유롭게 사용할 수 있음.
- 서비스 프로파이더는 특정 @NgModule에 포함되지 않는 이상, 최상위 인젝터에 등록하는 것이 좋다.
