### **Angular Interview Topics Explanation**

Here’s an overview of each topic with explanations and details of what they include.

---

### **1. Signals**
**Definition:**  
Signals in Angular provide a new way of managing state and reactivity by tracking dependencies automatically. They are designed to work without requiring `RxJS` and improve the performance of applications by reducing unnecessary computations and change detection calls.

**Includes:**
- `signal()`: Creates a reactive state variable.
- `computed()`: Derives a value from signals.
- `effect()`: Runs a function when dependent signals change.

**Example:**
```ts
import { signal, computed, effect } from '@angular/core';

const count = signal(0);
const doubleCount = computed(() => count() * 2);

effect(() => console.log(`Count changed: ${count()}`));

count.set(5); // This triggers the effect and updates the computed value.
```

---

### **2. Components**
**Definition:**  
A component is the fundamental building block of an Angular application, consisting of a TypeScript class, an HTML template, and CSS styles.

**Includes:**
- **Class (`.ts`)**: Contains logic and data.
- **Template (`.html`)**: Defines the UI.
- **Styles (`.css` or `.scss`)**: Adds styles.
- **Metadata (`@Component`)**: Defines selector, template, styles, and behavior.

**Example:**
```ts
@Component({
  selector: 'app-example',
  template: `<h1>Hello, {{name}}</h1>`,
  styleUrls: ['./example.component.scss']
})
export class ExampleComponent {
  name = 'Angular';
}
```

---

### **3. Templates**
**Definition:**  
Templates define the structure and layout of an Angular component, written in HTML with Angular directives and bindings.

**Includes:**
- **Interpolation (`{{ }}`)**: Inserts dynamic values.
- **Property binding (`[ ]`)**: Binds an attribute to a value.
- **Event binding (`( )`)**: Listens for events.
- **Two-way binding (`[()]`)**: Syncs model and UI.
- **Directives (`*ngFor`, `*ngIf`)**: Adds dynamic behavior.

**Example:**
```html
<h1>{{ title }}</h1>
<input [(ngModel)]="name">
<button (click)="sayHello()">Click me</button>
```

---

### **4. Pipes**
**Definition:**  
Pipes transform data in templates before displaying them.

**Includes:**
- **Built-in Pipes:** `DatePipe`, `CurrencyPipe`, `UpperCasePipe`, `LowerCasePipe`, `JsonPipe`
- **Custom Pipes:** Define custom transformations.

**Example:**
```html
<p>{{ date | date:'short' }}</p>
<p>{{ price | currency:'USD' }}</p>
```

---

### **5. Directives**
**Definition:**  
Directives are classes that add behavior to elements.

**Types:**
- **Structural Directives (`*`)**: Modify the DOM structure (`*ngIf`, `*ngFor`, `*ngSwitch`).
- **Attribute Directives**: Change appearance or behavior (`[ngClass]`, `[ngStyle]`).
- **Custom Directives**: Created by developers.

**Example:**
```ts
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```

Usage:
```html
<p appHighlight>Highlighted text</p>
```

---

### **6. Dependency Injection (DI)**
**Definition:**  
DI is a design pattern in Angular that provides dependencies to classes instead of creating them manually.

**Includes:**
- **Injecting Services** via constructor.
- **Providers (`@Injectable`)**: Makes services available.
- **Hierarchical Dependency Injection**.

**Example:**
```ts
@Injectable({ providedIn: 'root' })
export class LoggerService {
  log(message: string) { console.log(message); }
}
```

Usage:
```ts
constructor(private logger: LoggerService) { }
```

---

### **7. Routing**
**Definition:**  
Routing in Angular enables navigation between different views/pages.

**Includes:**
- `RouterModule`
- **Route definitions** (`path`, `component`).
- **RouterLink** directive.
- **Navigation via `Router` service**.

**Example:**
```ts
const routes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: '**', redirectTo: 'home' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

Usage:
```html
<a routerLink="/home">Go Home</a>
```

---

### **8. Guards**
**Definition:**  
Guards restrict or allow access to routes.

**Types:**
- **CanActivate**: Prevents entering a route.
- **CanDeactivate**: Prevents leaving a route.
- **Resolve**: Fetches data before loading a route.
- **CanLoad**: Restricts lazy loading.

**Example:**
```ts
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    return this.authService.isLoggedIn();
  }
}
```

---

### **9. Interceptors**
**Definition:**  
Interceptors modify HTTP requests and responses.

**Includes:**
- **Logging requests**
- **Adding headers**
- **Handling errors**
- **Authentication tokens**

**Example:**
```ts
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const modifiedReq = req.clone({ headers: req.headers.set('Authorization', 'Bearer token') });
    return next.handle(modifiedReq);
  }
}
```

---

### **10. Forms - Reactive Forms**
**Definition:**  
Reactive Forms allow structured form handling with full control.

**Includes:**
- **FormGroup**: Manages form state.
- **FormControl**: Controls individual input.
- **Validators**: Adds validation.

**Example:**
```ts
this.form = new FormGroup({
  name: new FormControl('', [Validators.required]),
  email: new FormControl('', [Validators.email])
});
```

Usage:
```html
<form [formGroup]="form">
  <input formControlName="name">
