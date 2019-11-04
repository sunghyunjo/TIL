# 필요성
- As-is : `app.module.ts`에 각 module들을 import 한 후, `vendor.js`로 번들링
- To-be : `app.module.ts`로 각 module을 연결하되, 특정 routing에서 필요한 라이브러리들만 번들링되어 뽑아낼 수 있도록 변경하고자 함.

# 코드 비교
### AS-IS
```ts
// app.module.ts
import { AuthenticationModule } from './authentication/authentication.module'
import { BrowserModule } from '@angular/platform-browser'
import { NgModule } from '@angular/core'

import { AppComponent } from './app.component'

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AuthenticationModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}

;<app-login />
```


### TO-BE
```ts
// app.routing.ts
import { ContentComponent } from './content/content.component'
import { Routes, RouterModule } from '@angular/router'
import { ModuleWithProviders } from '@angular/core'

export const routes: Routes = [
  { path: '', pathMatch: 'full', redirectTo: 'content' },
  { path: 'content', component: ContentComponent },
  {
    path: 'login',
    loadChildren: './authentication/authentication.module#AuthenticationModule',
  },
]

export const routing: ModuleWithProviders = RouterModule.forRoot(routes)
```

```ts
// app.module.ts
import { routing } from './app.routing'
import { BrowserModule } from '@angular/platform-browser'
import { NgModule } from '@angular/core'

import { AppComponent } from './app.component'
import { ContentComponent } from './content/content.component'

@NgModule({
  declarations: [AppComponent, ContentComponent],
  imports: [
    BrowserModule,
    routing, //import routing
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

router-outlet 추가해주기
```html
<router-outlet></router-outlet>
```

- 즉, routing 을 app.moule.ts에 추가해줌으로 각 routing에 해당하는 module들이 lazy-loading될 수 있어진다.

[자세한 글내용 / 출처](https://malcoded.com/posts/angular-fundamentals-modules/)
