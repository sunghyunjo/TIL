## directive 
- `*` 로 시작하는 디렉티브들은 모두 structual directive
- `*ngFor` : 반복문
- `*ngIf` : 조건문 (이 값이 true가 될 때 html이 렌더링 된다)

## syntax
- `{{ }}` : 문자열 바인딩 
- `[ ]` : property 바인딩 ex. <a [title]="product.name + ' details'">
- `( )` : event 바인딩 


## decorator 
### @Component 
```ts
@Component({
  selector: 'app-product-alerts', // 컴포넌트를 구분하는 id, 일반적으로 Angular 컴포넌트 셀렉터는 app- 접두사를 사용한다.
  templateUrl: './product-alerts.component.html',	// 컴포넌트 생성할 때 자동으로 생성
  styleUrls: ['./product-alerts.component.css'] 	// 컴포넌트 생성할 때 자동으로 생성
})
```
- decorator아래에 있는 클래스가 컴포넌트라는 것을 지정하는 데코레이터. 
- template, style, selector와 같이 컴포넌트와 관련된 메타데이터를 인자로 받음.
- ProductAlertsComponent : 컴포넌트의 동작을 처리하는 클래스

### @Input
```ts
export class ProductAlertsComponent implements OnInit {
	@Input() product;
	constructor() { }
	ngOnInit() { }
}
```
- product 프로퍼티에 할당되는 값이 부모 컴포넌트에서 전달된다는 것을 의미한다.

### @Output
```ts
export class ProductAlertsComponent {
	@Input() product;
	@Output() notify = new EventEmitter();
}
```
- 이 컴포넌트 클래스에서 밖으로 이벤트를 보낼 notify 프로퍼티를 정의하고, 이 프로퍼티에 @output 데코레이터를 지정한다. 
- @Output데코레이터를 지정하면 다른 컴포넌트에서 이벤트를 보낼 수 있다.