</form>
```

---

### **11. HTTP Client**
**Definition:**  
Handles API requests in Angular.

**Includes:**
- **GET, POST, PUT, DELETE** requests.
- **Interceptors**.
- **Error Handling**.

**Example:**
```ts
this.http.get('https://api.example.com/data').subscribe(data => console.log(data));
```

---

### **12. NGModules**
**Definition:**  
Modules group related components, services, and directives.

**Includes:**
- **Root Module (`AppModule`)**.
- **Feature Modules**.
- **Lazy-loaded Modules**.

**Example:**
```ts
@NgModule({
  declarations: [HomeComponent],
  imports: [CommonModule]
})
export class HomeModule { }
```

---

### **13. Component Lifecycle**
**Definition:**  
Hooks allow handling component behavior at different stages.

**Includes:**
- `ngOnInit()`: Runs after component loads.
- `ngOnChanges()`: Runs when input properties change.
- `ngOnDestroy()`: Cleans up when the component is destroyed.

**Example:**
```ts
ngOnInit() { console.log('Component initialized'); }
```

---

### **14. RxJS**
**Definition:**  
RxJS is a library for reactive programming, handling asynchronous operations with Observables.

**Includes:**
- **Observables (`Observable`)**.
- **Operators (`map`, `filter`, `switchMap`)**.
- **Subjects (`BehaviorSubject`, `ReplaySubject`)**.

**Example:**
```ts
this.http.get('api/data').pipe(map(response => response.data)).subscribe(data => console.log(data));
```

---
### **Continued Angular Topics for Your Interview**

---

### **15. Forms - Template-driven**
**Definition:**  
Template-driven forms rely on directives in the template to manage form controls, validation, and submission. They are simpler than reactive forms but offer less control.

**Includes:**
- Uses `NgForm` and `ngModel`
- Two-way data binding (`[(ngModel)]`)
- Validators using attributes (e.g., `required`, `minlength`)

**Example:**
```html
<form #myForm="ngForm" (ngSubmit)="onSubmit(myForm)">
  <input type="text" name="username" [(ngModel)]="username" required>
  <button type="submit" [disabled]="myForm.invalid">Submit</button>
</form>
```
```ts
onSubmit(form: NgForm) {
  console.log(form.value);
}
```

---

### **16. Jest**
**Definition:**  
Jest is a JavaScript testing framework often used for unit testing in Angular.

**Includes:**
- Fast execution using a **test runner**
- Snapshot testing
- Mocking functions and dependencies
- Supports **asynchronous tests**

**Example Test Case (Component Test):**
```ts
import { render, screen } from '@testing-library/angular';
import { MyComponent } from './my.component';

test('renders component', async () => {
  await render(MyComponent);
  expect(screen.getByText('Hello, Angular')).toBeInTheDocument();
});
```

---

### **17. Subjects (RxJS)**
**Definition:**  
A `Subject` is a special type of `Observable` that allows multiple subscribers and can emit values manually.

**Types of Subjects:**
- **Subject**: Emits values to multiple subscribers.
- **BehaviorSubject**: Stores the last emitted value.
- **ReplaySubject**: Replays previous values to new subscribers.
- **AsyncSubject**: Emits only the last value when completed.

**Example:**
```ts
const subject = new BehaviorSubject<number>(0);
subject.subscribe(value => console.log('Subscriber 1:', value));

subject.next(1); // Output: Subscriber 1: 1
```

---

### **18. Observables (RxJS)**
**Definition:**  
Observables are used to handle asynchronous operations such as HTTP requests, user interactions, and event streams.

**Includes:**
- **Creating Observables**
- **Subscribing**
- **Operators (`map`, `filter`, `mergeMap`)**
- **Unsubscribing to prevent memory leaks**

**Example:**
```ts
const observable$ = new Observable(observer => {
  observer.next('Hello');
  observer.complete();
});

observable$.subscribe(value => console.log(value));
```

---

### **19. Change Detection**
**Definition:**  
Change detection in Angular updates the UI when the component state changes.

**Strategies:**
- **Default**: Runs automatically for all components.
- **OnPush**: Runs only when `@Input()` values change.

**Example (OnPush Change Detection):**
```ts
@Component({
  selector: 'app-child',
  template: `<p>{{data}}</p>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() data: string;
}
```

---

### **20. `@Input` and `@Output`**
**Definition:**  
These decorators allow data sharing between parent and child components.

- **`@Input()`**: Passes data **from parent to child**.
- **`@Output()`**: Emits events **from child to parent**.

**Example:**
```ts
// Child Component
@Component({
  selector: 'app-child',
  template: `<button (click)="sendData()">Send</button>`
})
export class ChildComponent {
  @Input() message: string;
  @Output() messageEvent = new EventEmitter<string>();

  sendData() {
    this.messageEvent.emit('Hello from Child');
  }
}

// Parent Component
<app-child [message]="parentMessage" (messageEvent)="receiveMessage($event)"></app-child>
```

---

### **21. Control Flows (`@for`, `@if`, `@switch`)**
**Definition:**  
Control flow syntax simplifies template logic in Angular 17+.

**Includes:**
- `@for` (Replacement for `*ngFor`)
- `@if` (Replacement for `*ngIf`)
- `@switch` (Replacement for `*ngSwitch`)

**Example:**
```html
@for (item of items; track item.id) {
  <p>{{ item.name }}</p>
}

@if (showMessage) {
  <p>Hello, User!</p>
}

@switch (status) {
  @case ('active') { <p>Active</p> }
  @case ('inactive') { <p>Inactive</p> }
  @default { <p>Unknown</p> }
}
```

---

### **22. FormBuilder (Reactive Forms)**
**Definition:**  
`FormBuilder` simplifies the creation of complex reactive forms.

**Includes:**
- **Creates `FormGroup` and `FormControl`**
- **Supports validation**
- **Reduces boilerplate code**

**Example:**
```ts
constructor(private fb: FormBuilder) {}

form = this.fb.group({
  name: ['', Validators.required],
  email: ['', [Validators.required, Validators.email]]
});
```

Usage in template:
```html
<form [formGroup]="form">
  <input formControlName="name">
  <input formControlName="email">
</form>
```

---