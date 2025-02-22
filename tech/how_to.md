### **1. Pipe Example (Custom Pipe)**  
A pipe transforms data in templates. Below is a **custom pipe** that converts a string to uppercase but adds an exclamation mark.

```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'shout'
})
export class ShoutPipe implements PipeTransform {
  transform(value: string): string {
    return value.toUpperCase() + '!';
  }
}
```

**Usage in Template:**  
```html
<p>{{ 'hello world' | shout }}</p>
```
**Output:**  
```html
HELLO WORLD!
```

---

### **2. Directive Example (Highlight Directive)**  
A directive can change the appearance or behavior of an element.

```ts
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.el.nativeElement.style.backgroundColor = 'transparent';
  }
}
```

**Usage in Template:**  
```html
<p appHighlight>Hover over me</p>
```

---

### **3. Injectable Example (Logging Service)**  
A service using `@Injectable` to be used across the application.

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class LoggerService {
  log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}
```

**Usage in a Component:**  
```ts
constructor(private logger: LoggerService) {}

ngOnInit() {
  this.logger.log('Component initialized');
}
```

---

### **4. Guard Example (Auth Guard for Route Protection)**  
A guard restricts access to routes based on authentication.

```ts
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}
```

**Usage in Routing Module:**  
```ts
const routes: Routes = [
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
  { path: 'login', component: LoginComponent }
];
```

---

### **5. Interceptor Example (Adding Authorization Header to Requests)**  
An HTTP interceptor modifies requests and responses.

```ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const authReq = req.clone({
      setHeaders: { Authorization: 'Bearer YOUR_TOKEN' }
    });
    return next.handle(authReq);
  }
}
```

**Usage in App Module:**  
```ts
providers: [
  { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
]
```

---

### **6. Reactive Form Example (With Validators & Form Array)**  
A reactive form with validation and a dynamic array of items.

```ts
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, FormArray, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  templateUrl: './reactive-form.component.html'
})
export class ReactiveFormComponent {
  form: FormGroup;

  constructor(private fb: FormBuilder) {
    this.form = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      items: this.fb.array([])
    });
  }

  get items() {
    return this.form.get('items') as FormArray;
  }

  addItem() {
    this.items.push(this.fb.control('', Validators.required));
  }

  submit() {
    console.log(this.form.value);
  }
}
```

**Usage in Template:**  
```html
<form [formGroup]="form" (ngSubmit)="submit()">
  <input formControlName="name" placeholder="Name">
  <input formControlName="email" placeholder="Email">
  
  <div formArrayName="items">
    <div *ngFor="let item of items.controls; let i = index">
      <input [formControlName]="i" placeholder="Item {{i + 1}}">
    </div>
  </div>

  <button (click)="addItem()">Add Item</button>
  <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

---

### **7. HTTP Client Error Handling Example**  
Handles HTTP errors globally.

```ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { catchError } from 'rxjs/operators';
import { throwError, Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private API_URL = 'https://api.example.com';

  constructor(private http: HttpClient) {}

  getData(): Observable<any> {
    return this.http.get(`${this.API_URL}/data`).pipe(
      catchError(this.handleError)
    );
  }

  private handleError(error: HttpErrorResponse) {
    let errorMsg = 'An error occurred';
    if (error.error instanceof ErrorEvent) {
      errorMsg = `Client-side error: ${error.error.message}`;
    } else {
      errorMsg = `Server-side error: ${error.status} ${error.message}`;
    }
    return throwError(() => new Error(errorMsg));
  }
}
```

---

### **8. Subjects and Observables Example (Real-time Data)**  
A subject allows multiple components to subscribe to the same data stream.

```ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private dataSubject = new BehaviorSubject<string>('Initial Value');
  data$: Observable<string> = this.dataSubject.asObservable();

  updateData(newData: string) {
    this.dataSubject.next(newData);
  }
}
```

**Usage in a Component:**
```ts
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-data',
  template: `<p>Data: {{ data }}</p> <button (click)="changeData()">Update</button>`
})
export class DataComponent implements OnInit {
  data: string = '';

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.data$.subscribe(value => this.data = value);
  }

  changeData() {
    this.dataService.updateData('New Value');
  }
}
```

---

These examples cover the essential Angular features you requested. Let me know if you need any modifications or further explanations! 🚀